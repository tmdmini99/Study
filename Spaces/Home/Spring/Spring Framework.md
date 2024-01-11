## **contextConfigLocation**

스프링프레임 워크가 동작하기 위한 설정파일의 위치를 알려주는 파라미터  
contextConfigLocation이라는 파라미터를 사용하면 Context Loader가 load할 수 있는 설정파일을 쓸 수 있다.


## ContextLoaderListener

스프링에서 제공하는 클래스중 하나로 ContextLoader와 ServletContextListener를 상속하고 있다.  
서블릿 컨테이너 생명주기에 맏춰서 spting의 application context를 servlet attribute에 등록하고 제거한다

**WAS구동시에 web.xml을 읽어들여 웹 어플리케이션 설정을 구성하기위한 즉, 초기셋팅작업을 해주는 역할**


ContextLoaderListener의 역할

- ContextLoaderListener 와 DispatcherServlet 은 각각 WebApplicationContext 를 생성하는데, 스프링에서 사용되는 Context 간의 계층 관계를 연결해주는 부분
- 웹 어플리케이션이 시작되고 종료되는 시점에 Servlet Context가 생성하는 이벤트를 연결
- Servlet WebApplicationContext 에서는 Root WebApplicationContext 를 참조 가능하지만, 그 반대로 참조 불가







스프링 프레임워크가 동작하려면 가장 기본적으로 web.xml 파일에 스프링 설정 파일을 명시해 줘야 한다.