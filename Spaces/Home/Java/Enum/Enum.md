**Java `enum`**(열거형)은 **상수를 그룹화**하고, 각 상수에 고유한 이름과 값을 부여할 수 있는 **데이터 타입**입니다. `enum`은 Java 5부터 도입되었으며, 주로 **상수 집합을 표현**할 때 사용됩니다.

## **1. 기본 사용법 (상수 정의)**

`enum`은 특정 값의 **집합**을 정의하는 데 사용됩니다. 예를 들어, 요일을 나타내는 `enum`을 정의할 수 있습니다.

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

사용 예시:

```java
public class EnumExample {
    public static void main(String[] args) {
        Day today = Day.MONDAY;
        System.out.println("Today is: " + today); // Today is: MONDAY
    }
}
```


## **2. `enum`의 특징**

- **불변 상수 집합**: `enum`은 미리 정의된 상수를 다룹니다.
- **타입 안정성 보장**: `enum` 타입으로 선언된 변수는 해당 `enum`에 정의된 값만 사용할 수 있습니다.
- **자동 `toString()` 제공**: `enum` 값을 `System.out.println()`으로 출력하면 **이름 그대로** 출력됩니다.
- **정렬 가능 (`ordinal`)**: 상수의 순서를 기반으로 인덱스를 가짐. (`0`부터 시작)

`ordinal()` 메서드 (인덱스 가져오기)

```java
System.out.println(Day.MONDAY.ordinal()); // 출력: 0
System.out.println(Day.FRIDAY.ordinal()); // 출력: 4
```


## **3. `enum`의 메서드 사용 (`values()`와 `valueOf()`)**

### **`values()`**: 모든 열거 상수를 배열로 반환


```java
for (Day day : Day.values()) {
    System.out.println(day);
}
```

**`valueOf()`**: 문자열을 `enum`으로 변환

```java
Day day = Day.valueOf("FRIDAY");
System.out.println("Selected day: " + day); // Selected day: FRIDAY
```


## **4. `enum`에 필드와 메서드 추가하기**

`enum`은 **필드**와 **메서드**를 가질 수 있습니다. 이를 사용하여 **상수에 고유한 속성**을 추가할 수 있습니다.


```java
public enum Status {
    ACTIVE("사용 가능"),
    INACTIVE("사용 불가능"),
    PENDING("대기 중");

    private final String description;

    // 생성자
    Status(String description) {
        this.description = description;
    }

    // Getter 메서드
    public String getDescription() {
        return description;
    }
}
```

사용 예시:

```java
public class EnumWithFields {
    public static void main(String[] args) {
        Status status = Status.ACTIVE;
        System.out.println("Status: " + status);
        System.out.println("Description: " + status.getDescription());
    }
}
```

출력:



