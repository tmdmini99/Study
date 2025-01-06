**Java `enum`**(열거형)은 **상수를 그룹화**하고, 각 상수에 고유한 이름과 값을 부여할 수 있는 **데이터 타입**입니다. `enum`은 Java 5부터 도입되었으며, 주로 **상수 집합을 표현**할 때 사용됩니다.

`enum`은 **클래스가 아닌 특별한 데이터 타입**입니다. `enum`은 **상수 집합을 표현하기 위해 사용하는 특수한 클래스**입니다. 그래서 **메서드로는 `enum`을 만들 수 없고**, 반드시 **클래스 수준**에서 선언해야 합니다.




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

```plaintext
Status: ACTIVE
Description: 사용 가능
```


## **5. `switch`문과 `enum` 사용**

`enum`은 `switch`문과도 잘 어울립니다.


```java
public class EnumSwitchExample {
    public static void main(String[] args) {
        Day today = Day.FRIDAY;

        switch (today) {
            case MONDAY:
                System.out.println("It's Monday! Start of the week.");
                break;
            case FRIDAY:
                System.out.println("It's Friday! Weekend is near.");
                break;
            default:
                System.out.println("It's another day.");
        }
    }
}
```


## **6. `enum`의 장점**

- **가독성 향상**: 의미 있는 이름으로 상수를 표현할 수 있음.
- **타입 안정성 보장**: `enum` 타입 외의 값을 사용할 수 없음.
- **데이터의 일관성 유지**: 상수 집합을 고정.
- **메서드와 속성 추가 가능**: 단순한 상수 이상의 정보를 저장 가능.



## **7. `enum` vs. 상수 (`final static`)**

| 특징             | `enum`              | `final static` 상수 |
| -------------- | ------------------- | ----------------- |
| **타입 안정성**     | 강함 (컴파일 타임 체크)      | 약함 (오타 가능)        |
| **가독성**        | 우수 (의미 있는 이름 사용)    | 제한적 (단순한 상수만 사용)  |
| **데이터 추가 가능성** | 가능 (필드, 메서드 포함)     | 제한적 (메서드 사용 불가)   |
| **정렬 및 순서 관리** | 가능 (`ordinal()` 사용) | 불가능               |
| **코드 유지보수**    | 우수                  | 제한적               |

## **언제 `enum`을 사용해야 하나요?**

- **상수가 고정적일 때**: 요일, 상태값, 성별, 계절 등.
- **상수에 추가 속성이 필요할 때**: `status`와 같이 설명이 필요할 때.
- **타입 안정성이 중요할 때**: `switch`나 특정 데이터만 허용할 때.



## 실제 사용


```java
public enum CommonAttribute {
    CATEGORY("category", "categoryList"),
    ARTISTS("artists", "artistsList"),
    MEDIA("media", "mediaList"),
    LABELS("labels", "labelList"),
    TAGS("tags", "tagsList"),
    COMPANIES("companies", "companiesList"),
    STATUS_CD("status_cd", "statusList");

    private final String targetTable;
    private final String attrName;

    CommonAttribute(String targetTable, String attrName) {
        this.targetTable = targetTable;
        this.attrName = attrName;
    }

    public String getTargetTable() {
        return targetTable;
    }

    public String getAttrName() {
        return attrName;
    }
}
```


**2. `addAttribute`와 `addAttributeTranslates` 메서드 (Enum 활용)**

```java
public void addAttribute(Model model, CommonAttribute attribute) {
    model.addAttribute(attribute.getTargetTable(), attribute.getAttrName());
}

public void addAttributeTranslates(Model model, CommonAttribute attribute) {
    model.addAttribute(attribute.getTargetTable(), attribute.getAttrName());
}
```

사용 예제

```java
public String getKWM1201List(@ModelAttribute("BasicParamVo") BasicParamVo paramVo, Model model) {
    log.info("KWM1201 ::: " + paramVo);
    // 모든 공통 속성 추가
    for (CommonAttribute attribute : CommonAttribute.values()) {
        addAttribute(model, attribute);
    }
    return "/order/KWM1201";
}

public String getAnotherPage(@ModelAttribute("BasicParamVo") BasicParamVo paramVo, Model model) {
    log.info("AnotherPage ::: " + paramVo);
    // 특정 속성만 추가
    addAttributeTranslates(model, CommonAttribute.ARTISTS);
    addAttributeTranslates(model, CommonAttribute.MEDIA);
    return "/order/AnotherPage";
}
```

4. 특정 속성만 추가 (개별 사용 가능)

```java
public String getSpecialPage(@ModelAttribute("BasicParamVo") BasicParamVo paramVo, Model model) {
    addAttribute(model, CommonAttribute.CATEGORY); // 카테고리만 추가
    addAttributeTranslates(model, CommonAttribute.ARTISTS); // 아티스트만 추가
    return "/order/SpecialPage";
}
```

### **3. Enum의 다른 활용 (Map을 사용한 검색 기능 추가)**

만약 **targetTable** 기준으로 `CommonAttribute`를 검색하는 기능이 필요하다면, 아래와 같이 `Map`을 추가할 수도 있습니다.


```java
import java.util.HashMap;
import java.util.Map;

public enum CommonAttribute {
    CATEGORY("category", "categoryList"),
    ARTISTS("artists", "artistsList"),
    MEDIA("media", "mediaList");

    private static final Map<String, CommonAttribute> lookup = new HashMap<>();

    static {
        for (CommonAttribute attr : CommonAttribute.values()) {
            lookup.put(attr.targetTable, attr);
        }
    }

    private final String targetTable;
    private final String attrName;

    CommonAttribute(String targetTable, String attrName) {
        this.targetTable = targetTable;
        this.attrName = attrName;
    }

    public String getTargetTable() { return targetTable; }
    public String getAttrName() { return attrName; }

    // targetTable로 enum 찾기
    public static CommonAttribute getByTargetTable(String targetTable) {
        return lookup.get(targetTable);
    }
}
```


사용 예시:

```java
CommonAttribute attr = CommonAttribute.getByTargetTable("category");
System.out.println(attr.getAttrName());  // categoryList
```


## 클래스 내부에 선언

```java
public class CommonAttributeHandler {

    // ✅ 클래스 내부에 선언된 enum (Nested Enum)
    public enum CommonAttribute {
        CATEGORY("category", "categoryList"),
        ARTISTS("artists", "artistsList");

        private final String targetTable;
        private final String attrName;

        CommonAttribute(String targetTable, String attrName) {
            this.targetTable = targetTable;
            this.attrName = attrName;
        }

        public String getTargetTable() {
            return targetTable;
        }

        public String getAttrName() {
            return attrName;
        }
    }

    // ✅ 내부 enum을 순회하여 Model에 데이터를 추가하는 메서드
    public void addAttributesToModel(Model model) {
        for (CommonAttribute attribute : CommonAttribute.values()) {
            model.addAttribute(attribute.getTargetTable(), attribute.getAttrName());
        }
    }
}
```



## 외부 클래스에 선언



```java
public class OrderController {
    private final CommonAttributeHandler commonAttributeHandler = new CommonAttributeHandler();

    public void addAttributesToModel(Model model) {
        commonAttributeHandler.addAttributesToModel(model);
    }

    public void addSingleAttribute(Model model) {
        model.addAttribute(
            CommonAttributeHandler.CommonAttribute.CATEGORY.getTargetTable(),
            CommonAttributeHandler.CommonAttribute.CATEGORY.getAttrName()
        );
    }
}

```


## **4. `enum`을 클래스 내부에 선언할 때의 장점**

1. **캡슐화 강화**:
    - 해당 클래스와 강하게 결합된 상수를 외부에서 직접 접근하지 못하게 합니다.
2. **의미 명확화**:
    - 해당 클래스의 로직과 강하게 관련된 상수임을 명확하게 표현합니다.
3. **코드 정리**:
    - 관련 데이터와 로직을 하나의 클래스 내에 유지하여 코드 유지보수가 용이해집니다.