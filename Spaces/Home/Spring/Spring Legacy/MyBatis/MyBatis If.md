
```xml
<if test="dateKey != null and !dateKey.equals('')">
```



```xml
<if test="!''.equals(dateKey)">
```

앞에 문자열.equlas()이렇게 쓰면 자동으로 null 체크도 해줌
xml 파일에서는 불가능

java에서 가능


### **자바에서 동작하는 이유**

자바 코드에서 다음과 같이 실행하면 정상적으로 `null`을 처리할 수 있음:


```java
String search = null;
System.out.println(!"".equals(search)); // ✅ NullPointerException 발생!
```

위 코드에서 `search`가 `null`이면 `"".equals(null)` 실행 중 **`NullPointerException`이 발생**함.

하지만 안전하게 쓰면:

```java
System.out.println(search != null && !"".equals(search)); // ❌ false (예외 발생 X)
```

이렇게 하면 **정확하게 null과 빈 문자열을 모두 필터링할 수 있음**.

### **하지만 MyBatis XML에서는 동작하지 않을 가능성이 큼**

MyBatis에서는 `<if>` 문 내부에서 `"".equals(sc_SEARCH)` 같은 **메서드 호출이 비정상적으로 동작할 수 있음**. 즉, `sc_SEARCH == null`일 때 `"".equals(sc_SEARCH)`를 MyBatis가 해석하면서 **NullPointerException이 발생하지 않지만, 조건문이 무조건 `true`로 동작할 수도 있음.**

즉, **MyBatis에서는 `NULL`을 정확히 체크하는게 중요함.**

```xml
<if test="sc_SEARCH != null and sc_SEARCH.trim() != ''">
```
