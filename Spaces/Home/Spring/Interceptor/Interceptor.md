
### 1.1 인터셉터란?

**컨트롤러(Controller)의 '핸들러(Handler)'를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할수 있는 일종의 필터**

interceptr 란 단어는 '낚아채다'라는 의미이다. 해당 단어의 의미와 같이 사용자 요청에 의해 서버에 들어온

Request 객체를 컨트롤러의 핸들러(사용자가 요청한 url에 따라 실행되어야 할 메서드, 이하 핸들러)로

도달하기전에 낚아채서 개발자가 원하는 추가적인 작업을 한후 핸들러로 보낼수 있도록 해주는것이 인터셉터 이다.

### 1.2 왜 사용하는가?

**개발자는 특정 Controller의 핸들러가 실행되기 전이나 후에 추가적인 작업을 원할때 Interceptor를 사용한다.**

(추가적인 작업으로는 로그인체크, 권한 체크 등이 있다.)

권한 체크 예를 통해서 개발자가 인터셉터의 어떠한 이점때문에 사용하기를 원하는지 살펴보겠다.

개발자가 관리자 계정만이 실행할수 있는 Controller 핸들러를 작성한다고 가정하겠다.

개발자는 오직 관리자 계정만 실행할수 있도록 하기 위해 핸들러에 접근하는 사용자가 관리자 인지 확인하는

세션 체크 코드를 각 핸들러에 작성해줘야한다.

작성해주어야 할 핸들러수가 적다면 문제가 되지않는다. 하지만 적용해야할 핸들러가 수천개가 된다면 어떻게될까?

크게 두가지 문제가 생긴다.

1) 메모리 낭비, 서버의 부하가 늘어난다. 적용해야할 핸들러만 수만큼 세션체크 코드를 작성함으로써 반복되는 코드들이 매우 많아지기 때문이다.

2) 코드의 누락에 대한 걱정이다. 사람이 작성을 하는것이기 때문에 누락과 같은 실수가 발생할수 밖에없다.


 만약 회원정보에 접근하는 핸들러가 세션체크가 누락되어서 관리자 인지 확인을 안한다면, 자격이 없는 사용자가

 접근할수 있게되어 보안적으로 큰 문제를 가지게 된다.

이러한 문제점을 줄이기 위한 수단으로, 개발자는 인터셉터를 사용할수 있다.

인터셉터를 사용하게 되면 개발자는 핸들러 수 만큼 작성했던 세션 체크 코드를 인터셉터 클래스에 한번만 작성하면된다. 이로 인해 코드의 량이 현저히 줄기 대문에 메모리 낭비를 줄일수 있다.

그리고 인터셉터 적용의 유무 기준이 되는 url을 servlet-context.xml에 설정해주게 되면 스프링에서 일괄적으로

해당 url 경로의 핸들러에 인터셉터를 적용해주기 때문에 누락에 대한 위험이 상당히 줄게된다.

### 1.3 구현수단

스프링에서 제공하는 **org.springframework.web.servlet.HandlerInterceptor** 인터페이스를 구현하거나,

**org.springframework.web.servlet.handler.HandlerInterceptorAdapter** 추상클래스를 오버라이딩 함으로써


자신만의 인터셉터를 만들수 있다. HandlerInterceptorAdapter 추상클래스 경우  HandlerInterceptor 인터페이스를 상속받아 구현되었다.

### 1.4 어떤 메서드를 가지고 있는가?

스프링이 제공해주는 HandlerInterceptor 인터페이스와 HandlerInterceptorAdapter 추상클래스에 정의되어 있는

메서드는 preHandle(), postHandle(), afterCompletion() 3가지이다.

**1) preHandle()**

- 컨트롤러가 호출되기 전에 실행됨
- 컨트롤러가 실행 이전에 처리해야 할 작업이 있는경우 혹은 요청정보를 가공하거나 추가하는경우 사용
- 실행되어야 할 '핸들러'에 대한 정보를 인자값으로 받기때문에 '서블릿 필터'에 비해 세밀하게 로직을 구성할수 있음
- 리턴값이 boolean이다. 리턴이 true 일경우 preHandle() 실행후 핸들러에 접근한다. false일경우 작업을 중단하기 때문에 컨트롤러와 남은 인터셉터가 실행되지않는다.

**2) postHandle()**

- 핸들러가 실행은 완료 되었지만 아직 View가 생성되기 이전에 호출된다.
- ModelAndView 타입의 정보가 인자값으로 받는다. 따라서 Controller에서 View 정보를 전달하기 위해 작업한        Model 객체의 정보를 참조하거나 조작할수 있다.
- preHandle() 에서 리턴값이 fasle인경우 실행되지않음.
- 적용중인 인터셉터가 여러개 인경우, preHandle()는 역순으로 호출된다.
- 비동기적 요청처리 시에는 처리되지않음.

**3)  afterCompletion()**

- 모든 View에서 최종 결과를 생성하는 일을 포함한 모든 작업이 완료된 후에 실행된다.
- 요청 처리중에 사용한 리소스를 반환해주기 적당한 메서드 이다.
- preHandle() 에서 리턴값이 false인경우 실행되지 않는다.
- 적용중인 인터셉터가 여러개인경우 preHandle()는 역순으로 호출된다.
- 비동기적 요청 처리시에 호출되지않음.

## 2. 인터셉터 동작 위치 및 순서

1) 사용자는 서버에 자신이 원하는 작업을 요청하기 위해 url을 통해 Request 객체를 보낸다.

2) DispatcherServlet은 해당 Request 객체를 받아서 분석한뒤 '핸들러 매핑(HandlerMapping)' 에게

사용자의 요청을 처리할 핸들러를 찾도록 요청 한다.

3) 그결과로 핸들러 실행체인(HandlerExectuonChanin)이 동작하게 되는데, 이 핸들러 실행체인은 하나이상의 핸들러 인터셉터를 거쳐서 컨트롤러가 실행될수 있도록 구성되어 있다.

(핸들러 인터셉터를 등록하지 않았다면, 곧바로 컨트롤러가 실행된다. 반대로 하나이상의 인터셉터가 지정되어 있다면 지정된 순서에 따라서 인터셉터를 거쳐서 컨트롤러를 실행한다)

![[Interceptor1.png]]


## 3. 구현 방법 및 실습

#### 3.1 구현 방법

 스프링 프레임워크에서 인터셉터를 적용하기 위한 큰틀은 아래와 같다.

1) HandlerInterceptor 혹은 HandlerInterceptorAdater 를 상속받아서 자신만의 Interceptor 클래스를 생성한다.

2) servlet-context.xml에 우리가 작성한 Interceptor 클래스를 빈(bean)으로 등록해주고, Interceptor를 적용할 url을 작성

#### 3.2 실습

스프링 프레임워크에서 MVC프로젝트(스프링 레거시) 처음만들게 되었을때 기본적으로 작성되 있는 

HomeController와 home.jsp로 인터셉터 구현 및 테스트를 해보겠다.

1. pom.xml에 추가 (기본적으로 레거시 프로젝트를 생성하면 설정되어 있음.)


```xml
<dependency>

	<groupId>org.springframework</groupId>

	<artifactId>spring-webmvc</artifactId>

	<version>${org.springframework-version}</version>

</dependency>
```



 자신이 원하는 이름의 클래스(class)를 생성한 후 해당 클래스에 HandlerInterceptor, HandlerIntreceptorAdapter 둘 중 하나를 선택하여 상속을 해주시면 됩니다. 상속 시에 차이점은 HandlerInterceptor 경우 인터페이스이기 때문에 implements 키워드를, HandlerInterceptorAdapter는 추상 클래스이기 때문에 extends를 키워드를 사용해야 합니다.


![[Interceptor2.jpg]]


HandlerInterceptor 상속



![[Interceptor3.jpg]]


 메서드는 preHandle(), postHandle(), afterCompletion()를 모두 사용해보겠습니다. 모든 메서드 구현부에는 는 공통적으로 메서드의 이름을 출력하도록 코드를 추가하였습니다. (단순히 테스트를 위한 목적이기 때문에 logger 기록이 아닌 println() 메서드를 사용하였습니다.




```java
public class CustomInterceptor implements HandlerInterceptor {

    @Override

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

            throws Exception {

        System.out.println("preHandle1");

        return true;

    }

    @Override

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,

            ModelAndView modelAndView) throws Exception {

        System.out.println("postHandle1");

    }

    @Override

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)

            throws Exception {

        System.out.println("afterCompletion1");

    }    

}
```


![[Interceptora4.png]]

 preHandler() 메서드에는 인자값으로 받는 request와 handler의 정보를 출력하도록 코드를 추가하였습니다. (해당 정보들은 postHandle(), afterCompletion() 메서드에서도 동일한 출력값을 가지기 때문에 preHandle() 에만 작성하였습니다.

* return 값에 false를 입력할 경우 위에서 설명한 것처럼 Contorller로 넘어가지 않습니다.


```java
 @Override

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

            throws Exception {

        System.out.println("preHandle1");

        System.out.println("[preHandle][" + request + "]" + "[" + request.getMethod() + "]" + request.getRequestURI());

        System.out.println("[handler][" + handler.toString() + "]");

        return true;

    }
```

![[Interceptor5.png]]

 postHandle() 메서드에는 Contorller에서 Model에 담은 정보("serverTime")를 인자값으로 들어오는 ModelAndView를 통해서 출력시켜보겠습니다.

![[Interceptor6.png]]



```java
@Override

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,

            ModelAndView modelAndView) throws Exception {

        System.out.println("postHandle1");

        System.out.println("[ModelAndView][ model 이름 : " + modelAndView.getViewName() + "][ model 내용 :" + modelAndView.getModel() + "]" );

    }
```


작성한 Interceptor 클래스를 스프링에서 인식할 수 있도록 빈으로 등록해주어야 합니다. Interceptor가 Servlet 단계에서 작동하는 것이기 때문에 servlet-context.xml에 설정을 해주어야합니다. 우리가 만든 Interceptor 클래스를 빈으로 등록해주는 코드와 Interceptor를 어떠한 url에 적용시킬지를 설정하는 코드를 추가합니다. (프로젝트 생성 후 아무것도 추가하지 않았기 때문에 만들어진 url이 '/home' 밖에 없습니다. 따라서 설정에는 '/home' 입력해주거나 전체 url에 적용되도록'/**'를 입력해주시면 됩니다.


```xml
<interceptors>

           <interceptor>

               <mapping path="/**"/>

                   <beans:bean id="commonInterceptor" class="com.vam.interceptor.CustomInterceptor"/>

        </interceptor>

    </interceptors>
```

![[Interceptor7.png]]


서버를 실행시켜 테스트를 해봅니다. 저의 경우 Tomcat server의 경로를 전혀 설정해주지 않았기 때문에 '/home'을 실행하기 위해서 'http://localhost:8080/controller/home'를 입력하였습니다.

![[Interceptor8.png]]


![[Interceptor9.png]]
![[Pasted image 20240930125542.png]]



 파랑은 prehandle(), 빨강은 posthandle(), 노랑은 aterCompletion()입니다. Request, Handler, ModelAndView의 정보가 정상적으로 출력된 것을 확인할 수 있습니다.

## **4. 다중 인터셉터**

 하나의 url 한 개 이상의 인터셉터를 적용시킬 수 있습니다. 직접 적용해보겠습니다.

 추가로 적용시킬 Interceptor 클래스를 작성하였습니다. 메서드는 3개 모두 작성하였으며 2번째 Interceptor 이기 때문에 각 메서드의 구현부에 '메서드 이름 + 2"를 작성하였습니다.



```java
public class SubInterceptor implements HandlerInterceptor {

    @Override

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

            throws Exception {

        System.out.println("preHandle2");

        return true;

    }

    @Override

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,

            ModelAndView modelAndView) throws Exception {

        System.out.println("postHandle2");

    }

    @Override

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)

            throws Exception {

        System.out.println("afterCompletion3");

    }
```



![[Interceptor10.png]]

```xml
<interceptors>

           <interceptor>

              <mapping path="/**"/>

               <beans:bean id="commonInterceptor" class="com.vam.interceptor.CustomInterceptor"/>

        </interceptor>

        <interceptor>

            <mapping path="/**"/>

            <beans:bean id="subInterceptor" class="com.vam.interceptor.SubInterceptor"/>

        </interceptor>

    </interceptors>
```



![[Interceptor11.png]]


 결과는 아래와 같습니다. 결과를 보시면 servlet-context.xml에 Interceptor를 작성한 순으로 출력되는 것을 확인할 수 있습니다. 특이한 점은 컨트롤러를 거친 후 perhandler(), afterCompletion() 메서드는 역순으로 출력된다는 점입니다.


![[Interceptor12.png]]


---
출처 - https://popo015.tistory.com/115