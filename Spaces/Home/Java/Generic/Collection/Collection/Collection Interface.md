



### `removeIf(...)` → `java.util.Collection` 인터페이스의 **default 메서드**

- Java 8부터 지원
    
- `Predicate<T>` 조건을 만족하는 요소를 **삭제**
    
- 하나라도 삭제되면 `true` 반환, 없으면 `false`


```java
boolean removed = dataValues.removeIf(id -> {
    try {
        return idList.contains(Integer.parseInt(id.trim()));
    } catch (NumberFormatException e) {
        return false;
    }
});
```



# ✅ java.util.Collection 인터페이스의 Default 메서드 (Java 8 기준)

Java 8부터 `Collection` 인터페이스에 추가된 `default` 메서드는 다음과 같습니다:

| 메서드명        | 시그니처                                                              | 설명 |
|----------------|------------------------------------------------------------------------|------|
| `removeIf`     | `default boolean removeIf(Predicate<? super E> filter)`                | 조건에 맞는 요소를 제거하고, 제거된 항목이 있으면 `true` 반환 |
| `spliterator`  | `default Spliterator<E> spliterator()`                                 | 병렬 처리를 위한 `Spliterator` 객체 반환 |
| `stream`       | `default Stream<E> stream()`                                           | 순차 스트림 반환 |
| `parallelStream` | `default Stream<E> parallelStream()`                                 | 병렬 스트림 반환 |

---

## ✅ 예제

### 📌 removeIf
```java
List<String> list = new ArrayList<>(Arrays.asList("a", "b", "c"));
list.removeIf(s -> s.equals("b")); // "b" 제거
System.out.println(list); // [a, c]
```

---

### 📌 spliterator
```java
Collection<String> list = List.of("a", "b", "c");
Spliterator<String> spliterator = list.spliterator();
spliterator.forEachRemaining(System.out::println);
```

---

### 📌 stream
```java
List<String> list = List.of("apple", "banana", "cherry");
list.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);
// APPLE, BANANA, CHERRY
```

---

### 📌 parallelStream
```java
List<String> list = List.of("apple", "banana", "cherry");
list.parallelStream()
    .forEach(System.out::println); // 병렬 출력 (순서 보장 X)
```


