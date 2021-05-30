# 개요

- 스프링은 클래스를 작성할때 직접 싱글톤 패턴을 작성하는것을 권고하지 않는다.
- 대신 스프링에서 제공하는 스프링 컨테이너 빈 생성 어노테이션 @Controller, @Service, @Repository, @Component, @Bean을 활용할것을 권고한다
- 왜냐면 스프링 빈 컨테이너의 인스턴스 생성 로직이 프로젝트 구동시 한번만 생성한 뒤, 그 뒤로는 동일 인스턴스를 반복 주입하기 때문에 싱글톤의 원리와 같기 때문이다.
- 이때 클래스를 스프링 빈 컨테이너에서 관리하게 할 객체로 생성하고 싶은데, 싱글톤을 피하고 싶을경우 @Scope를 사용한다

# @Scope의 종류

- @Scope("globalSession") : 전체 모든 세션에 대해 하나의 빈을 생성
- @Scope("session") : HTTP Session 객체마다 하나의 빈을 생성
- @Scope("request") : HTTP Request 객체마다 하나의 빈을 생성
- @Scope("prototype") : 요구가 있을때마다 새로운 빈을 생성
- @Scope("singleton") : 스프링 컨테이너당 하나의 빈을 생성