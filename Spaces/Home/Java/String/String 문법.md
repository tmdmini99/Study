



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



## String 한글자씩 가져오기


```java
String xString = Integer.toString(x);
        
int sum = 0;
for (int i = 0; i < s.length(); i++) {
    int digit = s.charAt(i) - '0';
    System.out.println(digit);
}
```


## String.format()


```java
String b = String.format("%05d", Integer.parseInt(Integer.toBinaryString(14))); System.out.print(b); // 01110
```

%d -정수
%f -실수
%s -문자열
%**5**d - 5자리 표현
-> 뒤에값이 5자리 미만이라면 공백으로표시
%**0**5d - 공백대신 0으로 표시
받은 정수를 형식에 맞춰서 string에 저장한다.

​