# Bean Scope

- 변수의 생명주기와는 다른 내용 (로컬변수: 메소드 종료때, 인스턴스 변수: 참조 없을, static 변수: 클래스 언로드때)
- 종류는 싱글턴, 프로토타입, 리퀘스트, 세션, 글로벌 세션, 어플리케이션 6가지가 존재
    - singleton: 컨테이너에 딱 하나, 메모리 및 성능 유리
    - request: HTTP Request 별로 하나씩, 요청마다 고유의 빈 
    - session: HTTP Session 별로 하나식
    - application: 서블릿 생명주기 안에 하나
- 객체생성 -> 의존 설정 -> 초기화 -> 사용 -> 소멸(destroyed)
- Application Context가 아닌 Spring context에 관리를 위임할시 스프링이 어노테이션 스캔하여 해당 클래스들에 대한 빈 생성 및 소멸을 관리한다
- @PostConstruct로 생성, @PreDestroy로 소멸
- 컨테이너 생성 -> 빈 생성 -> 의존관계 주입 -> 초기화 콜백 메소드 호출 -> 소멸 전 콜백 메소드 호출 -> 종료

---

# Spring context(스프링 컨테이너)

- 인스턴스의 생명주기 관리를 위임받은 경우, 스프링은 해당 인스턴스들을 spring context에 저장하여 관리한다
- IoC, DI는 스프링 컨테이너에 저장된 인스턴스를 이용하여 처리된다

---

# IoC

- 위의 인스턴스의 생명주기 관련된 생성 및 관리를 위임받아 객체의 생성과 소멸을 대신 처리해주는것을 제어의 역전(IoC)라 함

---

# DI

- 작성한 코드의 의존성을 스프링 컨테이너로부터 주입받는것
- 생성자 주입, 세터 주입, 필드 주입이 있으며 생성자 주입이 권장된다
    - 생성자 주입은 인스턴스를 생성할때 주입되며 final을 붙여 불변으로 만들 수 있기 때문이다
- 의존성은 모듈의 변경시 다른 모듈까지 모두 바뀌어야 되거나 오류가 전염되는등, 이러한 문제를 코드에서 걷어내어 DI를 스프링에게 위임한다

---

# Filter, Interceptor, AOP

![img.png](img.png)

## Filter

- 서블릿 이전에 실행된다
- 인코딩 변환 처리, XSS 공격 방어 등의 처리 등에 사용 가능
- WAS 구동시 FilterMap이라는 배열에 등록되어 Filter chain을 구성하여 순차 실행됨
- spring context 외부에 존재하여 Spring과 무관한 자원에 대해 동작
- init()으로 필터 인스턴스를 초기화 -> doFilter()로 필터처리 로직 -> destroy()로 종료

## Interceptor

- 서블릿 이후 컨트롤러 이전에 실행
- spring context 내부에서 컨트롤러의 요청 응답에 관여하며, 모든 bean에 접근 가능
- 예외 발생시 예외핸들러로 처리
- 로그인 체크, 권한 체크, 로그 등에 사용 가능
- preHandler(): 컨트롤러 전, postHandler(): 컨트롤러 후, afterCompletion(): 뷰 렌더링 후

## Filter와 Interceptor 차이

- Filter: WAS단에 설정, Spring과 무관한 자원에 동작
- Interceptor: Spring context에 설정, 컨트롤러 전후에 동작
- Filter는 doFilter()만 존재, Interceptor는 pre, post가 존재
- Interceptor는 AOP와 유사하게 사용 가능
    - handlerMethod(@RequestMapping을 사용해 매핑 된 @Controller의 메소드)를 파라미터로 제공하여 메소드 시그니처 등 추가 정보를 파악해 로직 실행 여부 판단 가능

## AOP

- 반복되는 특정 핵심 로직, 부가기능을 모듈화하여 분리하고 재사용하는것
- 주요 용어
    - Aspect: Advice + PointCut
    - Target: Aspect를 적용할 곳
    - Advice: 수행해야 되는 기능을 담은 구현체
    - JoinPoint: Advice 적용할 위치, 끼어들 지점
    - PointCut: JoinPoint의 상세 정의(어느 메소드, 어느 어노테이션, 어느 파일명 등등)
    - Weaving: PointCut에 의해 결정된 Target의 JoinPoint에 Advice를 끼워넣는 과정

### AOP 적용방식

- AspectJ: 컴파일시 적용(자바를 컴파일하여 바이트코드로 만들때 Adivce 소스가 추가된 바이트코드를 만듬), 로드시 적용(컴파일된 파일을 로딩할때 Advice 소스를 추가)
- Spring AOP: 런타임시 빈을 생성하는데, A라는 빈을 만들때 A라는 타입의 프록시 빈도 생성하고, 프록시빈이 A 메소드 호출 직전에 Advice를 호출한 뒤에 A 메소드를 호출함

### Spring AOP

- 프록시 패턴 기반이다
    - Target 객체에 대한 프록시를 만들어 제공
    - Target을 감싸는 프록시가 런타임시 제공
    - 접근 제어 및 Advice추가를 위해 프록시 객체 사용
- 프록시가 Target 객체 호출을 가로채 Target 수행 전/후에 Advice를 호출
- Spring bean에만 AOP를 적용할 수 있음
- 메소드 JoinPoint만 지원함, 메소드가 호출되는 런타임 시점에만 Advice를 적용할 수 있음
- 모든 AOP를 다 지원하지는 않음

---

# 스프링 프레임워크란

---

# Spring, Spring MVC, Spring Boot의 차이

---

# MVC

---

# POJO

---

# 라이브러리와 프레임워크의 차이

---

# DAO와 DTO의 차이

---

# DTO와 VO의 차이

---

# Spring JDBC

---