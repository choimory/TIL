# API 에러 처리의 중요성

- API 에러 처리는 클라이언트가 요청 실패 원인을 이해하고 적절히 대응할 수 있도록 명확하고 일관된 정보를 제공하는 과정.
- 잘 설계된 에러 처리 방식은 개발자 경험, 시스템 유지보수성, 사용자 경험을 크게 향상시킴.

---

# API 에러 처리의 베스트 프랙티스

## **HTTP 상태 코드 활용**

- HTTP 상태 코드는 요청의 성공 또는 실패 여부를 나타내는 기본 수단.
- 각 상황에 맞는 적절한 상태 코드를 사용하여 요청 결과를 명확히 전달.

**주요 상태 코드**

- **2xx (성공)**: 요청이 성공적으로 처리됨.
    - `200 OK`: 성공적인 요청 처리.
    - `201 Created`: 리소스가 성공적으로 생성됨.
    - `204 No Content`: 성공했으나 응답 본문이 없음.
- **4xx (클라이언트 에러)**: 요청에 문제가 있음.
    - `400 Bad Request`: 잘못된 요청 데이터.
    - `401 Unauthorized`: 인증이 필요함.
    - `403 Forbidden`: 권한 부족.
    - `404 Not Found`: 요청한 리소스를 찾을 수 없음.
    - `422 Unprocessable Entity`: 유효성 검증 실패.
- **5xx (서버 에러)**: 서버에서 요청을 처리하지 못함.
    - `500 Internal Server Error`: 서버에서 예기치 않은 오류 발생.
    - `503 Service Unavailable`: 서버가 현재 요청을 처리할 수 없음.

---

## **일관된 에러 응답 형식 설계**

- 에러 응답의 구조를 일관되게 유지하여 클라이언트가 예측 가능하게 설계.
- JSON 포맷을 사용하는 것이 일반적.

**예시: JSON 기반 에러 응답**

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "The 'email' field is required.",
  "details": [
    {
      "field": "email",
      "issue": "missing"
    }
  ],
  "timestamp": "2024-11-30T12:34:56Z"
}
```

**필수 필드**

- **status**: HTTP 상태 코드.
- **error**: 에러 유형 또는 이름.
- **message**: 사람이 읽을 수 있는 간단한 에러 설명.
- **details**: 추가적인 에러 정보(필요 시 포함).
- **timestamp**: 에러 발생 시간.

---

## **에러 코드 정의**

- 에러 코드를 정의하여 문제를 명확히 구분.
- 클라이언트가 에러 코드를 기반으로 대응 로직을 구현 가능.

**예시: 에러 코드 활용**

```json
{
  "status": 404,
  "error_code": "USER_NOT_FOUND",
  "message": "The requested user does not exist."
}
```

---

## **명확하고 간결한 에러 메시지 작성**

- 클라이언트가 이해하기 쉬운 언어로 에러 메시지를 작성.
- 기술적인 디버깅 정보는 숨기고, 필요한 경우 별도의 로깅 시스템에 저장.

**좋은 예**

```arduino
"The 'email' field is required."
```

**나쁜 예**

```arduino
"NullPointerException in line 42 of UserService.java"
```

---

## **클라이언트와 서버의 역할 분리**

- 클라이언트는 요청의 유효성을 1차적으로 검사하고, 서버는 최종적으로 검증.
- 서버는 항상 클라이언트가 조작할 수 있는 데이터를 신뢰하지 않도록 설계.

---

## **유효성 검사와 에러 처리 분리**

- 데이터 유효성 검사는 별도의 레이어에서 처리하고, 검증 실패 시 적절한 에러를 반환.

**예시: 유효성 검사 실패 응답**

```json
{
  "status": 422,
  "error": "Validation Error",
  "message": "The provided data is invalid.",
  "details": [
    {
      "field": "email",
      "issue": "Invalid format"
    },
    {
      "field": "password",
      "issue": "Too short"
    }
  ]
}
```

---

## **로깅과 모니터링 통합**

- 에러 발생 시 로그를 기록하여 문제 분석과 해결을 지원.
- Sentry, ELK(Stack), Datadog 등과 같은 도구를 사용해 실시간 에러 추적.

---

## **에러의 사용자화(Fallback 처리)**

- 클라이언트가 에러를 처리하지 못할 경우, 기본 응답을 제공하여 사용자 혼란 방지.
- 예: 서버 에러 발생 시 사용자에게 "일시적인 문제가 발생했습니다. 잠시 후 다시 시도해주세요" 메시지 반환.

---

## **조건부 요청과 캐싱 활용**

- 에러를 줄이기 위해 조건부 요청(ETag, If-None-Match 등)을 사용.
- 불필요한 요청을 방지하여 서버 부하를 감소.

---

## **재시도 및 백오프 전략 구현**

- 클라이언트는 일시적인 서버 에러(5xx) 발생 시, 재시도를 일정 횟수 진행.
- Exponential Backoff를 사용해 재시도 간격을 점진적으로 늘림.

**예시: 재시도 로직**

```jsx
function retryRequest(request, retries, delay) {
    return request().catch(error => {
        if (retries === 0) throw error;
        return new Promise(resolve => setTimeout(resolve, delay))
            .then(() => retryRequest(request, retries - 1, delay * 2));
    });
}
```

---

# 주요 사례

## **GitHub API**

- HTTP 상태 코드와 JSON 에러 메시지를 조합.
- 상세 에러 메시지와 문서를 함께 제공하여 디버깅 지원.

## **Stripe API**

- `type`, `message`, `code` 필드를 활용한 상세 에러 설명.
- 예:

    ```json
    {
      "error": {
        "type": "invalid_request_error",
        "message": "Missing required param: amount.",
        "code": "parameter_missing"
      }
    }
    ```


## **Google API**

- 다국어 지원 메시지와 표준화된 에러 응답 제공.
- 예:

    ```json
    {
      "error": {
        "code": 403,
        "message": "The user does not have permission to access the resource.",
        "errors": [
          {
            "message": "The user does not have permission to access the resource.",
            "domain": "global",
            "reason": "forbidden"
          }
        ]
      }
    }
    ```


---

# **요약**

- API 에러 처리는 HTTP 상태 코드, 일관된 응답 형식, 명확한 메시지, 에러 코드 정의를 통해 클라이언트가 문제를 쉽게 이해하고 해결하도록 설계.
- 유효성 검사, 로깅, 재시도 전략을 함께 활용하여 안정적이고 신뢰성 높은 API를 구축하는 것이 중요.