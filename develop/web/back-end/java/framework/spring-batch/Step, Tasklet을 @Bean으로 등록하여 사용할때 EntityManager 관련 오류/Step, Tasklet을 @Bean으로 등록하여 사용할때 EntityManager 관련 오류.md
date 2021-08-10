# 개요

- Step과 Tasklet을 @Bean으로 등록하게 될 경우, 어플리케이션이 구동될때 바로 스프링 빈을 생성하여 관리하게 됨
- 이때 특히 Tasklet에서 @Configuration의 필드인 EntityManagerFactory를 참조하거나, 해당 필드를 매개변수로 넘겨받게 될 경우 EntityManagerFactory가 정상적으로 등록되기 전에 넘겨받아 오류가 날 수 있음
- 이는 결국 Bean을 생성하는 시기에 대한 문제로, 특히 EntityManagerFactory 빈과, Step 그리고 Tasklet빈의 생성시기가 순서대로 생성되지 않을수 있기 때문임

# 해결방안 1 - Bean으로 등록하지 않는다

- Tasklet을 @Bean으로 등록하지 않고 그저 호출시 EntityManagerFactory를 매개변수로 넘겨받아 사용한다.
- 이 경우 @Bean이 아니므로 EntityManagerFactory를 인자로 넘겨줄때 이미 정상적으로 생성된 EntityManagerFactory 객체를 넘겨주므로 문제가 발생하지 않는다

# 해결방안 2 - @JobScope, @StepScope로 빈 생성시기를 지연, 조정한다

- Step에게는 @JobScope를 달아준다
    - 이는 해당 Step의 빈을 Job이 실행될때 생성한다는 뜻이다
- Tasklet에게는 @StepScope를 달아준다
    - 이는 해당 Tasklet의 빈을 Step이 실행될때 생성한다는 뜻이다
- 실행시기에 맞춰서 빈을 생성하도록 지연하므로, 앱구동시 생성되는 EntityManagerFactory가 먼저 빈이 생성되어 문제가 발생하지 않는다