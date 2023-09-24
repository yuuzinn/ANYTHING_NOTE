### Spring MVC vs Spring flux

- 동기와 비동기
  - Spring MVC
    - MVC는 동기다.
    - 하나의 요청이 들어올 경우, Thread Pool (기본 200개)에 있는 하나의 Thread를 가져와서 그 요청을 처리한다.<br>
    작업을 하는 도중 DB에 작업이 필요해 잠시 작업을 쉬게 되어도, DB 작업을 마친 후 그 데이터 결과물을 원래 맡았던 Thread가 가져와서
    처리하고 응답한다.
    - Webflux와는 다르게 디버깅의 난이도가 비교적 쉽다. 하나의 Thread가 처리하기 때문이다. 
    - MVC의 경우 최고 성능을 낼 수 있는 Thread의 수를 맞춰 주는 것이 상당히 좋다. 너무 적을 경우, 작업 queue에 쌓일 Thread 수가 늘어나거나, 
    작업할 수 있는 량이 적어지고 Thread가 너무 많이 있다면 오버헤드일 가능성과 쓰레싱의 위험이 있다.<br>
    준비 작업 queue 에 전부 다 들어와 있다가, 대기하는 악순환이 반복될 수 있다.
  - Spring Webflux
    - Spring Webflux는 비동기다.
    - 하나의 요청이 들어올 경우, Core 수의 X2 만큼(기본) 있는 Thread 들 중 하나가 가져와서 처리한다. <br>
    하지만 동기와는 다르게 똑같이 DB 작업이 필요할 경우 같은 Thread가 이 결과물을 가져오는 것이 아니라, (가져올 수도 있음)
    다른 Thread 가 가져올 수도 있으며 작업 분기마다 각각의 Thread가 처리할 수도 있다. 
    - 이러한 특성 때문에 디버깅의 난이도는 상당히 높다. 어떤 Thread가 작업을 어디까지 했고, 어디부터 했는지 알아내기 힘들기 때문.

### Spring 에서는 왜 IoC가 필요한지?

- Proxy 에 관하여

  - 정리 필요

```java
  @Service
  class Test {
    @Transactional // <-- Transactional 적용되는가 ?
    clazz test() {
        ServiceLogic...
      return thread; 
  }
} 

  class Test2 {
    type thread;
    ...
  }
```

- 되지 않는다. 
  - Bean 으로 등록(thread)이 안되어 있기 때문이다.


### Spring 에서 Bean 이름이 같을 경우에는 어떻게 할까 ?

- Bean의 이름이 같을 경우 내부적으로 같지 않게끔 처리가 되어 있지만, 빈의 이름이 같을 수 있게끔 설정을 바꿔줄 수 있다.
만약 이름이 같을 경우에는 Spring이 패키지를 순회하면서 Bean들을 등록하는데, 이 Bean의 등록 순서와도 연관이 있다.
- Bean의 등록 순서는 영문 순으로 위에서 아래로 돌면서 Bean들을 등록한다. Spring에서 같은 이름의 Bean을 등록하게 될 때
기존에 등록되어 있는 Bean이 있고, 이제 등록할 이름이 같은 Bean이 있을 경우 그 Bean을 기존에 있던 Bean의 정보로 덮어씌워버린다.
- 그리고 Spring에서 Bean을 등록하는 순서로는, 개발자가 먼저 등록한 Bean을 먼저 등록하고 그 다음 Spring에서 등록할 Bean을 등록한다.

- 따라서 해결 방법으로는, @Primary를 붙인다던지 ? @Qualifier를 붙인다던지.. 여러 가지의 해결 방법들이 존재한다.