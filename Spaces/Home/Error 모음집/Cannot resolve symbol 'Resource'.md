확인해보니 해당 스프링 서적은 **java8** 기준으로 작성이 되었고, 내가 프로젝트에 적용한 자바 버전은 **java11**이었다.  
java9 버전 부터는 javax.annotaion을 제공하지 않기 때문에 java11에서 `@Generated, @PostConstruct, @PreDestroy, @Resource, @Resources` 어노테이션들을 사용하려면 따로 의존성을 추가해 주거나, java8로 버전을 낮춰주면 된다.

![[error1.png]]

클릭후 project structure (커맨드 ctrl+alt+shift+s)


![[error2.png]]

버전 확인 후  내가 원하는 버전 적용