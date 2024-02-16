
## **어노테이션이란? (Annotation)**

- 어노테이션은 다른 프로그램에게 유용한 정보를 제공하기 위해 사용되는 것으로 주석과 같은 의미를 가진다.
- 주석과는 그 역할이 다르지만 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다
*메타데이터 : 데이터에 대한 데이터, 어떤 목적을 가지고 만들어진 데이터, 다른 데이터를 설명해주는 데이터, 콘텐츠에 부여되는 데이터

# 📌어노테이션의 종류
- 표준(내장) 어노테이션 : 자바가 기본적으로 제공해주는 어노테이션
- 메타 어노테이션 : 어노테이션을 위한 어노테이션
- 사용자정의 어노테이션 : 사용자가 직접 정의하는 어노테이션




![[Annotation3.png]]



## 🔎표준 어노테이션

표준 어노테이션(메타 어노테이션 제외)의 종류는 다음과 같다.

1. @Override
2. @Deprecated
3. @SuppressWarnings

### 📌@Override

> [[오버라이딩]]을 올바르게 했는지 컴파일러가 체크한다.

Override는 오버라이딩할 때, 메서드의 이름을 잘못적는 실수를 방지해준다.

```java
class Parent{
	void parentMethod(){}
}

class Child extends Parent{
	@Override
    void pparentmethod(){} // 컴파일 에러! 잘못된 오버라이드 스펠링 틀림
     
```

### 📌@Deprecated

> 앞으로 사용하지 않을 것을 권장하는 필드나 메서드에 붙인다.

자바에서 메소드를 사용했는데 다음과 같이 표시된 경험이 있을 것이다.  
"~~getDate~~()"  
이유는 해당 메소드 상위에 @Deprecated 어노테이션이 붙어있기 때문이다.

자바의 Date 클래스의 getDate()

```java
@Deprecated
public int getDate(){
	return normalize().getDayOfMonth();
}
```

위의 "getDate" 메서드는 자바에서 사용하지 않을 것을 권장하는 메소드이다.

**없애지 그랬니?**  
자바는 하위 호환성을 엄청나게 중요하게 여긴다. 이전에 해당 메소드로 개발을 진행한 프로젝트들이 있기 때문에 유지는 하되, 권장하지 않는다.

### 📌@FunctionalInterface

> 함수형 인터페이스에 붙이면, 컴파일러가 올바르게 작성했는지 체크

해당 어노테이션은, 함수형 인터페이스의 "하나의 추상메서드만 가져야 한다는 제약"을 확인해준다.  
또한 함수형 인터페이스라는 것을 알려주는 역할도 한다.

### 📌@SuppressWarnings

> 컴파일러의 경고메세지가 나타나지 않게 한다.

```java

@SuppressWarnings("unchecked")
ArrayList list = new ArrayList(); // 제네릭 타입을 지정하지 않음!
list.add(obj); // 경고 발생 !!! 경고 내용 = unchecked
```

위의 코드를 보자.  
Array를 선언할 때 제네릭을 통해서 타입에 대한 정보를 기입하지 않았다.  
그래서 타입을 선언하지 않았다는 "unchecked"라는 경고가 뜬다.  
하지만 "@SuppressWarnings("unchecked)"를 입력해주었기 때문에 "unchecked"에 대한 경고는 억제된다.

**보통 경고가 많을 때, 확인된 경고는 해당 어노테이션을 붙여서 새로운 경고를 알아보지 못하는 것을 방지하기 위해 사용한다.**

---

## 🔎메타 어노테이션

메타 어노테이션은 어노테이션을 위한 어노테이션이다.

### 📌@Target

> 어노테이션을 정의할 때, 적용대상을 지정하는데 사용한다.

```java
@Target({TYPE, FIELD, TYPE_USE})
@Retention(RetentionPolicy.SOURCE)
public @interface MyAnnotation{}

@MyAnnotation // 적용 대상이 Type(클래스, 인터페이스)
class MyClass{
	@MyAnnotation //적용 대상이 FIELD인 경우
    int i;
    
    @MyAnnotation //적용 대상이 TYPE_USE인 경우
    MyClass mc;
}
```



### 📌@Retention

> 어노테이션이 유지되는 기간을 지정하는데 사용

- SOURCE : 소스 파일에만 존재. runtime 때 없앤다
- RUNTIME : 클래스 파일에 존재. 실행시에 사용가능 runtime 때까지 접근할 수 있다.
- CLASS : .class 파일엔 적혀있지만 runtime 때 없앤다. 특별히 Retention을 지정하지 않았다면 이게 Default

```java

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override{}

```

### 📌@Documented

> javadoc으로 작성한 문서에 포함시키려면 해당 어노테이션을 붙인다. 
> Javadoc이 만들어질 때 Annotation 이 표시되도록 만드는 Annotation 이다.



### 📌@Inherited

> 어노테이션도 상속이 가능하다. 어노테이션을 자손 클래스에 상속하고자 할 때, @Inherited를 붙인다.

```java
@Inherited
@interface SuperAnno{}

@SuperAnno
class Parent{}

// <- 여기에 @SuperAnno 가 붙은 것으로 인식
class Child extends Parent{}
```

### 📌@Repeatable

> 반복해서 붙일 수 있는 어노테이션을 정의할 때 사용

```java
@Repeatable(ToDos.class)
@interface ToDo{
	String value();
}

@ToDo("delete test codes.")
@ToDo("override inherited methods")
class MyClass{
	~~
}

@interface ToDos{
	ToDo[] value();
}
```

MyClass를 보면 "@ToDo" 어노테이션이 여러개가 정의된 것을 볼 수 있다.

@Repeatable 어노테이션은 위의 "ToDos"처럼 "컨테이너 어노테이션"도 정의해야 한다.

### @SuppressWarnins

컴파일러의 경고를 억제하도록 알려주는 Annotation 이다.



---

# 🔎어노테이션 생성하기

어노테이션을 생성하는 것은 매우 쉽다.  
다음 코드와 같이 작성해주면 끝이다.

```java

@interface 이름{
	타입 요소 이름(); // 어노테이션의 요소를 선언
	    ...
}
```

실제로 만들고, 적용해보자.

```java
@interface DateTime{
	String yymmdd();
    String hhmmss();
}

@interface TestInfo{
	int count() default 1;
    String testedBy();
    TestType testType();
    DateTime testDate();
}


@TestInfo{
	testedBy="Kim",
    testTools={"JUnit", "AutoTester"},
    testType=TestType.FIRST,
    testDate=@DateTime(yymmdd="210922", hhmmss="211311")
)// count를 생략했으므로 default인 "count=1"이 적용된다.
public class NewClass{...}
```

그리고 추가적인 특징을 보자.

### 📌어노테이션 요소 특징

- 적용시 값을 지정하지 않으면, 사용될 수 있는 기본값을 지정할 수 있다.(위의 default)
    
- 요소가 하나이고 이름이 value일 때는 요소의 이름 생략가능하다.
    

```java
@interface TestInfo{
	String value();
}
@TestInfo("passed") // value="passed"와 동일
class NewClass{...}
```

- 요소의 타입이 배열인 경우, 괄호{}를 사용해야 한다.

```java
@interface TestInfo{
	String[] testTools();
}

@TestInfo(testTools={"JUnit", "AutoTester"})
@TestInfo(testTools="JUnit") // 요소가 1개일 때는 {}를 사용하지 않아도 된다.
@TestInfo(testTool={}) // 요소가 없으면 {}를 써넣어야 한다.
```

---

# 🔎모든 어노테이션의 조상

> Annotation은 모든 어노테이션의 조상이지만 상속은 불가능하다.

"Annotation"은 다음과 같이 생겼다.

```java
public interface Annotation{
	boolean equals(Object obj);
    int hashCode();
    String toString();
    
    Class<? extends Annotation> annotationType();
    }
```

---

# 🔎마커 어노테이션

> 요소가 하나도 정의되지 않은 어노테이션

대표적으로 "@Test"가 있다.  
해당 어노테이션은 테스트 프로그램에게 테스트 대상임을 알리는 어노테이션이다.

---

# 🔎어노테이션 규칙

어노테이션에도 반드시 지켜주어야 하는 규칙이 있다. 다음 4가지를 살펴보자.

- 요소의 타입은 기본형, String, enum, 어노테이션, Class만 허용된다.
- 괄호()안에 매개변수를 선언할 수 없다.
- 예외를 선언할 수 없다.
- 요소의 타입을 매개변수로 정의할 수 없다.(\<T>)

잘못된 예시를 통해서 이해해보자!

```java

@interface AnnoConfigTest{
    int id = 100; // 상수 ok
    String major(int i, int j) //매개변수 x
    String minor() throws Exception; // 예외 x
    ArrayList<T> list(); // 요소의 타입을 매개변수 x
```





## 의존 객체 자동 주입(Automatic Dependency Injection)

의존 객체 자동 주입(Automatic Dependency Injection)은 스프링 설정파일에서 혹은 태그로 의존 객체 대상을 명시하지 않아도 스프링 컨테이너가 자동적으로 의존 대상 객체를 찾아 해당 객체에 필요한 의존성을 주입하는 것을 말한다.

@Resource, @Autowired, @Inject 이 태그들의 차이점은 의존 객체를 찾는방식이 다르다.

## @Resource

Java에서 지원하는 어노테이션입니다. 특정 프레임 워크에 종속적이지 않다.

찾는 순서

> 이름 -> 타입 -> @Qualifier -> 실패

name 속성의 이름을 기준으로 찾습니다. 없으면 타입, 없으면@Qualifier 어노테이션의 유무를 찾아 그 어노테이션이 붙은 속성에 의존성을 주입한다.

<context:annotation-config/> 구문을 꼭 xml 설정파일에 추가해야한다.

**사용할 수 있는 위치**

멤버변수 , setter 메소드

## @Autowired

Spring에서 지원하는 어노테이션입니다.

찾는 순서

> 타입 -> 이름 -> @Qualifier -> 실패

@Autowired는 주입하려고 하는 객체의 타입이 일치하는지를 찾고 객체를 자동으로 주입한다. 만약에 타입이 존재하지 않는다면 @Autowired에 위치한 속성명이 일치하는 bean을 컨테이너에서 찾는다. 그리고 이름이 없을 경우 @Qualifier 어노테이션의 유무를 찾아 그 어노테이션이 붙은 속성에 의존성을 주입한다.

<context:annotation-config/> 구문을 꼭 xml 설정파일에 추가해야한다.

**사용할 수 있는 위치**

멤버변수, setter메소드, 생성자, 일반 메소드에 적용가능

## @Inject

Java에서 지원하는 어노테이션입니다. 특정 프레임 워크에 종속적이지 않다.

찾는 순서

> 타입 -> @Qualifier-> 이름 -> 실패

@Aurowired와 동일하게 작동하지만 찾는 순서가 다릅니다.

@Inject를 사용하기 위해서는 maven이나 gradle에 javax 라이브러리 의존성을 추가해야한다.

**사용할 수 있는 위치**

멤버변수, setter 메소드, 생성자, 일반 메소드에 적용 가능

## @Qualifier ?

그래서 이건뭘까? 해서 찾아봤더니

만약에 타입이 동일한 bean객체가 여러개 있으면 Spring이 Exception을 일으킨다. (ex..@Autowired를 동일한 타입에 쓴곳이 있다면)

스프링이 어떤bean을 주입해야 될지 모르기 때문에 스프링 컨테이너를 초기화 하는과정에서 Exception

- @Autowired의 주입 대상이 한 개여야 하는데 실제로는 두 개 이상의 빈이 존재해 주입할 때 사용할 객체를 선택할 수 없기 때문이다.
- 단, @Autowired가 적용된 필드나 설정 메서드의 property 이름과 같은 이름을 가진 빈 객체가 존재할 경우에는 이름이 같은 빈 객체를 주입받는다.

아무것도 몰랐을때는 이걸로 문제가 발생한적이 있는데 왜 그런지 몰라서 시간 좀 잡아먹었다 ;;

이럴때 @Qualifier를 쓰면 해결해줄수 있다. 한정자를 설정해준다.

```null
<context:annotation-config>
    <-- Member member = new Member() -->
    <bean id="member1" class="example.Member">
        <qualifier value="m1"/>
    </bean>
<context:annotation-config/>
```

이런식으로 qualifier를 추가해준다.

```null
pubic class MemberDao{  
    @Autowired  @Qualifier("m1")
    private Member member;       

    public void setMember(Member member){      
        this.member = member;  
    }
}
```

@Qualifier("m1")이라고 해줬으니 member1 bean을 쓰겟다는것이 되는것이다.

**Exception**

@Qualifier에 지정한 한정자 값을 갖는 bean 객체가 존재하지 않으면 Exception이 발생한다



`@Qualifier` 어노테이션을 사용하는 간단한 예제 코드입니다.


```java
// InterfaceA라는 인터페이스를 선언합니다.
public interface InterfaceA {
    void doSomething();
}

// InterfaceA를 구현하는 ClassA입니다.
@Component("classA") // 빈의 이름을 "classA"로 지정합니다.
public class ClassA implements InterfaceA {
    public void doSomething() {
        System.out.println("ClassA is doing something.");
    }
}

// InterfaceA를 구현하는 ClassB입니다.
@Component("classB") // 빈의 이름을 "classB"로 지정합니다.
public class ClassB implements InterfaceA {
    public void doSomething() {
        System.out.println("ClassB is doing something.");
    }
}

// InterfaceA 타입의 빈을 주입받는 클래스입니다.
@Service
public class SomeService {
    private final InterfaceA interfaceA;

    // 생성자를 통해 InterfaceA 타입의 빈을 주입받습니다.
    // @Qualifier 어노테이션을 사용하여 어떤 빈을 주입받을지 명시합니다.
    @Autowired
    public SomeService(@Qualifier("classA") InterfaceA interfaceA) {
        this.interfaceA = interfaceA;
    }

    public void doSomething() {
        interfaceA.doSomething(); // 주입받은 빈의 메소드를 호출합니다.
    }
}
```

위 코드에서 `SomeService` 클래스는 `InterfaceA` 타입의 빈을 주입받습니다. `@Qualifier("classA")`를 사용하여 `classA`라는 이름의 빈을 주입받도록 명시하였습니다. 따라서 `SomeService`의 `doSomething` 메소드를 호출하면, "ClassA is doing something."이 출력됩니다.

만약 `@Qualifier("classB")`로 변경하면, "ClassB is doing something."이 출력될 것입니다. 이렇게 `@Qualifier`를 사용하면 동일한 타입의 빈이 여러 개 있을 경우, 어떤 빈을 주입할지 명확하게 지정할 수 있습니다.



  

### @RequestParam

`@RequestParam` 어노테이션은 사용자가 요청시 전달하는 값을 `Handler(Controller)`의 매개변수로 1:1 맵핑할때 사용되는 어노테이션입니다.

```java
@Controller
public class TestController {
    @GetMapping("/")
    public String getTestPage(@RequestParam("name") String name) {
        System.out.println("이름 : " + name);
        return "test";
    }
}
```

예를 들어 사용자가 `/?name=test` 로  요청한다면, 위 핸들러의 매개변수인  `name` 에 "test"가 매핑됩니다.

  

### @ModelAttribute

우선 @ModelAttribute는 메소드레벨, 메소드의 파라미터 두군데에 적용이 가능합니다. 하지만 이번 포스팅에서는, 메소드의 파라미터에 사용되는 경우에 대해서 다루도록 하겠습니다.

`@ModelAttribute`는 사용자가 요청시 전달하는 값을 오브젝트 형태로 매핑해주는 어노테이션입니다.

```java
@Getter
@Setter
public class TestModel {
    private String name;
    private int age;
}

@RestController
public class TestController {
    @GetMapping("/")
    public String getTestPage(@ModelAttribute TestModel testModel) {
        System.out.println("이름 : " + testModel.getName());
        System.out.println("나이 : " + testModel.getAge());
        return "test";
    }
}
```

예를들어, `name, age`를 인스턴스 변수로 가지는 `TestModel`객체가 존재하고 이를 매개변수로 받기 위해서는 위와 같이 컨트롤러를 생성하고, `/?name=JJY&age=10`로 요청을 하면 각각의 값이 핸들러의 testModel 객체로 바인딩됩니다. (Setter가 존재해야 합니다.)

  

  

## 2. @ModelAttribute 사용시 장점

`@RequestParam`과 `@ModelAttribute`의 눈에 띄는 차이점은, 1:1 매핑이냐, 객체 매핑이냐 인것으로 보입니다.

그렇다면 [@RequestParam](https://github.com/RequestParam)으로 모두 전달받으면 되는데 `@ModelAttribute`로 사용자의 요청을 매핑을 해야하는 이유가 무엇일까요?

**사용자를 찾기위해, 검색조건을 요청에 담아 전달하는 경우를 예로 들어보겠습니다.**

### @ModelAttribute를 사용하지 않는 경우

```java
@RestController
public class TestController {
    @GetMapping("/")
    public String getTestPage(@RequestParam int id,
                              @RequestParam String name,
                              @RequestParam String email,
                              @RequestParam String phone,
                              Model model) {
        List<User> userList = userService.search(id, name, email, phone);
        model.addAttribute("userList", userList);
        return "test";
    }
}
```

예를들어 위와 같이 `@RequestParam`을 이용해 일일이 사용자의 요청을 맵핑하는 경우를 먼저 살펴보겠습니다. 

이때, 사용자를 찾기 위한 검색 조건이 늘어나거나 줄어드는 변경이 발생되었을때의 문제점은 아래와 같습니다.

  

#### **1. 다수의 변경점**

변경점은 3군데입니다. 

1. handler의 매개변수
2. userService.search() 호출시 넘겨주는 매개변수
3. UserService 클래스의 search() 메소드의 시그니쳐

이들을 모두 일일이 변경하는것은 매우 번거로운 작업입니다.

  

#### 2. 매개변수의 순서

매개변수가 많아지는 경우, 다른 타입의 매개변수라면 컴파일러가 에러를 잡아줄테지만, 매개변수의 타입이 같은 경우, 순서가 바뀐다면 이는 매우 위험한 에러입니다.

  

  

### @ModelAttribute를 사용하는 경우

```java
@Getter
@Setter
public class UserSearchForm {
    private int id;
    private String name;
    private String email;
    private String phone;
}

@RestController
public class TestController {
    @GetMapping("/")
    public String getTestPage(@ModelAttribute UserSearchForm userSearchForm,
                              Model model) {
        List<User> userList = userService.search(userSearchForm);
        model.addAttribute("userList", userList);
        return "test";
    }
}
```

이렇게 `@ModelAttribute`를 사용하는 경우의 장점은 앞서 살펴본 단점을 해결해준다고 생각하면 됩니다.

만약 검색 조건이 추가되는 경우, UserSearchForm의 필드를 추가해주면, 핸들러를 수정할 필요도 없으며, UserService의 search() 메소드의 시그니처 역시 수정할 필요가 없습니다.


## @Transactional


xml 설정
```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     <property name="dataSource" ref="dataSource"/>
</bean>
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```



- 어노테이션을 이용한 설정
    

@Transactional을 클래스 단위 혹은 메서드 단위에 선언해주면 된다.

클래스에 선언하게 되면, 해당 클래스에 속하는 메서드에 공통적으로 적용된다. (test(), test2() 메서드에 적용)

메서드에 선언하게 되면, 해당 메서드에만 적용된다. (test() 메소드에 적용)


```java
//클래스 단위 설정
@Service
@Transactional
public class TestService {

	public void test() {

	}
	
	public void test2() {
		
	}

}
```


```java
//메소드 단위 설정
@Service
public class TestService {

	@Transactional
	public void test() {

	}

	public void test2() {

	}
}
```


**2. 동작 원리**

@Transactional 어노테이션을 기준으로 설명하겠다.

트랜잭션은 **Spring AOP**를 통해 구현되어있다. 

더 정확하게 말하면, 어노테이션 기반 AOP를 통해 구현되어있다. (import문을 보면 알 수 있다)

```java
import org.springframework.transaction.annotation.Transactional;
```

따라서, 아래와 같은 특징이 있다

- 클래스, 메소드에 @Transactional이 선언되면 해당 클래스에 트랜잭션이 적용된 프록시 객체 생성
- 프록시 객체는 @Transactional이 포함된 메서드가 호출될 경우, 트랜잭션을 시작하고 Commit or Rollback을 수행
- CheckedException or 예외가 없을 때는 Commit
- UncheckedException이 발생하면 Rollback

**3. 주의점**

**1) 우선순위**

@Transactional은 우선순위를 가지고 있다.

클래스 메서드에 선언된 트랜잭션의 우선순위가 가장 높고, 인터페이스에 선언된 트랜잭션의 우선순위가 가장 낮다.

```
클래스 메소드 -> 클래스 -> 인터페이스 메소드 -> 인터페이스
```

따라서 공통적인 트랜잭션 규칙은 클래스에, 특별한 규칙은 메서드에 선언하는 식으로 구성할 수 있다.

또한, 인터페이스 보다는 클래스에 적용하는 것을 권고한다.

- 인터페이스나 인터페이스의 메서드에 적용할 수 있다.
- 하지만, 인터페이스 기반 프록시에서만 유효한 트랜잭션 설정이 된다.
- 자바 어노테이션은 인터페이스로부터 상속되지 않기 때문에  **클래스 기반 프록시 or AspectJ 기반에서 트랜잭션 설정을 인식 할 수 없다.**

**2) 트랜잭션의 모드**

@Transactional은 Proxy Mode와 AspectJ Mode가 있는데 **Proxy Mode가 Default**로 설정되어있다.

Proxy Mode는 다음과 같은 경우 동작하지 않는다.

- 반드시 public 메서드에 적용되어야한다.
    - Protected, Private Method에서는 선언되어도 에러가 발생하지는 않지만, 동작하지도 않는다.
    - Non-Public 메서드에 적용하고 싶으면 AspectJ Mode를 고려해야한다.
- @Transactional이 적용되지 않은 Public Method에서 @Transactional이 적용된 Public Method를 호출할 경우, 트랜잭션이 동작하지 않는다.


**4. @Transactional 설정**


![[Transactional1.png]]


**5. 다중 Transaction Manager**

필요에 따라서 다수의 독립된 트랜잭션 매니져를 사용할 수 있다.

이는 @Transactional의 **value** 속성에 사용할 PlatformTransactionlManager를 지정하면 된다.

참고로 [**PlatformTransactionManager**](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html)는 Spring에서 제공하는  **트랜잭션 관리 인터페이스**이다.

인터페이스의 구현체에는 JDBC, 하이버네이트 등이 있는데 이는 트랜잭션이 실제 작동할 환경이다.

따라서 반드시 알맞은 PlatformTransactionlManager 구현체가 정의되어  있어야한다.

다시 본론으로 돌아와, 다중 트랜잭션 매니저는 다음과 같이 지정한다.

**value 속성에 Bean의 id or qualifier 값을 지정한다.** 

아래는 qualifier를 이용한 Spring Docs의 예제이다.

```java
public class TransactionalService {

    @Transactional("order")
    public void setSomething(String name) { ... }

    @Transactional("account")
    public void doSomething() { ... }
}

```

```xml
<bean id="transactionManager1" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	...
	<qualifier value="order"/>
</bean>

<bean id="transactionManager2" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	...
	<qualifier value="account"/>
</bean>
```

**6. 일반적인 사용 예제**

트랜잭션은 다음과 같은 경우 사용한다.

- 하나의 작업 or 여러 작업을
- 하나의 작업 단위로 묶어 Commit or Rollback 처리가 필요할 때

특히, **JPA**를 사용한다면 단일 작업에 대해서는 @Transactional을 직접 선언할 필요가 없다.

JPA의 구현체를 살펴보면 모든 메서드에 이미 @Transactional이 선언되어 있기 때문에 문제가 발생하면 Rollback 처리해주기 때문이다.



![[Transactional2.png]]




![[Transactional3.png]]


따라서, 여러 작업을 하나의 단위로 묶어 Commit or Rollback 처리가 필요할 때 직접 선언해주면 된다.

결과를 확인하기 위해 간단한 사용 예시를 만들었다.

test() 메서드에서 @Transactional 선언을 통해 saveSuccess()와 saveFail()을 하나의 작업 단위로 묶었다.

**saveSuccess() 메서드는 문제가 없지만, saveFail()에 문제가 생겨 saveSuccess() 역시 저장되지 않고 Rollback 된다.**



![[Transactional4.png]]



만약, @Trasactional을 선언하지 않는다면 saveFail()의 실패 여부와 관계없이 saveSuccess()는 저장된다.


![[Transactional5.png]]




![[Transactional6.png]]


DB에 saveSuccess()가 저장되었다.



---
출처 - https://velog.io/@ruinak_4127/Annotation%EC%9D%B4%EB%9E%80


https://velog.io/@jkijki12/annotation


https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4

https://galid1.tistory.com/769


https://imiyoungman.tistory.com/9 - @Transactional