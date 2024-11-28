# 개요

- HTTPS -> HTTP 로의 통신을 할때 발생

# 문제사항

- HTTPS 도메인 적용된 프론트에서, HTTP 백엔드로 요청을 보낼때 Mixed Content 오류가 발생

# 왜?

- HTTPS는 HTTPS간의 통신만 가능하다
- HTTP는 HTTPS, HTTP 모두와 통신이 가능하다