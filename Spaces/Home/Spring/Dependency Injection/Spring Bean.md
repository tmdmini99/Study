## 1. 스프링 빈(Spring Bean)이란?

`Spring IoC 컨테이너가 관리하는 자바 객체를 빈(Bean)`이라고 부릅니다. 이전 포스팅에서 **제어의 역전 (IOC, Inversion Of Control)**에 대하여 간략하게 알아보았는데요. IOC의 특징은 아래와 같습니다.

> - 일반적으로 처음에 배우는 자바 프로그램에서는 **각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 조작하는 작업(객체를 직접 생성하여 메소드 호출)을 했습니다**. 즉, 모든 작업을 사용자가 제어하는 구조였습니다. 예를 들어 A 객체에서 B 객체에 있는 메소드를 사용하고 싶으면, B 객체를 직접 A 객체 내에서 생성하고 메소드를 호출합니다.
> - 하지만 **IOC가 적용된 경우, 객체의 생성을 특별한 관리 위임 주체에게 맡깁니다.** 이 경우 **사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 다른 주체**가 됩니다. 즉, 사용자의 제어권을 다른 주체에게 넘기는 것을 IOC(제어의 역전) 라고 합니다.

우리가 알던 기존의 Java Programming 에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용했었습니다. 하지만 Spring에서는 직접 new를 이용하여 생성한 객체가 아니라, Spring에 의하여 관리당하는 자바 객체를 사용합니다. 이렇게 **Spring에 의하여 생성되고 관리되는 자바 객체를 Bean**이라고 합니다. Spring Framework 에서는 Spring Bean 을 얻기 위하여 ApplicationContext.getBean() 와 같은 메소드를 사용하여 Spring 에서 직접 자바 객체를 얻어서 사용합니다.


![[Spring Bean1.png]]




## . Spring Bean을 Spring IoC Container에 등록하는 방법







JAVA에서 `Annotation` 이라는 기능이 있습니다. 사전상으로는 주석의 의미이지만 Java 에서는 주석 이상의 기능을 가지고 있습니다. Annotation은 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종입니다. 소스코드에 추가하면 단순 주석의 기능을 하는 것이 아니라 특별한 기능을 사용할 수 있습니다.

Java에서는 @Override, @Deprecated 와 같은 기본적인 Annotation을 제공합니다. 아래의 상속 예제에서는 @Override 를 이용하여 상속임을 명시해줍니다.



```java
public class Parent { 
    public void doSomething() { 
        System.out.println("This is Parent"); 
    } 
} 

public class Son extends Parent{ 
    @Override 
    public void doSomething() { 
        System.out.println("This is Son"); 
    } 
}
```

Spring에서는 여러 가지 Annotation을 사용하지만, Bean을 등록하기 위해서는 @Component Annotation을 사용합니다. **@Component Annotation이 등록되어 있는 경우에는 Spring이 Annotation을 확인하고 자체적으로 Bean 으로 등록**합니다.

실제로 사용되는 예시를 볼까요? 실제 Spring 프로젝트에서 Controller를 등록할 때에는 아래와 같은 **Annotation**을 사용합니다. 아래의 예시에서 Controller 임을 Spring 에게 알려주기 위하여 **@Controller Annotation**을 사용했습니다.

```java
// HelloController.java
@Controller
public class HelloController {
    // Http Get method 의 /hello 경로로 요청이 들어올 때 처리할 Method를 아래와 같이 @GetMapping Annotation을 사용하여 Mapping을 사용할 수 있습니다.
    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "This is data!!");
        return "hello";
    }
}
```

@Controller Annotation을 intelliJ에서 Ctrl 을 눌러서 이동해보면 아래와 같은 소스를 확인할 수 있습니다. @Controller Annotation에는 @Component Annotation이 있는 것을 확인할 수 있습니다. @Component Annotation 으로 인하여 Spring은 해당 Controller를 Bean 으로 등록합니다.

```java
// Controller.java

// -- 일부 생략 --
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

}
```

### 2.2. Bean Configuration File에 직접 Bean 등록하는 방법

@Configuration과 @Bean Annotation 을 이용하여 Bean을 등록할 수 있습니다. 아래의 예제와 같이 @Configuration을 이용하면 Spring Project 에서의 Configuration 역할을 하는 Class를 지정할 수 있습니다. 해당 File 하위에 Bean 으로 등록하고자 하는 Class에 @Bean Annotation을 사용해주면 간단하게 Bean을 등록할 수 있습니다.

```java
// Hello.java
@Configuration
public class HelloConfiguration {
    @Bean
    public HelloController sampleController() {
        return new SampleController;
    }
}
```






## xml bean


Beans는 설정 메타 데이터(xml)에 의해 생성이됩니다.  
  
객체 생성을 xml파일 내에서 하기 때문에 일반적으로 JAVA 코드내에서 객체를 생성하지 않습니다.  
  
xml 파일에서 객체를 생성하면 해당 객체는 getBean() 메소드를 사용하여 가져올 수 있습니다.

 Spring container에 의해 생성된다

- Spring container에서 객체를 생성해주기 때문에 개발자가 new 생성자로 객체를 생성할 필요가 없다.
- container에 의해 생성된 bean에 접근하기 위해서는 getBean("[bean_id]")로 통해 bean에 대한 reference 값을 가져올 수 있다.



Spring Beans의 주요 속성입니다.  
  
1. class : 필수 요소이며, 자바 클래스 이름입니다.  
2. id : 고유한 식별자입니다.  
3. scope : 객체의 범위입니다 보통 singleton, prototype을 사용합니다. 기본 scope는 singleton입니다.  
-singleton은 몇 번을 실행해도 같은 객체를 참조합니다.  
-prototype는 실행할 때마다 새로운 객체를 생성합니다.  
4. constructor-arg : 생성과 동시에 생성자에게 전달한 인자 입니다.  
5. property : bean setter에 전달할 인자 입니다.

![[Beans.png]]


---
참조
https://letitkang.tistory.com/127

https://melonicedlatte.com/2021/07/11/232800.html


https://ittrue.tistory.com/220