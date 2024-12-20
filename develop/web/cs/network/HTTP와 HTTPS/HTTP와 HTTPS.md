# HTTP와 HTTPS

- HTTP와 HTTPS의 차이점은 주로 **보안**과 **데이터 암호화**에 있다.

---

# HTTP (HyperText Transfer Protocol)

- **보안 없음**:
    - HTTP는 데이터를 암호화하지 않기 때문에 중간에 누군가가 데이터를 가로챌 경우, 내용이 그대로 노출될 수 있다.
- **속도 빠름**:
    - 데이터 암호화 과정이 없으므로 HTTPS에 비해 상대적으로 속도가 빠를 수 있다.
- **포트 번호 80**:
    - 기본적으로 포트 80을 사용한다.
- 사용사례:
    1. 민감한 정보가 포함되지 않은 웹 페이지나 단순한 데이터 전송에 주로 사용된다.

---

# HTTPS (HyperText Transfer Protocol Secure)

- **보안 제공**:
    - HTTPS는 SSL/TLS 프로토콜을 사용해 데이터를 암호화하여 전송한다.
    - 이를 통해 중간에 데이터가 가로채여도 해독할 수 없도록 보장한다.
- **속도 느림**:
    - 암호화 및 복호화 과정이 추가되기 때문에 HTTP보다 속도가 조금 느릴 수 있다.
- **포트 번호 443**:
    - 기본적으로 포트 443을 사용한다.
- 사용사례:
    1. 온라인 결제, 로그인 정보 등 민감한 데이터를 주고받는 웹사이트에서 사용되며, 보안이 중요한 모든 웹 서비스에서 권장된다.

---

# 비교

| **특징** | **HTTP** | **HTTPS** |
| --- | --- | --- |
| **보안** | 암호화 없음 | SSL/TLS로 암호화 |
| **속도** | 빠름 | 상대적으로 느림 |
| **포트 번호** | 80 | 443 |
| **인증서** | 인증서 필요 없음 | SSL/TLS 인증서 필요 |
| **사용 용도** | 민감하지 않은 데이터 전송 | 민감한 정보 전송, 보안 요구 서비스 |

---

# 결론

- HTTP와 HTTPS는 웹에서 사용되는 데이터 전송 프로토콜로, HTTPS는 HTTP에 비해 보안성이 뛰어나다.
- 특히, 개인 정보나 금융 정보처럼 중요한 데이터를 처리하는 웹사이트에서는 반드시 HTTPS를 사용해야 한다.
- 점차 모든 웹사이트가 보안을 강화하기 위해 HTTPS로 전환하는 추세이다.