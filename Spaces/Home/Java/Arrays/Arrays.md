
## Arrays

```java
import java.util.Arrays;
import java.util.Comparator;
```


### Arrays.sort

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 9, 1, 5, 6};

        // 배열을 오름차순으로 정렬
        Arrays.sort(numbers);

        // 정렬된 배열 출력
        System.out.println(Arrays.toString(numbers));  // [1, 2, 5, 5, 6, 9]
    }
}

```

### 2. **`Arrays.sort()`의 기본 동작**

- 기본적으로 **숫자 배열**은 **작은 값부터 큰 값으로 정렬**됩니다.
    
- **문자열 배열**은 **사전 순**으로 정렬됩니다.
    
    - 예: `Arrays.sort(new String[] {"banana", "apple", "cherry"})`는 `["apple", "banana", "cherry"]`로 정렬됩니다.
        

### 3. **`Arrays.sort()`를 사용한 **`배열의 객체 정렬`**

배열이 **객체**일 경우, 객체를 정렬하려면 객체가 **`Comparable`** 인터페이스를 구현해야 하며, **`compareTo()`** 메서드를 사용하여 비교할 수 있어야 합니다.

예를 들어, **`String`**은 **`Comparable`** 인터페이스를 구현하여 기본적으로 사전순으로 정렬됩니다. 그러나 **`Person`**과 같은 사용자 정의 객체를 정렬하려면, 해당 객체가 **`Comparable`**을 구현해야 합니다.


```java
import java.util.Arrays;

class Person implements Comparable<Person> {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Person other) {
        return this.age - other.age; // 나이 기준으로 오름차순 정렬
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        Person[] people = {
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35)
        };

        // Person 객체들을 나이 순으로 정렬
        Arrays.sort(people);

        // 정렬된 결과 출력
        System.out.println(Arrays.toString(people));
    }
}
```


### 4. **`Arrays.sort()`의 복잡도**

- `Arrays.sort()`는 **Dual-Pivot QuickSort** 알고리즘을 사용하여 평균적인 **O(n log n)** 시간 복잡도를 가집니다. 이는 대부분의 정렬 알고리즘보다 빠릅니다.
    
- `Arrays.sort()`가 기본으로 사용하는 정렬 방식은 내부적으로 최적화되어 있어 **일반적인 배열 정렬 작업에서 매우 효율적**입니다.
    

### 5. **내림차순 정렬**

`Arrays.sort()`는 기본적으로 오름차순으로 정렬되지만, 내림차순으로 정렬하려면 **`Comparator`**를 사용해야 합니다.


```java
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    public static void main(String[] args) {
        Integer[] numbers = {5, 2, 9, 1, 5, 6};

        // 내림차순으로 정렬
        Arrays.sort(numbers, Comparator.reverseOrder());

        // 정렬된 배열 출력
        System.out.println(Arrays.toString(numbers));  // [9, 6, 5, 5, 2, 1]
    }
}
```


### 6. **`Arrays.sort()`의 다양한 사용법 요약**

- **기본 정렬 (오름차순)**: `Arrays.sort(int[] array)`
    
- **문자열 배열 정렬 (사전순 정렬)**: `Arrays.sort(String[] array)`
    
- **객체 배열 정렬**: 객체가 **`Comparable`**을 구현하면 자동으로 정렬이 가능, 아니면 **`Comparator`**를 사용하여 정렬할 수 있습니다.
    
- **내림차순 정렬**: `Arrays.sort(array, Comparator.reverseOrder())`
    

### 7. **예외 처리**

배열에 **`null`** 값이 포함된 경우 `Arrays.sort()`는 **`NullPointerException`**을 던질 수 있습니다. 따라서, 배열에 **`null`** 값이 포함될 가능성이 있는 경우에는 `Comparator`를 이용해 정렬할 수 있도록 처리하는 것이 좋습니다.

java

복사편집

`Integer[] array = {5, 2, null, 1}; Arrays.sort(array, Comparator.nullsFirst(Comparator.naturalOrder()));`

위 코드는 `null` 값이 먼저 오도록 정렬합니다.

### Arrays.asList

### 1. **`Arrays.asList(array)`**

`Arrays.asList()`는 배열을 **`List`**로 변환하는 메서드입니다. 배열을 바로 리스트로 사용할 수는 없지만, `Arrays.asList()`를 사용하면 배열을 **고정 크기**의 리스트로 변환할 수 있습니다. 이 리스트는 **배열의 요소를 그대로 참조**하는 리스트입니다.
```java
String[] array = {"apple", "banana", "cherry"};
List<String> list = Arrays.asList(array);
```


### 2. **`contains(value)`**

`contains(value)`는 **`List`** 인터페이스에 정의된 메서드로, 리스트에 특정 **값**이 포함되어 있는지 확인합니다. 이 메서드는 **`boolean`** 값을 반환하며, 값이 존재하면 `true`, 그렇지 않으면 `false`를 반환합니다.

```java
list.contains("banana");  // true
list.contains("orange");  // false
```


### 3. **전체 흐름**

`Arrays.asList(array)`로 배열을 리스트로 변환하고, 그 리스트에서 `contains(value)`를 사용하여 특정 값이 있는지 확인하는 방식입니다.


```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // 배열 선언
        String[] fruits = {"apple", "banana", "cherry"};
        
        // 배열을 List로 변환
        List<String> fruitList = Arrays.asList(fruits);
        
        // 특정 값이 있는지 확인
        System.out.println(fruitList.contains("banana")); // true
        System.out.println(fruitList.contains("orange")); // false
    }
}
```

### 4. **배열과 리스트 차이점**

- `Arrays.asList(array)`로 만든 **리스트**는 **고정 크기**의 리스트입니다. 즉, 배열을 리스트로 변환했지만, 리스트의 크기를 변경할 수는 없습니다. 예를 들어, `add()`나 `remove()`를 사용하여 요소를 추가하거나 제거할 수 없습니다.
    
- 그러나 `contains(value)`는 `List`의 메서드이므로 배열을 리스트로 변환한 후, 리스트에서 해당 값을 **효율적으로** 검색할 수 있습니다.


### 5. **주의할 점**

- `Arrays.asList()`로 변환한 리스트는 고정 크기 리스트입니다. 예를 들어, `fruitList.add("orange")`를 시도하면 `UnsupportedOperationException`이 발생합니다. 배열의 크기를 변경하고 싶다면 `ArrayList`를 사용해야 합니다.


```java
List<String> mutableList = new ArrayList<>(Arrays.asList(fruits)); // 변경 가능한 리스트
mutableList.add("orange");  // 가능
```


```java
Arrays.asList(array).contains(value);
```

이렇게 사용 가능


## Collections


### Collections.swap

```java
import java.util.*;

public class SwapExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C", "D"));
        
        System.out.println("Before swap: " + list);
        
        // 인덱스 1과 3의 요소를 바꿈 ("B" <-> "D")
        Collections.swap(list, 1, 3);
        
        System.out.println("After swap: " + list);
    }
}

```

### Collections.min

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(5, 2, 8, 1, 4);
        
        // 가장 작은 값 찾기
        int minValue = Collections.min(list);
        
        System.out.println("가장 작은 값: " + minValue);  // 1
    }
}
```
