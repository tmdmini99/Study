

## Character.compare(char c, char ch);

`Character.compare(s1.charAt(n), s2.charAt(n))` 는 두 문자를 비교해서 **정수값**을 반환하는 메서드예요. `compareTo()`와 비슷하지만 **`char` 타입**을 다룰 때 더 명확하게 쓸 수 있어요.


```java
Character.compare(char1, char2)
```

- `char1 < char2` → 음수 반환
    
- `char1 == char2` → 0 반환
    
- `char1 > char2` → 양수 반환




```java
String s1 = "car";
String s2 = "cat";

// 세 번째 문자 'r' vs 't'
int result = Character.compare(s1.charAt(2), s2.charAt(2));
System.out.println(result); // 음수 → 'r' < 't'

```


| 비교 방식                 | 비교 대상  | 결과                 |
| --------------------- | ------ | ------------------ |
| `compareTo()`         | 문자열 전체 | 사전순 비교             |
| `Character.compare()` | 단일 문자  | 유니코드 값 비교 (작으면 음수) |