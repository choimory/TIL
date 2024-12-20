# DNS란

- DNS(Domain Name System)는 **도메인 이름을 IP 주소로 변환**해주는 시스템이다.
- 웹사이트에 접근할 때 우리가 사용하는 도메인 이름(예: www.example.com)은 사람이 기억하기 쉽게 만들어졌지만, 컴퓨터는 **IP 주소**(예: 192.168.0.1)로 통신한다.
- DNS는 도메인 이름을 컴퓨터가 이해할 수 있는 IP 주소로 변환하여 사용자가 웹사이트에 쉽게 접속할 수 있도록 해주는 역할을 한다.

---

# DNS의 주요 기능:

## **도메인 이름과 IP 주소 매핑**:

- 도메인 이름을 입력하면, DNS가 해당 도메인에 매핑된 IP 주소를 찾아 제공한다.

## **분산된 데이터베이스**:

- DNS는 전 세계에 분산된 서버들로 구성된 네트워크다.
- 하나의 중앙 서버가 아닌, 여러 DNS 서버들이 역할을 나누어 처리한다.

## **계층 구조**:

- DNS는 계층적인 구조로 되어 있으며, 최상위 도메인(TLD, 예: .com, .org 등), 2차 도메인(예: example.com) 등의 계층으로 이루어진다.

---

# DNS의 동작 방식:

## **DNS 쿼리**:

- 사용자가 브라우저에 도메인 이름을 입력하면, 해당 도메인의 IP 주소를 찾기 위해 DNS 쿼리가 발생한다.

## **DNS 리졸버**:

- 사용자의 컴퓨터는 DNS 리졸버(해결자)에게 쿼리를 보내고, 리졸버는 여러 DNS 서버를 통해 해당 도메인의 IP 주소를 찾는다.

## **DNS 서버 탐색**:

- DNS 리졸버는 먼저 **캐시**된 정보를 확인하고, 없을 경우 **최상위 도메인 서버**, **권한 있는 DNS 서버** 등을 차례대로 탐색하며 IP 주소를 찾는다.

## **IP 주소 반환**:

- 해당 도메인의 IP 주소가 확인되면, DNS 리졸버는 이를 사용자에게 반환하고, 사용자는 해당 IP 주소로 웹사이트에 접속한다.

---

# DNS의 계층 구조:

## **루트 DNS 서버**:

- DNS의 최상위 계층으로, 각 최상위 도메인(TLD)에 대한 정보를 가지고 있다.

## **TLD 서버**:

- .com, .org 같은 최상위 도메인에 대한 정보를 저장하는 서버.

## **권한 있는 DNS 서버**:

- 특정 도메인에 대한 실제 IP 주소를 저장하고 있는 서버.

---

# DNS의 주요 역할:

## **도메인 이름 해석**:

- 사람이 이해할 수 있는 도메인 이름을 컴퓨터가 처리할 수 있는 IP 주소로 변환한다.

## **부하 분산**:

- 웹 서버에 대한 부하를 줄이기 위해 여러 IP 주소를 할당하여 트래픽을 분산시키는 기능도 제공한다.

## **보안 기능**:

- DNSSEC(DNS Security Extensions) 같은 보안 확장을 통해 DNS 데이터의 무결성을 보장할 수 있다.

---

# 결론

- DNS는 인터넷의 전화번호부 같은 역할을 하며, 사용자가 도메인 이름을 쉽게 기억하고 사용할 수 있게 해준다.
- 이 시스템을 통해 도메인 이름과 IP 주소 간의 매핑이 원활히 이루어지며, 인터넷 상의 서비스와 리소스에 빠르고 정확하게 접근할 수 있다.