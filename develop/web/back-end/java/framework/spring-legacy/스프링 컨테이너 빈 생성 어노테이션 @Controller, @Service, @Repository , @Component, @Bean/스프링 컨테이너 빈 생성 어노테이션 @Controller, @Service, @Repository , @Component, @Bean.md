# 개요

- @Controller, @Service, @Repository, @Component, @Bean은 객체의 인스턴스를 스프링 빈 컨테이너에서 생성하고 관리하도록 위임하는 어노테이션들이다.
- 스프링은 해당 어노테이션이 붙은 클래스의 인스턴스는, 자원관리를 위해 프로젝트 가동할때 어노테이션 스캔을 거쳐 한번만 생성한다.
- 그 이후 인스턴스를 주입해야할땐 프로젝트 가동시 생성한 한개의 인스턴스를 반복 주입하므로, 결국 싱글톤 클래스를 활용하는것과 같은 원리가 된다. (대신 생성자가 private은 아니므로 인스턴스를 추가 생성하거나 상속이 허용되는 등 좀 더 유연함)
- 그래서 스프링측은 클래스를 작성할때 클래스에 싱글톤 패턴을 직접 작성 및 호출하는것을 권하지 않는다.
- 대신 빈 생성 어노테이션을 붙이고, 스프링 DI를 통해 해당 클래스의 인스턴스를 스프링 빈 컨테이너에서 주입받아 사용하는것을 권장한다.
- 지역변수로 활용하고 싶을땐 ApplicationContext container = new GenericXmlApplicationContext("컨테이너명.xml")로 컨테이너 생성 후, container.getBean("beanName", Bean.class)로 주입받는다.

# @Controller

- 컨트롤러에 붙임

# @Service

- 서비스에 붙임

# @Repository

- DAO, Repository에 붙임

# @Component

- 직접 작성한 클래스에  붙임

# @Bean

- 외부 라이브러리 클래스에 붙임

# @Scope

객체가 싱글톤 형태로 생성 및 관리되는것을 원치 않을시 @Scope 어노테이션을 추가로 사용한다

- @Scope("globalSession") : 전체 모든 세션에 대해 하나의 빈을 생성
- @Scope("session") : HTTP Session 객체마다 하나의 빈을 생성
- @Scope("request") : HTTP Request 객체마다 하나의 빈을 생성
- @Scope("prototype") : 요구가 있을때마다 새로운 빈을 생성
- @Scope("singleton") : 스프링 컨테이너당 하나의 빈을 생성