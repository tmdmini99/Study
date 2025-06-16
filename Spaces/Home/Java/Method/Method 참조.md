

## Integer::sum

```java
public static int sum(int a, int b) {
    return a + b;
}
```

`Integer::sum`은 위 메서드를 참조하는 것입니다.  
즉, `(a, b) -> a + b`(람다 표현식) 를 간단히 표현한 **함수 참조**입니다.


Map.merge() 예제로 보기

```java
Map<String, Integer> map = new HashMap<>();

map.put("apple", 1);
map.merge("apple", 1, Integer::sum);  // 기존 값 1 + 새 값 1 = 2
map.merge("banana", 1, Integer::sum); // banana 없으므로 1 추가

System.out.println(map); // 👉 {apple=2, banana=1}
```

