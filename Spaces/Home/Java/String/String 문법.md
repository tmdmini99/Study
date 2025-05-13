



```java
"list".equalsIgnoreCase(responseType)
```


`responseType`이 **"list"** (대소문자 구분 없이)와 같다


## String.repeat()


```java
String phone_number = "01012345678";
int len = phone_number.length();

String answer = "*".repeat(len - 4) + phone_number.substring(len - 4);
System.out.println(answer);  // 출력: *******5678

```



자바에서 `"*".repeat(n)`은 **문자열을 n번 반복하는 기능**인데, 이 기능은 **Java 11 이상**에서 사용할 수 있음


```java
String result = "*".repeat(5);
System.out.println(result);  // 출력: *****
```

- `"*"`: 반복할 문자열
    
- `.repeat(5)`: 이 문자열을 **5번 반복**