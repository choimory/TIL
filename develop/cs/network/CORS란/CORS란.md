# CORS란

- CORS(Cross-Origin Resource Sharing)는 브라우저 보안 정책인 **동일 출처 정책(Same-Origin Policy)**을 우회하여, 서로 다른 출처(Origin)에서 리소스를 안전하게 공유할 수 있도록 하는 메커니즘이다.
- *출처(Origin)*는 다음 세 요소로 정의된다:
    1. 프로토콜 (예: `http`, `https`)
    2. 도메인 (예: `example.com`)
    3. 포트 (예: `8080`)
- **문제 발생 배경**:
    - 기본적으로 브라우저는 보안상의 이유로 한 출처에서 다른 출처로 리소스를 요청하는 것을 차단한다.
    - CORS는 이러한 제한을 완화하여, 필요한 경우 교차 출처 요청을 허용한다.

---

# CORS의 동작 원리

## **단순 요청(Simple Requests)**

- 조건: 특정 HTTP 메서드(`GET`, `POST`, `HEAD`)와 헤더만 사용.
- 서버가 요청을 받으면, 응답 헤더에 `Access-Control-Allow-Origin`을 포함하여 허용 여부를 명시.

**응답 예시**:

```arduino
Access-Control-Allow-Origin: https://example.com
```

## **프리플라이트 요청(Preflight Requests)**

- 조건: 단순 요청 조건을 벗어나는 요청(`PUT`, `DELETE` 등 메서드 사용, 커스텀 헤더 포함).
- 브라우저가 실제 요청 전에 `OPTIONS` 메서드를 사용해 서버의 허용 여부를 확인.
- 서버는 허용된 메서드와 헤더를 응답으로 반환.

**프리플라이트 요청 예시**:

```mathematica
OPTIONS /resource HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-Custom-Header
```

**프리플라이트 응답 예시**:

```mathematica
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: X-Custom-Header
```

## **자격 증명(Credentials)**

- 쿠키나 인증 정보를 포함한 요청은 서버가 추가적으로 `Access-Control-Allow-Credentials: true`를 응답해야 허용.

---

# CORS 문제 해결 방법

## **서버 측 설정**

서버가 클라이언트의 교차 출처 요청을 허용하도록 적절한 응답 헤더를 설정해야 한다.

- `Access-Control-Allow-Origin`: 허용할 출처(예: `https://example.com`).
    - 모든 출처를 허용하려면: `` 사용(단, 자격 증명 요청 시 불가능).
- `Access-Control-Allow-Methods`: 허용할 HTTP 메서드(예: `GET, POST, OPTIONS`).
- `Access-Control-Allow-Headers`: 허용할 요청 헤더(예: `Content-Type, Authorization`).
- `Access-Control-Allow-Credentials`: 인증 정보 포함 여부 설정(`true`).

**Express 예시**:

```jsx
const express = require('express');
const app = express();

app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', 'https://example.com'); // 특정 도메인만 허용
    res.header('Access-Control-Allow-Methods', 'GET,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type,Authorization');
    res.header('Access-Control-Allow-Credentials', 'true');
    next();
});

app.listen(3000, () => console.log('Server running'));
```

## **프록시 서버 사용**

- 프록시 서버를 설정해 클라이언트와 대상 서버 간의 중계 역할 수행.
- 클라이언트는 동일 출처 정책을 우회하여 프록시 서버와 통신.
- 프록시 서버는 대상 서버와 통신하여 데이터를 반환.

**예시**:

- Nginx를 이용한 프록시 설정:

    ```
    location /api/ {
        proxy_pass https://api.example.com/;
        add_header Access-Control-Allow-Origin *;
    }
    ```


## **브라우저 설정 변경 (개발 환경)**

- 브라우저 보안 설정을 일시적으로 비활성화.
- 예: Chrome에서 `-disable-web-security` 플래그를 사용.
- **주의**: 보안 위험이 크므로, 실제 환경에서는 사용하지 않아야 함.

## **CORS 미들웨어 사용 (Node.js)**

- Express와 같은 프레임워크에서 간단히 미들웨어를 활용.

**예시**:

```jsx
const cors = require('cors');
app.use(cors({
    origin: 'https://example.com',
    methods: 'GET,POST',
    credentials: true
}));
```

---

# CORS 해결 시 주의사항

## **출처 제어**

- 허용할 출처를 명확히 지정해 보안 위험 최소화.
- 필요 시 동적으로 출처를 확인해 설정.

## **민감 데이터 보호**

- 인증 정보 포함 요청 시 `Access-Control-Allow-Credentials`와 `Access-Control-Allow-Origin` 설정에 주의.

## **프리플라이트 요청 최적화**

- 응답 헤더에 `Access-Control-Max-Age`를 설정해 브라우저 캐싱을 활용하여 불필요한 프리플라이트 요청 방지.

---

# **요약**

- CORS는 브라우저의 동일 출처 정책을 우회해 안전하게 교차 출처 요청을 가능하게 하는 메커니즘이다.
- 문제 해결은 주로 서버의 응답 헤더 설정, 프록시 사용, 또는 개발 도구 활용을 통해 이루어진다.
- 서버 보안을 유지하면서 필요한 요청만 허용하도록 세심하게 관리해야 한다.