
## **어노테이션이란? (Annotation)**

- 어노테이션은 다른 프로그램에게 유용한 정보를 제공하기 위해 사용되는 것으로 주석과 같은 의미를 가진다.
- 주석과는 그 역할이 다르지만 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다
*메타데이터 : 데이터에 대한 데이터, 어떤 목적을 가지고 만들어진 데이터, 다른 데이터를 설명해주는 데이터, 콘텐츠에 부여되는 데이터

# 📌어노테이션의 종류
- 표준(내장) 어노테이션 : 자바가 기본적으로 제공해주는 어노테이션
- 메타 어노테이션 : 어노테이션을 위한 어노테이션
- 사용자정의 어노테이션 : 사용자가 직접 정의하는 어노테이션


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

- SOURCE : 소스 파일에만 존재.
- RUNTIME : 클래스 파일에 존재. 실행시에 사용가능

```java

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override{}

```

### 📌@Documented

> javadoc으로 작성한 문서에 포함시키려면 해당 어노테이션을 붙인다.

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




---
출처 - https://velog.io/@ruinak_4127/Annotation%EC%9D%B4%EB%9E%80


https://velog.io/@jkijki12/annotation