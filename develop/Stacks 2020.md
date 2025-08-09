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

## FTP서버

파일 업로드 및 다운로드에 관련된 서버

- Filezilla - ftp클라이언트

## 웹서버

http 요청을 받으면 미리 작성된 파일을 결과로 응답하는 정적인 서버.

주로 디스크에 보관된 html, css, js등의 파일을 송수신 하는 역할.

서버 컴퓨터에 사용. 서버컴퓨터 대부분 리눅스이므로 주로 리눅스 기반.

주로 서버 미들웨어에 웹서버와 WAS를 같이 배치하여, 웹서버는 정적인 처리 담당, WAS는 동적인 처리 담당으로 용도를 나누어 분산하여 활용함. 웹서버가 앞선에서 정적인 처리를 먼저 해주고 동적인 처리만 WAS로 보낸다.

- nginx - 리눅스
- ms iis - 마이크로 소프트꺼다보니 윈도우서버의 대표
- apache http server
- webtob - 국산. 티맥스 소프트. 국산이다 보니 공공기관에서 활용 장려. 티맥스에서 유지보수 해주며 서버 장애시 책임도 티맥스가 처리함.
- netscape
- google web server

## WAS

웹어플리케이션 서버.

웹서버는 요청을 받으면 파일을 결과로 응답하는 정적인 서버.

WAS는 데이터를 가공하는 연산과 비즈니스 로직을 수행하여 웹페이지를 재조립해 동적인 페이지를 응답하게 함.

주로 서버 미들웨어에 웹서버와 WAS를 같이 배치하여, 웹서버는 정적인 처리 담당, WAS는 동적인 처리 담당으로 용도를 나누어 분산하여 활용함. 웹서버가 앞선에서 정적인 처리를 먼저 해주고 동적인 처리만 WAS로 보낸다.

대부분의 WAS는 Java가 제공하는 Java EE (J2EE)로 구현되어 있다

- tomcat (명확하게는 웹서버보다는 높고 WAS보다는 낮은 웹컨테이너 정도지만, WAS가 하는 기본적인것들은 수행가능해 WAS로도 많이 인식함)
- weblogic - 오라클의 was
- jeus - 국산. 티맥스 소프트. 국산이다 보니 공공기관에서 활용 장려. 티맥스에서 유지보수 해주며 서버 장애시 책임도 티맥스가 처리함.
- jboss
- websphere

# CI

소스코드를 지속적으로 통합(Continuous Integeration) 해주는 자동화 툴.

추가/변경된 코드를 자동으로 빌드 후 테스트해주며, 테스트를 통과한 경우에만 공유 리포지토리에 머지함.

여러 개발자가 동시에 코딩을 할때 충돌할 수 있는 문제를 해결해줌.

- jenkins : 자체 서버 컴퓨터가 필요함, 무료
- traivs ci : cloud시스템. 유료
- teamcity : jetbrain제품
- AWS CLI
- Github Action : 자동화툴

## CD

소스코드를 지속적으로 배포(Continuous Deployment) 해주는 자동화 툴.

리포지토리에 올라온 코드를 자동으로 배포해줌.

- AWS CodeDeploy
- AWS elastic beanstalk : AWS의 웹앱 배포 및 관리 서비스

# Cloud Computing

클라우드 시스템

## IaaS (Infrastructure as a Service)

서버, 네트워크. 전력 등의 하드웨어 관련 인프라를 제공함.

제공되는 가상머신에 네트워크 설정하고 하드웨어 설정하고 운영체제 설치해서 앱을 올려놓고 구동하고 모니터링함.

- AWS EC2

## PaaS (Platform as a Service)

IaaS에서 한번 더 추상화. 네트워크, 런타임까지 제공.

사용자는 앱만 배포하면 바로 구동 가능하게 함

- AWS Elastic beanstalk
- Azure AppService

## Serverless

### BaaS (Backend as a Service)

기능들을 API로 제공해줌으로서 백엔드개발 없이 필요기능을 쉽고 빠르게 구현

- Firebase

### FaaS

프로젝트를 함수단위로 쪼개거나 하나의 함수로 만들어, 서버에 함수를 등록하고 함수 호출횟수만큼 비용을 지불

- AWS Lambda

# 그 외

## 형상관리

- tortoise svn
- github

## 버그 트래킹, 이슈 관리, 협업툴

- jira
- redmine
- mantis

## 스케줄링과 배치

Quartz는 스케줄러의 역할이지, Batch 와 같이 대용량 데이터 배치 처리에 대한 기능을 지원하지 않습니다.

반대로 Batch 역시 Quartz의 다양한 스케줄 기능을 지원하지 않아서 보통은 Quartz + Batch를 조합해서 사용합니다.

정해진 스케줄마다 Quartz가 Spring Batch를 실행하는 구조라고 보시면 됩니다.

- spring batch : 대용량 데이터 배치 처리 프레임워크
- spring quartz : 스케줄링 프레임워크

## IDE

- eclipse
- intelij

## 검색엔진

- elastic search

## 테스트

테스트 주도 개발 : TDD (Test Driven Development)

TDD를 좀 더 명확히 표현하기 위해 탄생한 표현이 BDD (Behavior Driven Development)

### 테스트 관련툴

- JUnit
- xUnit
- Mockito
- Spock

## CDN

서버 트래픽 부하를 줄이기 위한 이런 저런 기능 제공. 캐싱 시스템

- cloudflare

## 그 외

- kafka : 단순 소량이 아닌 대량 메시지 처리 시스템
- kibana : 모니터링