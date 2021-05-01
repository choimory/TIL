# frontend

## 퍼블리싱

- HTML
    - CSS
        - CSS 프레임워크
            - Bootstrap
            - Bulma
            - Foundation
    - SASS : CSS에서 발전된 형태의 최신 스택

## Javascript

- 라이브러리
    - Vanilla JS : Pure Javascript를 뜻함. 순수 자바스크립트가 jQuery보다 낫다는 얘기를 순수 자바스크립트 대신 Vanilla JS라는 라이브러리 내지 프레임워크로 포장하여 있어보이게 설명함.(=튜닝의 끝은 순정이다)
    - Typescript : 자료형이 있는 Javascript
    - jQuery : js로직을 메소드로 묶어 간단히 사용케 해주는 라이브러리 정도. 현재 업계 표준이지만 슬슬 내리막
    - angular.js : 옛날 실무 사용률 1위.
    - vue.js : 가장 최신, 간단하고 쉽고 핫함, jQuery 유사 기능 제공, 템플릿 제공
    - react : 깊이있는 js 프레임워크, jQuery 다음주자 1순위, 템플릿 제공(JSX)
    - backbone.js : 옛날에 쓰이던것 중 하나.
- Javascript 빌드도구
    - Grunt / gulp :  자바스크립트 빌드도구(task runner). 과거엔 Grunt가 대세였으나 지금은 gulp. 하는 역할은 task들을 concat해서 병합, 자동화하는 정도.
    - webpack : 자바스크립트 리소스 모듈화 도구(bundler). react진영에서 주로 사용. grunt/gulp보다 강력하지만 좀 더 복잡. 기능 중 task runner의 기능도 포함되어있어 task runner+@의 역할을 수행가능.
    - babel : ECMAScript 상하위 버전 호환해주는 JavaScript 트랜스 컴파일러 (빌드시 ES6코드를 ES5코드로 자동변경 등)
- ESLint, JSLint... : 문법 오류시 오류를 강제진행 안하고 오류 표시 시키게 하는 정적 코드 분석 도구

## 템플릿 엔진

### 레이아웃 템플릿 엔진

지정된 페이지 레이아웃 틀에 따라 페이지 타일을 조립하여 완전한 페이지로 렌더링해줌.

- Tiles
- Sitemesh

### 텍스트 템플릿 엔진

데이터를 출력하거나 조건문, 반복문 등을 제공하여 상황에 따라 변화된 결과를 렌더링하게 해줌.

예) JSP의 EL({value}), JSTL(<c:if...>)

### spring legacy 템플릿 엔진

- jsp
- thymeleaf
- groovy
- freemarker  - 업데이트 중단됨
- velocity - 4.3부터 사용 중단됨
- jade4j
- jmustache
- pebble
- handlebars

### spring boot 템플릿 엔진

- mustache
- thymeleaf : Spring이 Spring boot용 템플릿 엔진으로 thymeleaf를 밀어주고 있음
- groovy
- handlebars
- freemarker - 업데이트 중단됨
- 그 외 : velocity는 springboot 미지원, freemarker는 업데이트 중단됨, jsp는 springboot에서 사용에 제한사항이 따름

### javascript 템플릿엔진

- JSP
- Thymeleaf
- velocity
- mustache
- handlebars
- Freemarker
- groovy
- jtwig
- JSX : React 제공 템플릿
- jade : node.js 템플릿 엔진

# backend

- java
    - servlet
        - spring legacy
            - maven
            - mybatis, ibatis
            - rdbms
        - spring boot
            - gradle
            - Spring JPA
                - Hibernate : JPA 하위 종류 중 하나 (JPA 인터페이스를 구현한 기능)
                - Spring Data JPA : JPA 관련 기능을 쉽게 쓰게 도와주는 모듈
            - nosql
- Groovy : 아파치의 프로그래밍 언어
    - Grails : Spring boot를 기반으로 하는 groovy 웹앱 프레임워크
- Javascript
    - node.js : 구조상 비동기 요청처리에 좋아서 restful api에 적합함. 비동기로 이루어지는 spa나 동영상스트리밍 사이트 등에 사용
        - express.js : express.js는 node.js의 웹 프레임워크 (node.js+express.js = Java+Spring)
        - nest.js : express.js가 백엔드만 제시해주는 프레임워크라면, nest.js는 프론트까지 제시해주는 풀스택 프레임워크. (express.js + angular.js = nest.js)
- python
    - django
        - django template : 장고생태계의 jsp같은 파이선 장고 템플릿 엔진
- Ruby
    - Ruby on Rails

# DB

## RDBMS

- oracle, tibero : 오라클류
- mysql, mariadb : mysql류, 오픈소스 db
- mssql
- postgresql
- tibero, cubrid, altibase : 국산 db
- cubrid : 국산 오픈소스 db
- AWS rds : AWS의 RDBMS. mariadb, mysql, oracle등의 주요db를 선택할 수 있음

## NoSQL

- mongodb : 문서형
- AWS dynamodb
- redis : 키-밸류형

# Network

- tcp ip
- ftp
- udp
- host
- domain
- http https tls
- dns
- VPN
- 내부망(내부 네트워크) 사설망(사설 네트워크)

# Server

작성한 앱을 배포하는 서버 컴퓨터.

서버 > OS > 미들웨어(FTP서버, 웹서버, WAS, DB서버, 메일서버....) > 애플리케이션의 구조.

- AWS EC2 : 서버 컴퓨터를 직접 사는게 아니라 아마존의 서버 컴퓨터를 대여해서 사용하는 시스템 (클라우드 컴퓨팅)
- aws lightsail : EC2의 간소화

## OS

작성한 앱을 배포하는 서버 컴퓨터의 운영체제

- linux : 서버 컴퓨터 대부분이 리눅스를 OS로 사용. 오픈소스라서 많은 파생형이 있음
    - 데비안계열 :
        - ubuntu : 사용자용으로 인기. 쉽고 편리. 활성화된 커뮤니티
    - 레드햇계열 :
        - centOS : 서버용으로 인기. 안정적. 제어패널 지원
        - fedora : 유료
    - docker + kubernetes : 하나의 리눅스 서버컴퓨터에서 여러 앱을 효율적으로 관리, 배포하게 해주는 리눅스 가상 컨테이너 기술
    - AWS ECS + EKS
        - AWS ECS : 아마존의 EC2를 docker 컨테이너로 구축
        - AWS EKS : 아마존의 완전관리형 kubernetes  제어 서비스