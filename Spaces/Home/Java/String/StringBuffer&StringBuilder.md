## **StringBuffer / StringBuilder 클래스**

StringBuffer / StringBuilder 클래스는 문자열을 연산(추가하거나 변경) 할 때 주로 사용하는 자료형이다.

물론 String 자료형만 으로도, ~~+~~ 연산이나 ~~concat()~~ 메소드로 문자열을 이어붙일수 있다.

하지만 덧셈(+) 연산자를 이용해 String 인스턴스의 문자열을 결합하면, 내용이 합쳐진 새로운 String 인스턴스를 생성하게 되어, 따라서 문자열을 많이 결합하면 결합할수록 공간의 낭비뿐만 아니라 속도 또한 매우 느려지게 된다는 단점이 있다.


```java
String result = "";
result += "hello";
result += " ";
result += "jump to java";
System.out.println(result); // hello jump to java
// → 심플하지만 연산 속도가 느리다는 단점이 있다
```
그래서 자바에서는 이러한 문자열 연산을 전용으로 하는 자료형을 따로 만들어 제공해주는데, StringBuffer 클래스는 내부적으로 버퍼(buffer)라고 하는 독립적인 공간을 가지게 되어, 문자열을 바로 추가할 수 있어 공간의 낭비도 없으며 문자열 연산 속도도 매우 빠르다는 특징이 있다.

```java
StringBuffer sb = new StringBuffer();  // StringBuffer 객체 sb 생성
sb.append("hello");
sb.append(" ");
sb.append("jump to java");
String result = sb.toString();
System.out.println(result); // hello jump to java
// → + 연산보다는 복잡해 보이지만 연산 속도가 빠르다는 장점이 있다
```

StringBuffer와 비슷한 자료형으로 StringBuilder 자료형이 있다.  
StringBuilder 사용법은 StringBuffer와 동일하다.  
뒤에서 자세히 다루겠지만 StringBuffer와 StringBuilder의 차이는, StringBuffer는 멀티 스레드 완경에서 안전하다는 장점이 있고, StringBuilder는 문자열 파싱 성능이 가장 우수하다는 장점이 있다.

```java
StringBuffer sb = new StringBuffer(); // 기본 16 버퍼 크기로 생성

// sb.capacity() - StringBuffer 변수의 배열 용량의 크기 반환
System.out.println(sb.capacity()); // 16 
 
sb.append("1111111111111111111111111111111111111111"); // 40길이의 문자열을 append
System.out.println(sb.capacity()); // 40 (추가된 문자열 길이만큼 늘어남)
```
### **StringBuffer 내장 메소드**
```java
String str = "abcdefg";
StringBuffer sb = new StringBuffer(str); // String -> StringBuffer

System.out.println("처음 상태 : " + sb); // 처음상태 : abcdefg

System.out.println("문자열 String 변환 : " + sb.toString()); // StringBuffer를 String으로 변환하기

System.out.println("문자열 추출 : " + sb.substring(2,4)); // 문자열 추출하기

System.out.println("문자열 추가 : " + sb.insert(2,"추가")); // 문자열 추가하기

System.out.println("문자열 삭제 : " + sb.delete(2,4)); // 문자열 삭제하기

System.out.println("문자열 연결 : " + sb.append("hijk")); // 문자열 붙이기

System.out.println("문자열의 길이 : " + sb.length()); // 문자열의 길이구하기

System.out.println("용량의 크기 : " + sb.capacity()); // 용량의 크기 구하기

System.out.println("문자열 역순 변경 : " + sb.reverse()); // 문자열 뒤집기

System.out.println("마지막 상태 : " + sb); // 마지막상태 : kjihgfedcba
```


|메서드|설  명|
|---|---|
|StringBuffer( )|버퍼의 길이를 지정하지 않으면 크기가  16 인 버퍼를 생성|
|StringBuffer(int length)|length 길이를 가진  StringBuffer 클래스의 인스턴스(buffer)를 생성|
|StringBuffer(String str)|지정한 문자열( str )의 길이보다  16 만큼 더 큰 버퍼를 생성|
|StringBuffer append(boolean b)<br>StringBuffer append(char c)<br>StringBuffer append(char[ ] str) <br>StringBuffer append(double d)<br>StringBuffer append(float f) <br>StringBuffer append(int i) <br>StringBuffer append(long l)<br>StringBuffer append(Object obj)<br>StringBuffer append(String str)| 매개변수로 입력된 값을 문자열로 변환하여  StringBuffer 인스턴스가 저장하고 있는 문자열의 뒤에 덧붙임|
|int capacity( )|StringBuffer 인스턴스의 버퍼크기 반환 (자료형의 할당된 크기를 반환)|
|int length( )|StringBuffer 인스턴스에 저장된 문자열의 길이 반환 (버퍼에 담긴 문자열 데이터를 반환)|
|char charAt(int index)| 지정된 위치( index )에 있는 문자를 반환|
|StringBuffer delete(int start, int end)|시작위치( start )부터 끝 위치( end )사이에 있는 문자를 제거  <br>단,  end 위치의 문자는 제외(start <= x < end)|
|StringBuffer deleteCharAt(int index)|지정된 위치( index )의 문자를 제거|
|StringBuffer insert(int pos, boolean b)  <br>|StringBuffer insert(int pos, char c)  <br>|StringBuffer insert(int pos, char[ ] str)  <br>|StringBuffer insert(int pos, doule d)  <br>|StringBuffer insert(int pos, float f)  <br>|StringBuffer insert(int pos, int i)  <br>|StringBuffer insert(int pos, long l)  <br>|StringBuffer insert(int pos, Object obj)  <br>|StringBuffer insert(int pos, String str)| 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치( pos )에 추가  <br>( pos 는 0부터 시작)|
|StringBuffer replace(int start, int end, String str)| 지정된 범위( start  ~  end )의 문자들을 주어진 문자열로 바꾼다. 단,  end 의 위치는 범위에 포함되지 X (start <= x < end)|
|StringBuffer reverse( )|StringBuffer 인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열|
|void setCharAt(int index, char ch)|지정된 위치( index )의 문자를 주어진 문자( ch )로 바꾼다.|
|void setLength(int newLength)| 지정된 길이로 문자열의 길이를 변경  <br>길이를 늘리는 경우에는 나머지 빈공간들을 널문자( \u0000 )로 채운다.|
|String toString( )| StringBuffer 인스턴스의 문자열을  String 으로 변환|
|String substring(int start)  String substring(int start, int end)| 지정된 범위 내의 문자열을  String 으로 뽑아서 반환  <br>(시작위치( start )만 지정하면 시작위치부터 끝까지 뽑아서 반환)|



---
참조- https://12bme.tistory.com/42