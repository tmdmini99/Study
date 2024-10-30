### **[ Optional이란? ]**

Java8에서는 `Optional<T>` 클래스를 사용해 NPE를 방지할 수 있도록 도와준다. `Optional<T>`는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다. Optional 클래스는 아래와 같은 value에 값을 저장하기 때문에 값이 null이더라도 바로 NPE가 발생하지 않으며, 클래스이기 때문에 각종 메소드를 제공해준다.


```java
public final class Optional<T> {

  // If non-null, the value; if null, indicates no value is present
  private final T value;
   
  ...
}
```


## **2. Optional 활용하기**

---

### **[ Optional 생성하기 ]**

#### **Optional.empty() - **값이 Null인 경우****

Optional은 Wrapper 클래스이기 때문에 값이 없을 수도 있는데, 이때는 Optional.empty()로 생성할 수 있다.

```java
Optional<String> optional = Optional.empty();

System.out.println(optional); // Optional.empty
System.out.println(optional.isPresent()); // false
```

Optional 클래스는 내부에서 static 변수로 EMPTY 객체를 미리 생성해서 가지고 있다. 이러한 이유로 빈 객체를 여러 번 생성해줘야 하는 경우에도 1개의 EMPTY 객체를 공유함으로써 메모리를 절약하고 있다.

```java
public final class Optional<T> {

    private static final Optional<?> EMPTY = new Optional<>();
    private final T value;
    
    private Optional() {
        this.value = null;
    }

    ...

}
```

#### ****Optional.**of**() -** 값이 Null이 아닌 경우**

만약 어떤 데이터가 절대 null이 아니라면 Optional.of()로 생성할 수 있다. 만약 Optional.of()로 Null을 저장하려고 하면 NullPointerException이 발생한다.

```xml
// Optional의 value는 절대 null이 아니다.
Optional<String> optional = Optional.of("MyName");
```

#### ****Optional.ofNullbale() -** 값이 Null일수도, 아닐수도 있는 경우**

만약 어떤 데이터가 null이 올 수도 있고 아닐 수도 있는 경우에는 Optional.ofNullbale로 생성할 수 있다. 그리고 이후에 orElse 또는 orElseGet 메소드를 이용해서 값이 없는 경우라도 안전하게 값을 가져올 수 있다.

```java
// Optional의 value는 값이 있을 수도 있고 null 일 수도 있다.
Optional<String> optional = Optional.ofNullable(getName());
String name = optional.orElse("anonymous"); // 값이 없다면 "anonymous" 를 리턴
```

### **[ Optional 사용법 예시 코드 ]**

#### **Optional 사용법 예시 (1)**

기존에는 아래와 같이 null 검사를 한 후에 null일 경우에는 새로운 객체를 생성해주어야 했다. 이러한 과정을 코드로 나타내면 다소 번잡해보이는데, `Optional<T>`와 Lambda를 이용하면 해당 과정을 보다 간단하게 표현할 수 있다.

```java
// Java8 이전
List<String> names = getNames();
List<String> tempNames = list != null 
    ? list 
    : new ArrayList<>();

// Java8 이후
List<String> nameList = Optional.ofNullable(getNames())
    .orElseGet(() -> new ArrayList<>());
```

#### **Optional 사용법 예시 (2)**

예를 들어 아래와 같은 우편번호를 꺼내는 null 검사 코드가 있다고 하자. 이 코드는 null 검사 때문에 상당히 복잡하다.

```java
public String findPostCode() {
    UserVO userVO = getUser();
    if (userVO != null) {
        Address address = user.getAddress();
        if (address != null) {
            String postCode = address.getPostCode();
            if (postCode != null) {
                return postCode;
            }
        }
    }
    return "우편번호 없음";
}
```

하지만 Optional을 사용하면 이러한 코드를 아래와 같이 표현할 수 있다.

```java
public String findPostCode() {
    // 위의 코드를 Optional로 펼쳐놓으면 아래와 같다.
    Optional<UserVO> userVO = Optional.ofNullable(getUser());
    Optional<Address> address = userVO.map(UserVO::getAddress);
    Optional<String> postCode = address.map(Address::getPostCode);
    String result = postCode.orElse("우편번호 없음");

    // 그리고 위의 코드를 다음과 같이 축약해서 쓸 수 있다.
    String result = user.map(UserVO::getAddress)
        .map(Address::getPostCode)
        .orElse("우편번호 없음");
}
```

#### **Optional 사용법 예시 (3)**

예를 들어 아래와 같이 이름을 대문자로 변경하는 코드에서 NPE 처리를 해준다고 하자.

```java
String name = getName();
String result = "";

try {
    result = name.toUpperCase();
} catch (NullPointerException e) {
    throw new CustomUpperCaseException();
}
```

위의 코드는 다소 번잡하고 가독성이 떨어지는데 이를 Optional를 활용하면 아래와 같이 표현할 수 있다.

```java
Optional<String> nameOpt = Optional.ofNullable(getName());
String result = nameOpt.orElseThrow(CustomUpperCaseExcpetion::new)
                  .toUpperCase();
```


## **1. 언제 Optional을 사용해야 하는가?** 

---

### **[ Optional이 만들어진 이유와 의도 ]**

Java8부터 Null이나 Null이 아닌 값을 저장하는 컨테이너 클래스인 Optional이 추가되었다. Java 언어의 아키텍트(설계자)인 Brian Goetz는 Optional에 대해 다음과 같이 정의하였다.


위의 내용을 정리하면 Optional은 null을 반환하면 오류가 발생할 가능성이 매우 높은 경우에 '결과 없음'을 명확하게 드러내기 위해 메소드의 반환 타입으로 사용되도록 매우 제한적인 경우로 설계되었다는 것이다. 이러한 설계 목적에 부합하게 실제로 Optional을 잘못 사용하면 많은 부작용(Side Effect)이 발생하게 되는데, 어떠한 문제가 발생할 수 있는지 자세히 살펴보도록 하자.

### **[ Optional이 위험한 이유, Optional을 올바르게 사용해야 하는 이유 ]**

Optional을 사용하면 코드가 Null-Safe해지고, 가독성이 좋아지며 애플리케이션이 안정적이 된다는 등과 같은 얘기들을 많이 접할 수 있다. 하지만 이는 Optional을 목적에 맞게 올바르게 사용했을 때의 이야기이고, Optional을 남발하는 코드는 오히려 다음과 같은 부작용(Side-Effect)를 유발할 수 있다.

- NullPointerException 대신 NoSuchElementException가 발생함
- 이전에는 없었던 새로운 문제들이 발생함
- 코드의 가독성을 떨어뜨림
- 시간적, 공간적 비용(또는 오버헤드)이 증가함

#### **NullPointerException 대신 NoSuchElementException가 발생함**

만약 Optional로 받은 변수를 값이 있는지 판단하지 않고 접근하려고 한다면 NoSuchElementException가 발생하게 된다.

```java
Optional<User> optionalUser = ... ;

// optional이 갖는 value가 없으면 NoSuchElementException 발생
User user = optionalUser.get();
```

Null-Safe하기 위해 Optional을 사용하였는데, 값의 존재 여부를 판단하지 않고 접근한다면 NullPointerException는 피해도 NoSuchElementException가 발생할 수 있다.

#### **이전에는 없었던 새로운 문제들이 발생함**

Optional을 사용하면 문제가 되는 대표적인 경우가 직렬화(Serialize)를 할 때이다. 기본적으로 Optional은 직렬화를 지원하지 않아서 캐시나 메세지큐 등과 연동할 때 문제가 발생할 수 있다. Optional을 사용함으로써 오히려 새로운 문제가 발생할 수 있는 것이다.

```java
public class User implements Serializable {

    private Optional<String> name;

}
```

물론 Jackson처럼 라이브러리에서 Optional이 있을 경우 값을 wrap, unwrap 하도록 지원해주기도 하지만 해당 라이브러리의 스펙을 파악해야한다는 점 등에서 오히려 불편함을 느낄 수 있다.

참고로 Jackson 라이브러리는 jackson-modules-java8 project 프로젝트부터 Optional을 지원하여, empty일 경우에는 null을 값이 있는 경우에는 값을 꺼내서 처리하도록 지원하고 있다. 또한 Cacheable, CacheEvict, CachePut과 같은 Spring의 캐시 추상화 기술들은 Spring4부터 이러한 wrap, unwrap을 지원하고 있다.

#### **코드의 가독성을 떨어뜨림**

예를 들어 다음과 같이 Optional을 받아서 값을 꺼낸다고 하자. 우리는 앞에서 살펴본대로 optionalUser의 값이 비어있으면 NoSuchElementException가 발생할 수 있으므로 다음과 같이 값의 유무를 검사하고 꺼내도록 코드를 작성하였다.

```java
public void temp(Optional<User> optionalUser) {
    User user = optionalUser.orElseThrow(IllegalStateException::new);
    
    // 이후의 후처리 작업 진행...
}
```

그런데 위와 같은 코드는 또 다시 NPE를 유발할 수 있다. 왜냐하면 optionalUser 객체 자체가 null일 수 있기 때문이다. 그러면 우리는 이러한 문제를 해결하기 위해 다음과 같이 코드를 수정해야 한다.

```java
public void temp(Optional<User> optionalUser) {
    if (optionalUser != null && optionalUser.isPresent()) {
        // 이후의 후처리 작업 진행...
    }
    
    throw new IllegalStateException();
}
```

이러한 코드는 오히려 값의 유무를 2번 검사하게 하여 단순히 null을 사용했을 때보다 코드가 복잡해졌다. 그 외에도 Optional의 제네릭 타입 때문에도 불필요하게 코드 글자수까지 늘어났다. 이렇듯 Optional을 올바르게 사용하지 못하고 남용하면 오히려 가독성이 떨어질 수 있다.

#### **시간적, 공간적 비용(또는 오버헤드)이 증가함**

- 공간적 비용: Optional은 객체를 감싸는 컨테이너이므로 Optional 객체 자체를 저장하기 위한 메모리가 추가로 필요하다.
- 시간적 비용: Optional 안에 있는 객체를 얻기 위해서는 Optional 객체를 통해 접근해야 하므로 접근 비용이 증가한다.

어떤 글에서는 Optional을 사용하면 단순 값을 사용했을 때보다 메모리 사용량이 4배 정도 증가한다고 한다. 그 외에도 Optional은 Serializable 하지 않는 등의 문제가 있으므로 이를 해결하기 위해 추가적인 개발 시간이 소요될 수 있다.

하지만 위에서 설명한 예시들의 대부분은 Optional을 올바르게 사용하지 않았기 때문에 발생하는 것이다. Optional은 만들어진 의도가 상당히 명확한 만큼 목적에 맞게 올바르게 사용되어야 한다. 아래에서는 올바른 Optional 사용을 위한 가이드를 소개한다.

## **2. 올바른 Optional 사용법 가이드**

---

### **[ 올바른 Optional 사용법 가이드 ]**

- Optional 변수에 Null을 할당하지 말아라
- 값이 없을 때 Optional.orElseX()로 기본 값을 반환하라
- 단순히 값을 얻으려는 목적으로만 Optional을 사용하지 마라
- 생성자, 수정자, 메소드 파라미터 등으로 Optional을 넘기지 마라
- Collection의 경우 Optional이 아닌 빈 Collection을 사용하라
- 반환 타입으로만 사용하라

#### **Optional 변수에 Null을 할당하지 말아라**

Optional은 컨테이너/박싱 클래스일 뿐이며, Optional 변수에 null을 할당하는 것은 Optional 변수 자체가 null인지 또 검사해야 하는 문제를 야기한다. 그러므로 값이 없는 경우라면 Optional.empty()로 초기화하도록 하자.

```csharp
// AVOID
public Optional<Cart> fetchCart() {

    Optional<Cart> emptyCart = null;
    ...
}
```

#### **값이 없을 때 Optional.orElseX()로 기본 값을 반환하라**

Optional의 장점 중 하나는 함수형 인터페이스를 통해 가독성좋고 유연한 코드를 작성할 수 있다는 것이다. 가급적이면 isPresent()로 검사하고 get()으로 값을 꺼내기 보다는 orElseGet 등을 활용해 처리하도록 하자.

```java
private String findDefaultName() {
    return ...;
}

// AVOID
public String findUserName(long id) {

    Optional<String> optionalName = ... ;

    if (optionalName.isPresent()) {
        return optionalName.get();
    } else {
        return findDefaultName();
    }
}

// PREFER
public String findUserName(long id) {

    Optional<String> optionalName = ... ;
    return optionalName.orElseGet(this::findDefaultName);
}
```

orElseGet은 값이 준비되어 있지 않은 경우, orElse는 값이 준비되어 있는 경우에 사용하면 된다. 만약 null을 반환해야 하는 경우라면 orElse(null)을 활용하도록 하자. 만약 값이 없어서 throw해야하는 경우라면 orElseThrow를 사용하면 되고 그 외에도 다양한 메소드들이 있으니 적당히 활용하면 된다.

추가적으로 Java9 부터는 ifPresentOrElse도 지원하고 있으며, Java 10부터는 orElseThrow()의 기본으로 NoSuchElementException()를 던질 수 있다. 만약 Java8이나 9를 사용중이라면 명시적으로 넘겨주면 된다.

#### **단순히 값을 얻으려는 목적으로만 Optional을 사용하지 마라**

단순히 값을 얻으려고 Optional을 사용하는 것은 Optional을 남용하는 대표적인 경우이다. 이러한 경우에는 굳이 Optional을 사용해 비용을 낭비하는 것 보다는 직접 값을 다루는 것이 좋다.

```java
// AVOID
public String findUserName(long id) {
    String name = ... ;
    
    return Optional.ofNullable(name).orElse("Default");
}

// PREFER
public String findUserName(long id) {
    String name = ... ;
    
    return name == null 
      ? "Default" 
      : name;
}
```

#### **생성자, 수정자, 메소드 파라미터 등으로 Optional을 넘기지 마라**

Optional을 파라미터로 넘기는 것은 상당히 의미없는 행동이다. 왜냐하면 넘겨온 파라미터를 위해 자체 null체크도 추가로 해주어야 하고, 코드도 복잡해지는 등 상당히 번거로워지기 때문이다. Optional은 반환 타입으로 대체 동작을 사용하기 위해 고안된 것임을 명심해야 하며, 앞서 설명한대로 Serializable을 구현하지 않으므로 필드 값으로 사용하지 않아야 한다.

```java
// AVOID
public class User {

    private final String name;
    private final Optional<String> postcode;

    public Customer(String name, Optional<String> postcode) {
        this.name = Objects.requireNonNull(name, () -> "Cannot be null");
        this.postcode = postcode;
    }

    public Optional<String> getName() {
        return Optional.ofNullable(name);
    }
    
    public Optional<String> getPostcode() {
        return postcode;
    }
}
```

Optional을 접근자에 적용하는 경우도 마찬가지이다. 위의 예시에서 name을 얻기 위해 Optional.ofNullable()로 반환하고 있는데, Brian Goetz는 Getter에 Optional을 얹어 반환하는 것을 두고 남용하는 것이라고 얘기하였다. 


#### **Collection의 경우 Optional이 아닌 빈 Collection을 사용하라**

Collection의 경우 굳이 Optional로 감쌀 필요가 없다. 오히려 빈 Collection을 사용하는 것이 깔끔하고, 처리가 가볍다.

```java
// AVOID
public Optional<List<User>> getUserList() {
    List<User> userList = ...; // null이 올 수 있음

    return Optional.ofNullable(items);
}

// PREFER
public List<User> getUserList() {
    List<User> userList = ...; // null이 올 수 있음

    return items == null 
      ? Collections.emptyList() 
      : userList;
}
```

아래의 경우도 사용을 피해야 하는 케이스이다. Optional은 비싸기 때문에 최대한 사용을 지양해야 한다. 아래의 케이스라면 map에 getOrDefault 메소드가 있으니 이걸 활용하는 것이 훨씬 좋다.

```java
// AVOID
public Map<String, Optional<String>> getUserNameMap() {
    Map<String, Optional<String>> items = new HashMap<>();
    items.put("I1", Optional.ofNullable(...));
    items.put("I2", Optional.ofNullable(...));
    
    Optional<String> item = items.get("I1");
    
    if (item == null) {
        return "Default Name"
    } else {
        return item.orElse("Default Name");
    }
}

// PREFER
public Map<String, String> getUserNameMap() {
    Map<String, String> items = new HashMap<>();
    items.put("I1", ...);
    items.put("I2", ...);
    
    return items.getOrDefault("I1", "Default Name");
}
```

#### **반환 타입으로만 사용하라**

Optional은 반환 타입으로써 에러가 발생할 수 있는 경우에 결과 없음을 명확히 드러내기 위해 만들어졌으며, Stream API와 결합되어 유연한 체이닝 api를 만들기 위해 탄생한 것이다. 예를 들어 Stream API의 findFirst()나 findAny()로 값을 찾는 경우에 어떤 것을 반환하는게 합리적일지 Java 언어를 설계하는 사람이 되어 고민해보자. 언어를 만드는 사람의 입장에서는 Null을 반환하는 것보다 값의 유무를 나타내는 객체를 반환하는 것이 합리적일 것이다. Java 언어 설계자들은 이러한 고민 끝에 Optional을 만든 것이다.

그러므로 Optional이 설계된 목적에 맞게 반환 타입으로만 사용되어야 한다.




---

출처- https://mangkyu.tistory.com/70
https://mangkyu.tistory.com/203