# SSL/TLS의 암호화, 3-Way 핸드셰이크

- 먼저 비대칭 암호화를 이용해 3-way 핸드쉐이킹을 하여 세션키를 공유한다
- 세션키 공유가 끝나면 대칭 암호화를 통해 빠르고 안전하게 데이터를 교환한다

---

# **핵심 원리**

## **비대칭 암호화**:

- 느리지만, 세션 키 교환에만 사용되어 보안을 강화.
- 서버의 공개키를 사용해 안전하게 세션 키를 공유.

## **대칭 암호화**:

- 빠르고 효율적이어서, 이후 데이터 전송에 활용.
- 클라이언트와 서버는 동일한 세션 키를 공유하여 데이터의 기밀성과 무결성을 보장.

## **세션 키(Session Key, 대칭 키)**

- 세션 키는 **대칭 암호화** 알고리즘에서 사용하는 암호화 키
- **클라이언트와 서버가 동일한 키를 공유하여 데이터를 암호화하고 복호화**하는 데 사용된다.
- 세션 키는 일반적으로 한 세션 동안만 유효하며, 세션이 종료되면 폐기된다.

---

# **비대칭 암호화를 이용한 세션 키 교환 과정 (3-way hand shake)**

- 비대칭 암호화는 **공개키(Public Key)**와 **개인키(Private Key)**를 사용하여 데이터를 암호화/복호화하는 방식이다.
- TLS/SSL에서는 이를 통해 안전하게 **세션 키(Session Key)**를 교환한다.

## 1. **클라이언트가 공개키 요청 (ClientHello)**

- 클라이언트가 서버에 연결을 요청하면서 Handshake 과정을 시작한다 (**ClientHello** 메시지).

## 2. **서버가 공개키 제공 (ServerHello)**

- 서버는 자신의 **공개키**와 **디지털 인증서**(SSL 인증서)를 클라이언트에게 보낸다 (**ServerHello**).
- 클라이언트는 서버의 인증서를 확인하여 신뢰성을 검증한다.
    - 신뢰 검증: 인증서가 CA(Certificate Authority)로부터 발급되었는지 확인.
    - 인증서에 포함된 서버의 공개키 사용.

## 3. **클라이언트가 세션 키 생성**

- 클라이언트는 **세션 키(대칭키)**를 생성.
- 생성한 세션 키를 서버의 **공개키**로 암호화하여 서버에 전송.

## 4. **서버가 세션 키 복호화**

- 서버는 자신의 **개인키**로 클라이언트가 보낸 데이터를 복호화하여 세션 키를 얻는다.

---

# **대칭 암호화를 이용한 데이터 교환**

- 대칭 암호화는 클라이언트와 서버가 **동일한 세션 키**를 사용하여 데이터를 암호화/복호화한다.
- 세션 키는 위의 비대칭 암호화 과정을 통해 안전하게 교환되었기 때문에, 대칭 암호화 단계는 빠르고 효율적이다.

## 1. **데이터 암호화**

- 클라이언트가 데이터를 세션 키를 사용해 암호화하여 서버로 전송.
    - 예: `암호화된 데이터 = AES(세션 키, 평문 데이터)`

## 2. **데이터 복호화**

- 서버는 동일한 세션 키를 사용해 데이터를 복호화.
    - 예: `복호화된 데이터 = AES(세션 키, 암호화된 데이터)`

---

# **정리된 흐름**

## **1. 비대칭 암호화**:

- 클라이언트가 서버 공개키로 암호화한 세션 키를 전송.
- 서버는 개인키로 복호화하여 세션 키를 확보.

## **2. 대칭 암호화**:

- 클라이언트와 서버는 공유된 세션 키로 데이터를 암호화/복호화.
- 데이터 전송은 빠르고 안전하게 이루어진다.

# 요약

- 먼저 비대칭 암호화 상태에서 세션 키를 공유함
- 같은 세션 키를 가지게 되면, 그 후로는 해당 세션 키를 이용해 대칭 암호화로 데이터 통신을 진행함
- 핸드쉐이크
    - 1단계 Client hello: 클라이언트가 서버에 인증 요청
    - 2단게 Server hello: 서버가 본인의 공개 키와 인증서를 클라이언트에 전달
    - 3단계: 클라이언트가 세션 키를 생성하고, 서버에게서 받은 공개키로 세션 키를 암호화해서 서버에 전달함
    - 4단계: 서버가 개인 키로 암호화를 풀어서 세션키를 획득함