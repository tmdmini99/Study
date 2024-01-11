의존성이란?  
- 객체를 생성 및 사용함에 있어 의존 관계가 있는 경우를 말합니다.
## **1. DI (Dependency Injection)**

DI란 의존 관계 주입 혹은 의존성 주입이라 불리고, Spring은 3가지 핵심 프로그래밍 모델(AOP, DI, IOC)을 지원하는데, 그 중 하나가 바로 의존 관계 주입(Dependency Injection)이다. 그리고 Spring은 객체의 의존 관계를 의존 관계 주입을 통해 관리한다.

DI는 외부에서 객체 간의 관계(의존성)를 결정해 주는데 즉, 객체를 직접 생성하는 것이 아니라 외부에서 생성 후 주입시켜 주는 방식이라 할 수 있다.

DI를 통해 객체 간의 관계를 동적으로 주입하여 유연성을 확보하고 결합도를 낮출 수 있다.

![[DI1.jpg]]

**의존 관계**에 대해서 먼저 살펴보자면 어떤 클래스가 다른 클래스를 참조하는 것을 의미한다.

왼쪽 그림에서는 `A객체`가 `B,C 객체`를 참조하는 일반적인 의존성 관계의 모양이다.

오른쪽 그림은 외부에서 의존관계에 있는 B,C 객체를 생성한 후, setter 또는 생성자를 사용하여 `의존 객체를 주입`하고 있는 형태이다.


## **2. Spring에서의 DI 방법 3가지**

Spring에서 의존 관계 주입(DI) 방법에는 총 3가지가 있다.

들어가기 전에 앞서 **Spring 3.x 버전까지만 해도 Setter 주입을 권장하였으나, 최근에는 순환 참조 등의 문제로 인해 Spring 4.3 이후 버전부터는 생성자 주입(Construct Injection) 방법을 권장**하고 있다.

**[ Spring DI 3가지 방법 ]**

- Construct Injection(생성자 주입)
- Field Injection(필드 주입)
- Setter Injection(Setter 주입)

#### **(1) Constructor Injection(생성자 주입)**

- 현재 가장 권장되는 의존 관계 주입 방식이다.
    
    - Spring 공식 문서에서도 생성자 주입을 권장한다.
    - 하나의 생성자가 존재할 때 필드 주입의 대부분의 단점을 극복한다.
    - Spring 4.3부터는 @Autowired가 생략 가능해서 최근에는 생성자를 딱 1개 두고, @Autowired를 생략하는 방법을 주로 사용한다.
    - 추가로 Lombok 라이브러리의 @RequiredArgsConstructor를 함께 사용하면 생성자를 생략 가능해서 코드가 깔끔해진다.
    
- 오직 생성자 주입만이 final 키워드를 사용할 수 있고, 생성자를 통해 주입되는 방식이다.  
    
    - final 키워드를 사용하기에 생성자로 인해 인스턴스가 생성될 때 1번만 할당된다.
    
- final 키워드를 사용해서 값이 한번 할당되면 변경할 수 없기에 객체의 불변성(Immutability)가 보장된다.
    
    - 불변성에 관해서는 맨 아래에서 추가로 정리하였음.
    
- 초기에 할당되기에 NPE(Null Pointer Exception)이 절대 발생하지 않는다.


```java
public class Injection {

    private InjectionService injectionService;
    
    // 생성자 DI
    // @Autowired -> Spring4.3부터는 @Autowired 생략 가능
    public Injection(InjectionService injectionService) {
    	this.injectionService = injectionService;
    }
}
```

위에서는 생성자에 @Autowired 어노테이션을 사용하였다. 하지만 Spring 4.3부터는 @Autowired를 생략 가능하다.

생성자 주입이 가장 좋은 방법이지만, 이렇게 하면 매번 생성자를 만들어야 해서 귀찮고, 코드가 길어질 수 있다.

여기서 롬복(Lombok) 라이브러리를 사용하면 생성자 또한 어노테이션으로 생략할 수 있다.


```java
@RequiredArgsConstructor
public class Injection {

    private InjectionService injectionService;
    
}
```

위의 코드를 보면 **@RequiredArgsConstructor** 은 롬복(Lombok)의 어노테이션 중 하나이다.

해당 어노테이션은 final 키워드가 붙은 주입에만 생성자를 만들어준다. 만약 final 키워드를 사용하지 않으면 @AllArgsConstructor 어노테이션을 사용하면 된다.






---
출처 -
https://backendcode.tistory.com/249
https://huisam.tistory.com/entry/spring-introduce

https://velog.io/@ashwon1218/DIDependency-injection-%EB%9E%80
