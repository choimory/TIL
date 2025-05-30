# API에서 토큰의 유효기간을 어떻게 설정하는가

- API에서 토큰의 유효기간은 인증 및 권한 부여의 보안을 강화하고 시스템의 효율성을 유지하기 위해 설정.
- 토큰 유효기간은 클라이언트의 접근 권한을 제한적으로 부여하며, 만료된 토큰은 재사용을 방지해 보안 위험을 줄이는 데 기여.

---

# 토큰의 유형과 유효기간 설정 방식

## **Access Token (액세스 토큰)**

- 클라이언트가 보호된 리소스에 접근할 때 사용하는 짧은 수명의 토큰.
- 일반적으로 **수 분에서 수 시간**의 짧은 유효기간 설정.
- 유효기간이 짧아 토큰이 탈취되어도 악용 가능 기간이 제한적.

## **Refresh Token (리프레시 토큰)**

- 만료된 액세스 토큰을 갱신하기 위한 장기 유효기간의 토큰.
- 일반적으로 **수 일에서 수 주**의 긴 유효기간 설정.
- 리프레시 토큰의 사용은 추가적인 인증 검증과 함께 이루어져야 함.

---

# 토큰 유효기간 설정 방법

## **JWT (JSON Web Token)의 `exp` 클레임 사용**

- JWT는 자체적으로 만료 시간을 지정할 수 있는 `exp`(expiration) 클레임을 제공.
- `exp` 필드는 유닉스 타임스탬프(초 단위)로 만료 시점을 나타냄.

**JWT 생성 예시 (Node.js)**

```jsx
const jwt = require('jsonwebtoken');

// Access Token 생성 (1시간 유효)
const accessToken = jwt.sign(
  { userId: 123 },
  'secretKey',
  { expiresIn: '1h' } // 1시간
);

// Refresh Token 생성 (7일 유효)
const refreshToken = jwt.sign(
  { userId: 123 },
  'refreshSecretKey',
  { expiresIn: '7d' } // 7일
);

console.log(accessToken, refreshToken);
```

## **데이터베이스 기반 유효기간 관리**

- 토큰 생성 시 발급 시간(`issued_at`)과 만료 시간(`expiration`)을 데이터베이스에 저장.
- 요청 시 토큰의 만료 여부를 확인하여 처리.

**예시 (SQL)**

```sql
CREATE TABLE tokens (
    token_id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    token VARCHAR(255) NOT NULL,
    issued_at TIMESTAMP NOT NULL,
    expires_at TIMESTAMP NOT NULL
);

-- 토큰 저장
INSERT INTO tokens (user_id, token, issued_at, expires_at)
VALUES (123, 'abc123token', NOW(), NOW() + INTERVAL '1 hour');
```

## **Redis 기반 토큰 만료 관리**

- Redis와 같은 캐시 시스템을 사용해 토큰과 유효기간을 저장.
- Redis의 TTL(Time to Live) 기능을 사용해 만료된 토큰 자동 삭제.

**Redis 사용 예시**

```jsx
const redis = require('redis');
const client = redis.createClient();

// Access Token 저장 (1시간 유효)
client.setex('accessToken:abc123', 3600, 'user123');

// Refresh Token 저장 (7일 유효)
client.setex('refreshToken:def456', 604800, 'user123');
```

---

# 토큰 유효기간 설정 시 고려사항

## **보안 강화**

- 짧은 유효기간으로 설정해 탈취된 토큰의 위험을 최소화.
- 예: 액세스 토큰은 15분~1시간, 리프레시 토큰은 ~730일 설정.

## **사용자 편의성**

- 너무 짧은 유효기간은 사용자 경험에 불편을 초래할 수 있음.
- 예: 리프레시 토큰을 활용해 긴 세션 유지와 보안 간 균형을 맞춤.

## **토큰 재사용 방지**

- 리프레시 토큰 사용 시, 한 번 사용된 토큰은 즉시 폐기(서버 측 상태 저장).

## **환경에 따른 조정**

- 민감한 데이터 접근 시 유효기간을 더 짧게 설정.
- 내부 서비스 API는 더 긴 유효기간 설정 가능.

---

# 유효기간이 만료된 토큰 처리

## **액세스 토큰 만료 시**

- 클라이언트는 만료된 액세스 토큰을 리프레시 토큰과 함께 서버에 제출하여 새 액세스 토큰 요청.
- 리프레시 토큰의 유효성을 확인한 후 새로운 액세스 토큰 발급.

## **리프레시 토큰 만료 시**

- 클라이언트는 재인증(로그인)을 통해 새로운 리프레시 토큰 발급.

**액세스 토큰 갱신 API 예시**

```jsx
app.post('/auth/refresh', (req, res) => {
    const { refreshToken } = req.body;

    try {
        // 리프레시 토큰 검증
        const decoded = jwt.verify(refreshToken, 'refreshSecretKey');

        // 새로운 액세스 토큰 발급
        const newAccessToken = jwt.sign(
            { userId: decoded.userId },
            'secretKey',
            { expiresIn: '1h' }
        );

        res.json({ accessToken: newAccessToken });
    } catch (err) {
        res.status(401).json({ error: 'Invalid or expired refresh token' });
    }
});
```

---

# 장점

- 보안 강화: 토큰 유효기간으로 탈취 위험 완화.
- 시스템 효율성: 짧은 유효기간으로 오래된 토큰을 정리해 리소스 낭비 방지.
- 사용자 편의성: 리프레시 토큰 활용으로 세션 유지와 보안 간 균형 제공.

# 단점

- 관리 복잡성: 유효기간 설정, 토큰 갱신 로직, 만료된 토큰 폐기 등이 추가 개발 필요.
- 성능 부담: 짧은 유효기간으로 인해 토큰 갱신 요청이 빈번하게 발생 가능.

---

# **요약**

- API에서 토큰 유효기간은 `exp` 클레임, 데이터베이스, 또는 Redis를 활용해 설정하며, 액세스 토큰은 짧게(15분~1시간), 리프레시 토큰은 길게(~730일) 설정하는 것이 일반적.
- 보안을 강화하면서도 사용자 경험을 고려한 유효기간 설계가 중요.