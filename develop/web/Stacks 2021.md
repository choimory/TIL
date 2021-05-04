# Front

## Markup

- HTML

## Stylesheet

- CSS
    - CSS Framework
        - Bootstrap
        - Bulma
        - Foundation
- SASS
- LESS
- Styluss
- Compass

## Script

- Javascript
    - Framework
        - backbone.js : 옛날에 쓰이던것 중 하나.
        - jQuery : Javascript 로직을 메소드로 묶어 간단히 사용케 해주는 라이브러리 정도. 현재 업계 표준이지만 슬슬 내리막.
        - Angular : 옛날 실무 사용률 1위.
        - Vue : 가장 최신, 간단하고 쉽고 핫함, jQuery 유사 기능 제공, 템플릿 제공
        - React : 깊이있는 js 프레임워크, jQuery 다음주자 1순위, 템플릿 제공(JSX)
        - Typescript : 자료형이 있는 Javascript
    - Library
        - 빌드 도구
            - Grunt / gulp : Javascript 빌드 도구(Task runner). 과거 Grunt, 현재 Gulp. 역할은 Task들을 Concat하여 병합, 자동화 하는 정도.
            - Web pack : Javascript 리소스 모듈 번들러. 리액트 진영 주력 사용. Grunt, Gulp보다 강력하지만 복잡. 기능 중 Task runner의 기능이 포함되어 있어 Task runner+@의 역할을 수행.
            - Babel : ECMAScript 상하위 버전 호환해주는 JavaScript 트랜스 컴파일러 (빌드시 ES6코드를 ES5코드로 자동변경 등)
        - 테스트 프레임워크 및 코드 분석 도구
            - ESLint : 정적 코드 분석
            - JSLint : 정적 코드 분석
            - Mocha : 테스트 프레임워크
            - Jest : 테스트 프레임워크

## Layout Template Engine

> 지정된 페이지 레이아웃 틀에 따라 페이지 타일을 조립하여 완전한 페이지로 렌더링해줌.

- Sitemesh
- Tiles

## Template Engine

> 데이터를 출력하거나 조건문, 반복문 등을 제공하여 상황에 따라 변화된 결과를 렌더링하게 해줌.

- JSP
- Thymeleaf
- Handlebars
- EJS
- Pug (Jade)
- Dust
- Hogan.js
- Twig
- Vash

---

# Back

## Java / Kotlin

- Framework
    - Struts
    - Spring
    - Spring boot
- Persistence Layer
    - ORM
        - Mybatis
    - JPA
        - Hibernate
        - Spring Data JPA
            - querydsl : JPA에서 복잡한 쿼리를 사용해야 할때, querydsl을 사용하던가 Mybatis를 병행하여 사용함.
- Build tool
    - Maven
    - Gradle
- Library
    - Spring Batch : 대용량 데이터 배치 처리 프레임워크
    - Spring quartz : 스케줄링 프레임워크
    - Kafka : 대량 메시지 처리 시스템
    - Kibana : 모니터링
    - API 문서화 라이브러리 (작성한 API 메소드를 자동 문서화해줌)
        - Spring Rest Docs : 코드에 영향 없음, 테스트 성공시 문서생성, 문서가 깔끔함, 적용이 어려움
        - Swagger : API 테스트 화면 제공, 적용이 쉬움, 코드에 어노테이션 추가해야함, 동기화 안될 수 있음
    - 테스트 라이브러리
        - JUnit : 기본 제공 라이브러리, 컴파일 시점에 오류를 잡기 쉬움, 동적 테스트 작성이 불편,
            - xUnit : CUnit, JUnit, PyUnit 등등 언어별로 유닛테스트 라이브러리 시리즈가 있는데 이걸 통틀어 xUnit이라 부름.
        - Spock : BDD 스타일로 직관적, 동적 테스트가 쉬움, 라이브러리 추가 필요, 컴파일 시점 오류 찾기 어려움.
        - Mockito : Mock 테스트

## Javascript

- Node.js : 구조상 비동기 요청처리에 좋아서 restful api에 적합함. 비동기로 이루어지는 spa나 동영상스트리밍 사이트 등에 사용.
    - Express.js : express.js는 Node.js의 웹 프레임워크. Node.js 프로젝트만으로는 제공되는 기본 구조가 아예 없어서 같이 사용됨. (node.js+express.js = Java+Spring)
    - Nest.js : Express.js가 백엔드만 제시해주는 프레임워크라면, Nest.js는 프론트까지 제시해주는 풀스택 프레임워크. (Express.js + angular.js = Nest.js)

## Python

- Framework
    - Django
    - Flask
- Library
    - Tensorflow : 예, 아니오에 대한 기준점을 정해주고 데이터를 많이 밀어넣으면 스스로 학습하며 구분해나감

## PHP

- Framework
    - Code Igniter
    - Laravel

# Ruby

> 일본에서 만든 언어. 일본에서 잘나감.

- Framework
    - Ruby on Rails

# Groovy

- Framework
    - Grails : Spring boot를 기반으로 하는 Groovy 웹앱 프레임워크

# Go

---

# DB

## RDBMS

- Oracle
    - Tibero
    - Altibase
    - CUBRID
- MySQL / MariaDB
- MSSQL
- PostgreSQL
- SQLite
- HSQL
- H2
- AWS RDS : AWS의 RDBMS. MariaDB, MySQL, Oracle등의 주요 DB를 선택할 수 있음.

## NoSQL

### Document

- MongoDB
- AWS Document DB

### Key-Value

- Redis
- AWS Dynamo DB

### Column

- Cassandra
- AWS keyspaces

### Graph

- GraphQL
- AWS Neptune

# Network

- TCP / IP
- UDP
- ftp
- host
- domain
- DNS
- http https tls
- VPN
- 내부망(내부 네트워크) 사설망(사설 네트워크)

---

# Server

> 작성한 앱을 배포하는 서버 컴퓨터.
서버 > OS > 미들웨어(FTP서버, 웹서버, WAS, DB서버, 메일서버....) > 애플리케이션의 구조.

## OS

- Linux
    - Devian
        - Ubuntu
    - Redhat
        - CentOS
    - Docker + Kubernetes : 하나의 Linux 서버 컴퓨터에서 여러 앱을 효율적으로 관리, 배포하게 해주는 리눅스 가상 컨테이너 기술
- Windows
    - Windows Server : 개구림 쓰레기

## Web server

> http 요청을 받으면 미리 작성된 파일을 결과로 응답하는 정적인 서버.
주로 http/https 통신 + 디스크에 보관된 html, css, js등의 파일을 송수신 하는 역할.
서버 컴퓨터에 사용. 서버컴퓨터 대부분 리눅스이므로 주로 리눅스 기반.
주로 서버 미들웨어에 웹서버와 WAS를 같이 배치하여, 웹서버는 정적인 처리 담당, WAS는 동적인 처리 담당으로 용도를 나누어 분산하여 활용함.
웹서버가 앞선에서 정적인 처리를 먼저 해주고 동적인 처리만 WAS로 보낸다.

- nginx - 리눅스. apache httpd가 무겁고 느려져서 탄생. 요즘 apache httpd에서 갈아타는 추세.
- ms iis - 마이크로 소프트꺼다보니 윈도우즈 서버의 대표 웹서버
- apache http server (apache httpd)
- webtob - 국산. 티맥스 소프트. 국산이다 보니 공공기관에서 활용 장려. 티맥스에서 유지보수 해주며 서버 장애시 책임도 티맥스가 처리함.
- netscape
- google web server

### CDN

> 정적 리소스 파일의 효율적인 관리 외

- CloudFlare : CDN, DDOS 공격 방어,  DNS 등등의 서비스 제공

## WAS

> 웹어플리케이션 서버.
웹서버는 요청을 받으면 파일을 결과로 응답하는 정적인 서버.
WAS는 데이터를 가공하는 연산과 비즈니스 로직을 수행하여 웹페이지를 재조립해 동적인 페이지를 응답하게 함.
주로 서버 미들웨어에 웹서버와 WAS를 같이 배치하여, 웹서버는 정적인 처리 담당, WAS는 동적인 처리 담당으로 용도를 나누어 분산하여 활용함.
웹서버가 앞선에서 정적인 처리를 먼저 해주고 동적인 처리만 WAS로 보낸다.
대부분의 WAS는 Java가 제공하는 Java EE (J2EE)로 구현되어 있다

- Tomcat (명확하게는 웹서버보다는 높고 WAS보다는 낮은 웹컨테이너 정도지만, WAS가 하는 기본적인것들은 수행가능해 WAS로도 많이 인식함)
- Weblogic : Oracle사의 WAS
- Jeus : 국산. 티맥스 소프트. 국산이다 보니 공공기관에서 활용 장려. 티맥스에서 유지보수 해주며 서버 장애시 책임도 티맥스가 처리함.
- JBoss
- Websphere

## CI (Continuous Integration, 지속적 통합) / CD (Continuous Deployment 혹은 Delivery, 지속적 배포)

> CI : 원격 저장소에 Push 발생시, 변경사항을 자동 빌드 및 테스트하여 Merge하고 알림 메시지를 발송함. 빌드 및 테스트 실패시 Push 거절.

> CD : VCS의 특정 이벤트에 대해 트리거를 걸어, 이벤트 발생시 코드를 운영 서버로 자동 배포함. (예 : Git의 master Branch에 Push 되었을시 변경된 master Branch의 코드를 자동 배포)

- Jenkins
- Travis CI
- Github action
- Jetbrain Team city

---

# Cloud Server

> 하드웨어 구축없이 외부 클라우드 서비스를 이용하여 서버를 구축함.
주요 클라우드 서비스는 Amazone AWS, Microsoft Azure가 있음.

## AWS

### OS

- AWS EC2
- AWS lightsail

### 파일 서버 (저장소)

> 파일 저장서버를 별도로 분리하고 싶을때

- AWS S3

### DB 서버

- AWS RDS
- AWS Dynamo DB
- AWS Document DB
- AWS keyspaces

### CDN

> 정적 리소스파일의 효율적인 관리 외

- AWS CloudFront

### CI / CD

- AWS Code Deploy
- AWS Code Delivery

### 그 외

- AWS Lambda : 앱 없이 실행하는 코드. 특정 이벤트에 대한 트리거를 걸어서 해당 이벤트 발생시 실행할 코드만 작성하면 해당 코드 로직이 실행됨. 이러한 서버 없이 동작하는 시스템을 FaaS(Function as a Service), Serverless라고 함.
- AWS Cloud9 : 온라인 IDE

---

# Tool

---

## DB Connection Tool

- SQL Developer : 오라클 공식 지원 도구
- MySQL Workbench : MySQL 공식 지원 도구
- Toad for ... :
- DBeaver

## VCS

### Git

- Gitlab
- Github

### SVN

- TortoiseSVN

## ETC

- Atlassian Jira : 이슈 트래커
- Atlassian Confluence : 팀 업무

# E.T.C

---