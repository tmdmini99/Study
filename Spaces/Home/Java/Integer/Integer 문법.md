

## Integer.parseInt()

받은 문자열을 정수로 바꾼다.

```java
String str = "14";
Integer.parseInt(str);
```


## Integer.toBinaryString()

int형을 2진수로 바꾸고 *문자열*로 저장한다.

```java
String str = Integer.toBinaryString(14);
System.out.println(str); //"1110"
```



## Integer 람다

```java
Integer.toString(n).chars().sorted().forEach(c -> res = Character.valueOf((char)c) + res);
```


```java
Integer.toString(n)           // 정수를 문자열로 변환 (예: 321 -> "321")
    .chars()                  // IntStream 생성 (각 문자의 유니코드 정수값 스트림)
    .sorted()                 // 오름차순 정렬
    .forEach(c ->             // 람다 표현식 시작 (각 문자 코드에 대해 실행)
        res = Character.valueOf((char)c) + res  // res 앞에 문자 추가
    );

```

- `n = 321`일 경우 → `"321"` → 문자 코드 스트림: `'3'`, `'2'`, `'1'` → 정렬: `'1'`, `'2'`, `'3'`
    
- `res`라는 문자열 앞에 차례대로 붙이기 때문에: `res = "3" + "2" + "1"` → `"321"` 처럼 뒤집힌 결과
    

> 결국 숫자의 각 자릿수를 오름차순으로 정렬해서 **거꾸로(res 앞에 붙이기)** 저장하는 코드입니다.


## Integer.bitCount(n)


```java
int count = Integer.bitCount(n); // n을 2진수로 변환 후 1의 갯수
```
