# TODO

- JWT 구조
- JWT 인증방식
- JWT 사용 이유
- JWT 생성 및 검증
- JWT 401 핸들링 클래스 작성
- JWT 403 핸들링 클래스 작성
- Spring Security 구조
- Spring Security 권한 적용하기

---

# 들어가며

# JWT 관련 선수지식

- JWT는 Header.Payload.Signature 세가지로 구성되어 있으며 .으로 구분함
    - e.g. aaa.bbb.ccc

# 스프링 시큐리티 관련 선수지식

- filter에서 권한을 확인하고 처리해줄수 있음

# 작성 및 적용

## 토큰 관리 클래스

## 토큰 필터 클래스

## 401 오류 처리 핸들링 & 403 오류 처리 핸들링용 클래스

## JWT 전용 회원 조회 관련 비즈니스 레이어 클래스

## JWT 관련 스프링 시큐리티 분리 클래스 

## 스프링 시큐리티 적용

# 참고

- https://velog.io/@jkijki12/Spirng-Security-Jwt-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0
- https://velog.io/@shinmj1207/Spring-Spring-Security-JWT-%EB%A1%9C%EA%B7%B8%EC%9D%B8
- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-jwt/unit/65764
  
- https://00hongjun.github.io/spring-security/securitycontextholder/ 
- https://irrte.ch/jwt-js-decode/