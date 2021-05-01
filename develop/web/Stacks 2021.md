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