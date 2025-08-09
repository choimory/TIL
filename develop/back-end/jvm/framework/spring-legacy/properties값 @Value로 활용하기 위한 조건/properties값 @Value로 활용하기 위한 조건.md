1. @Value를 사용할 클래스는 Bean을 생성하여 스프링컨테이너가 관리하여야 함
    - 왜냐면 @Value를 통해 필드를 초기화하는것은 스프링 컨테이너에서 bean을 만들고 bean의 property에 값을 주입해주는 로직이기 때문으로 추측됨
2. @Value를 사용할 필드는 static final을 가질수 없음 (final은 초기화실패로 인한 컴파일오류, static은 값이 매핑이 되지 않는 상태로 진행됨)