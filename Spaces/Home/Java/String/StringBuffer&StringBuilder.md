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


## **String vs (StringBuffer, StringBuilder) 비교**

### **문자열 자료형의 불변과 가변**

#### **String은 불변**

기본적으로 자바에서는 String 객체의 값은 변경할 수 없다.

이는 한번 할당된 공간이 변하지 않는다고 해서 '불변(immutable)' 자료형 이라고 불리운다. 그래서 초기공간과 다른 값에 대한 연산에서 많은 시간과 자원을 사용하게 된다는 특징이 있다.

실제로 String 객체의 내부 구성 요소를 보면 다음과 같이 되어 있다.

인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 value 라는 인스턴스 변수에 문자형 배열로 저장되게 된다. 이 value 라는 변수는 상수(final)형이니 값을 바꾸지 못하는 것이다.

```java
public final class String implements java.io.Serializable, Comparable {
	private final byte[] value;
}
```


> 
> Tip
> 
> jdk 8 까지는 String 객체의 값은 char[] 배열로 구성되어져 있지만, jdk 9부터 기존 char[]에서 byte[]을 사용하여 String Compacting을 통한 성능 및 heap 공간 효율(2byte -> 1byte)을 높이도록 수정되었다.

아래 예제 코드를 보면 변수 ~~a~~ 가 참조하는 메모리의 "Hello" 라는 값에 "World" 라는 문자열을 더해서 String 객체의 자체의 값을 업데이트 시킨 것으로 보일수 있다.

하지만 실제로는 메모리에 새로 "Hello World" 값을 저장한 영역을 따로 만들고 변수 ~~a~~ 를 다시 참조하는 식으로 작동한다.

```java
String str = "hello";
str = str + " world";

System.out.println(str); // hello world
```

[![java-string](https://blog.kakaocdn.net/dn/OngYs/btrJNkniOBK/Kj8NYtK9neoy1JKqxs8Yh1/img.png)](https://blog.kakaocdn.net/dn/OngYs/btrJNkniOBK/Kj8NYtK9neoy1JKqxs8Yh1/img.png)

이외에도 문자열을 다루는데 있어 가장 많이 사용하는 trim 이나toUpperCase 메소드 사용 형태를 보면, 문자열이 변경되는 것 처럼 생각 될 수도 있지만 해당 메소드 수행 시 또 다른 String 객체를 생성하여 리턴할 뿐이다.

```java
String sql = "abc";  // "abc"
sql.toUpperCase();  // "ABC"

System.out.println(sql); // "abc" - toUpperCase를 해도 자체 문자열은 변경되지 않는다 (불변)
```


> Info
> 
> 
> **[ 자바 언어에서 String을 불변으로 설정한 이유 ]**  
> String 객체를 불변하게 설계한 이유는 캐싱, 보안, 동기화, 성능측면 이점을 얻기 위해서이다.  
>   
> 1. 캐싱 : String을 불변하게 함으로써 String pool에 각 리터럴 문자열의 하나만 저장하며 다시 사용하거나 캐싱에 이용가능하며 이로 인해 힙 공간을 절약할 수 있다는 장점이 있다.   
>   
> 2. 보안 : 예를 들어 데이터베이스 사용자 이름, 암호는 데이터베이스 연결을 수신하기 위해 문자열로 전달되는데, 만일 번지수의 문자열 값이 변경이 가능하다면 해커가 참조 값을 변경하여 애플리케이션에 보안 문제를 일으킬 수 있다.  
>   
> 3. 동기화 : 불변함으로써 동시에 실행되는 여러 스레드에서 안정적이게 공유가 가능하다.
> 
#### **StringBuffer / StringBuilder 는 가변**

StringBuffer나 StringBuilder의 경우 문자열 데이터를 다룬다는 점에서 String 객체와 같지만, 객체의 공간이 부족해지는 경우 버퍼의 크기를 유연하게 늘려주어 가변(mutable)적이라는 차이점이 있다.

두 클래스는 내부 Buffer(데이터를 임시로 저장하는 메모리)에 문자열을 저장해두고 그 안에서 추가, 수정, 삭제 작업을 할 수 있도록 설계되어 있다.

String 객체는 한번 생성되면 불변적인 특징 때문에 값을 업데이트하면, 매 연산 시마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리공간을 차지하게 되지만, 

StringBuffer / StringBuilder 는 가변성 가지기 때문에 .append().delete() 등의 API를 이용하여 동일 객체내에서 문자열 크기를 변경하는 것이 가능하다.

[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/bs470d/btrJQ2zSLaW/vphjS7LM84BKxI5jnSy9B0/img.png)](https://blog.kakaocdn.net/dn/bs470d/btrJQ2zSLaW/vphjS7LM84BKxI5jnSy9B0/img.png)

따라서 값이 변경될 때마다 새롭게 객체를 만드는 String 보다 훨씬 빠르기 때문에, 문자열의 추가, 수정, 삭제가 빈번하게 발생할 경우라면 String 클래스가 아닌 StringBuffer / StringBuilder를 사용하는 것이 이상적이라 말할 수 있다.

> Tip
> StringBuilder 나 StringBuffer 클래스의 사용 문법은 둘이 똑같다.  
> 다만 내부적으로 동작 차이점 존재하는데 이에 대해선 바로 뒤에서 다룬다. 지금은 둘이 동일시로 봐도 된다.
StringBuffer의 내부구조를 보면 상수(final) 키워드가 없는것을 볼 수 있다.


```java
public final class StringBuffer implements java.io.Serializable {
	private byte[] value;
}
```


```java
StringBuffer sb = new StringBuffer(); // new StringBuffer() 인수에 아무것도 넣어주지 않으면 기본 16으로 배열 길이를 잡음

// sb.capacity() - StringBuffer 변수의 배열 용량의 크기 반환
System.out.println(sb.capacity()); // 16 

sb.append("1111111111111111111111111111111111111111"); // 40길이의 문자열을 append
System.out.println(sb.capacity()); // 40 (추가된 문자열 길이만큼 늘어남)
```

아래 코드는 별* 문자를 루프문을 순회할때마다 추가해 길다란 별 문자열을 만드는 예제로, 각각 String 객체와 StringBuffer 객체를 이용했을시 차이를 보여준다.



```java
String star = "*";
		
for ( int i = 1; i < 10; i++ ) {
	star += "*";
}
```



```java
StringBuffer sb= new StringBuffer("*");
sb.append("*********");
```

  

[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/cF9aG4/btrJCoh7Ggl/ssXtC4Hk2uMj2Wk52MnwmK/img.png)](https://blog.kakaocdn.net/dn/cF9aG4/btrJCoh7Ggl/ssXtC4Hk2uMj2Wk52MnwmK/img.png)[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/b5yFcL/btrJAqV28gw/TB6UBb7LZLxxoPbyZI83KK/img.png)](https://blog.kakaocdn.net/dn/b5yFcL/btrJAqV28gw/TB6UBb7LZLxxoPbyZI83KK/img.png)

String 객체일 경우 매번 별 문자열이 업데이트 될때마다 계속해서 메모리 블럭이 추가되게 되고, 일회용으로 사용된 이 메모리들은 후에 Garbage Collector(GC)의 제거 대상이 되어 빈번하게 Minor GC를 일으켜 Full GC(Major Gc)를 일으킬수 있는 원인이 된다. 
반면 StringBuffer는 위 사진 처럼 자체 메모리 블럭에서 늘이고 줄이고를 할수 있기 때문에 훨씬더 효율적으로 문자열 데이터를 다룰 수 있다는 것을 볼 수 있다.

---

### ****문자열 자료형의 값 비교****

#### **String 값 동등 비교**

String 객체는 간단하게 equals() 메서드를 통해 문자열 데이터 동등 비교가 가능하다.



```java
String str1 = "Hello"; // 문자열 리터럴을 이용한 방식
String str3 = new String("Hello"); // new 연산자를 이용한 방식
 
// 리터럴과 객체 문자열 비교
System.out.println(str1 == str3); // false
System.out.println(str3.equals(str1)); // true
```

#### **StringBuffer / StringBuilder 값 동등 비교** 

하지만 StringBuffer/StringBuilder 클래스는 String 객체와 달리 ~~equals()~~ 메서드를 오버라이딩하지 않아 '==' 로 비교한 것과 같은 결과를 얻게 되 버린다.



```java
StringBuffer sb = new StringBuffer("hello");
StringBuffer sb2 = new StringBuffer("hello");

System.out.println(sb == sb2); // false
System.out.println(sb2.equals(sb)); // false
```

따라서 toString() 으로 StringBuffer/StringBuilder 객체를 String 객체로 변환하고 난 뒤에 equals 로 비교를 해야 한다. (약간 번거롭다)



```java
// StringBuffer객체를 toString()을 통해 String객체화를 하고 equals 비교
String sb_tmp = sb.toString();
String sb2_tmp = sb2.toString();
System.out.println(sb_tmp.equals(sb2_tmp)); // true
```

---

### ****문자열 자료형의 성능 비교****

#### **문자열 합치기 성능**

자바를 개발 하다보면 문자열을 다루는데 있어, 별다른 고민없이 "Hello" + " World" 와 같이 + 기호를 통해 문자열을 이어 붙이곤 한다. 하지만 참된 Java 개발자라면 고민을 더 해보고 문자열 관련 Class를 선택해야한다.

위에서 알아봤듯이, String 데이터를 + 연산하면 불필요한 객체들이 힙 메모리에 추가되어 안좋기 때문에  String을 직접 + 연산을 통한 문자열 합치기를 지양하고 StringBuffer나 StringBuilder의 append() 메소드를 통해 문자열 데이터를 추가하는 것이 좋다고 한다.

하지만 이는 **반은 맞고 반은 틀린 말**이다.

사실 자바는 문자열에 + 연산을 사용하면, 컴파일 전 내부적으로 StringBuilder 클래스를 만든 후 다시 문자열로 돌려준다고 한다.

[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/bYHqK3/btrJCoJdVdL/2b6a2stQ8xSE3KlU0UMfD1/img.png)](https://blog.kakaocdn.net/dn/bYHqK3/btrJCoJdVdL/2b6a2stQ8xSE3KlU0UMfD1/img.png)

즉, "hello" + "world" 문자열 연산이 있다면 이는 new StringBuilder("hello").append("world").toString() 과 같다는 말이다.



```java
String a = "hello" + "world";
/* 는 아래와 같다. */
String a = new StringBuilder("hello").append("world").toString(); 
// StringBuilder를 통해 "hello" 문자열을 생성하고 "world"를 추가하고 toString()을 통해 String 객체로 변환하여 반환
```

이처럼 겉으로는 보기에는 문자열 리터럴로 + 연산하거나, StringBuilder 객체를 사용하거나 어차피 자동 변환해줘서 차이가 없어 보일지도 모른다.

하지만 다음과 같이 문자열을 합치는 일이 많을 경우 단순히 + 연산을 쓰면 성능과 메모리 효율이 떨어지게 된다.



```java
String a = "";

for(int i = 0; i < 10000; i++) {
    a = a + i;
}

/* 위의 문자열 + 연산 식은 결국 아래와 같다. */
/* 즉, 매번 new StringBuilder() 객체 메모리를 생성하고 다시 변수에 대입하는 멍청한 짓거리를 하고 있는 것이다. */

String a = "";

for(int i = 0; i < 10000; i++) {
    a = new StringBuilder(b).append(i).toString();
}
```

위의 예시 처럼 매번 new StringBuilder() 객체 메모리를 생성하고 다시 변수에 대입하는 멍청한 짓을 반복하고 있으니 한눈에 봐도 문자열 값을 변경할 일이 잦을 경우 + 연산자를 사용하는 것은 좋지 못한 것을 알 수 있다.

따라서 만일 문자열 연산이 많을 경우 아예 처음부터 StringBuilder() 객체로 문자열을 생성해서 다루는게 훨씬 낫다.



```java
StringBuilder a = new StringBuilder();

for(int i = 0; i < 10000; i++) {
    a.append(i);
}

final String b = a.toString();
```

#### **String.concat 과의 비교**

자바 프로그래밍에서 문자열을 합치는데 있어 총 4가지 방법이 존재한다.

+ 연산자 또는 String.concat 메소드 이용 또는 StringBuffer 또는 StringBuilder 객체를 이용하는 방법이다.



```java
String str = "hello ";

String a = str + "world"; // 앞서 + 연산은 자동으로 StringBuilder로 변환된다고 말했다.
String b = str.concat("world");
String c = new StringBuffer("hello").append("world").toString();
String d = new StringBuilder("hello").append("world").toString();
```

이처럼 String 객체 자체에도 String.concat 메소드로 자체 메모리 영역에서 문자열 데이터를 변경이 가능하다.

그럼 이중에 어느 것이 성능이 좋을까? 당연히 승자는 StringBuffer 와 StringBuilder 객체이다.

String.concat 같은 경우, 이 메소드는 호출할 때마다 원본 문자열의 매번 배열을 재구성 하는 과정을 거치기 때문에 당연히 느릴 수밖에 없다. 반면 StringBuilder나 StringBuffer는 처음부터 배열 크기를 일정하게 잡고 시작하기 때문에 합치는 과정이 String.concat 보다 월등히 빠르다.

[![String.concat-stringbuilder-stringbuffer](https://blog.kakaocdn.net/dn/dkVybJ/btrJSXR5xT3/K9qcr6WzHL9NJoUbYxXWk1/img.png)](https://blog.kakaocdn.net/dn/dkVybJ/btrJSXR5xT3/K9qcr6WzHL9NJoUbYxXWk1/img.png)

#### **성능상에서 문자열 자료형 선택 결론**

그렇다면 무조건 StringBuffer / StringBuilder를 사용하는 것이 좋을다고 맹신할수 있겠지만, 그건 상황에 따라 다르다.

StringBuffer나 StringBuilder을 생성할 경우 buffer의 크기를 초기에 설정해줘야하는데 이러한 동작으로 인해 무거운 편에 속한다. 그리고 StringBuffer나 StringBuilder에서 문자열 수정을 할 경우에도 마찬가지로 버퍼의 크기를 늘리고 줄이고 명칭을 변경해야하는 내부적인 연산이 필요하므로 **많은 양의 문자열 수정**이 아니라면 String 객체를 사용하는것이 오히려 나을 수 있다.

String 클래스는 크기가 고정되어 있으므로 **단순하게 읽는 조회 연산**에서는 StringBuffer나 StringBuilder 클래스보다 빠르게 읽을 수 있다.

따라서 정리하자면, **문자열 추가나 변경등의 작업이 많을 경우에는 StringBuffer**를, **문자열 변경 작업이 거의 없는 경우에는 그냥 String**을 사용하는 것만 기억하면 된다.

---

## **StringBuffer vs StringBuilder 차이점**

StringBuffer와 StringBuilder 클래스는 둘 다 크기가 유연하게 변하는 가변적인 특성을 가지고 있으며, 제공하는 메서드도 똑같고 사용하는 방법도 동일하다.

그럼 이 둘의 차이점은 무엇일까? 어렵게 생각할 필요없이, 사실 둘의 차이는 **딱 한가지**이다.  
바로 **멀티 쓰레드(Thread)에서 안전(safe)하냐 아니냐** 이 차이 뿐이다.

### **쓰레드의 안정성**

[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/cHSE1K/btrJDhwhFLY/kCCeH9MJoDukajQxTsgIu1/img.png)](https://blog.kakaocdn.net/dn/cHSE1K/btrJDhwhFLY/kCCeH9MJoDukajQxTsgIu1/img.png)

- StringBuffer 클래스는 쓰레드에서 안전하다. (thread safe)
- StringBuilder 클래스는 쓰레드에서 안전하지 않다.(thread unsafe) 

두 클래스는 문법이나 배열구성도 모두 같지만, **동기화**(**Synchronization**)에서의 지원의 유무가 다르다. 

**StringBuilder**는 동기화를 지원하지 않는 반면, **StringBuffer**는 동기화를 지원하여 멀티 스레드 환경에서도 안전하게 동작할 수 있다.

그 이유는 StringBuffer는 메서드에서 synchronized 키워드를 사용하기 때문이다.

> 
> Tip
> 
> 
> java에서 synchronized 키워드는 여러개의 스레드가 한 개의 자원에 접근할려고 할 때, 현재 데이터를 사용하고 있는 스레드를 제외하고 나머지 스레드들이 데이터에 접근할 수 없도록 막는 역할을 수행한다.
> 

글로만 보면 이게 정확히 어떤 현상인지 와닿지 않는다. 직접 StringBuilder와 StringBuffer를 코드로 테스트 해보자. 

아래의 예제는 StringBuilder와 StringBuffer 객체를 선언하고, 두개의 멀티 쓰레드를 돌려 StringBuilder와 StringBuffer 객체에 각각 1 요소를 1만번 추가하는(append) 로직을 수행한 코드이다.

두개의 쓰레드가 있고 한개의 쓰레드에서 배열요소를 1만번 추가하니 문자열 배열의 길이는 20000이 된다고 유추할 수 있다.



```java
import java.util.*;

public class Main extends Thread{
  public static void main(String[] args) {
    StringBuffer stringBuffer = new StringBuffer();
    StringBuilder stringBuilder = new StringBuilder();

    new Thread(() -> {
        for(int i=0; i<10000; i++) {
            stringBuffer.append(1);
            stringBuilder.append(1);
        }
    }).start();

    new Thread(() -> {
        for(int i=0; i<10000; i++) {
            stringBuffer.append(1);
            stringBuilder.append(1);
        }
    }).start();

    new Thread(() -> {
        try {
            Thread.sleep(2000);

            System.out.println("StringBuffer.length: "+ stringBuffer.length()); // thread safe 함
            System.out.println("StringBuilder.length: "+ stringBuilder.length()); // thread unsafe 함
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }).start();
  }
}
```



```shell
StringBuffer.length: 20000
StringBuilder.length: 19628
```

하지만 위의 결과를 보면 다른 값이 나온 것을 볼 수 있다.

StringBuilder의 값이 더 작은 것을 볼 수 있는데, 이는 쓰레드들이 동시에 StringBuilder 클래스에 접근하여 동시에 append() 를 수행하다 몇번 씹혀서 제대로 수행이 안되어 일어난 결과라고 보면 된다.

StringBuilder는 Thread safe 하지 않아서 각기 쓰레드가 객체에 접근해서 변경을 하면 기다려주지 않기 때문에 이러한 현상이 발생한 것이다.

이와 달리 StringBuffer는 멀티 쓰레드(multi thread)환경에서, 한 쓰레드가 append() 를 수행하고 있을경우 다른 쓰레드가 append() 를 수행을 동시에 하지못하도록 잠시 대기를 시켜주고 순차적으로 실행하게 한다. 이처럼 동시에 접근해 다른 값을 변경하지 못하도록 하므로 Thread Safe로서 정상적으로 2만개의 배열요소가 추가된 것이다.

그래서 web이나 소켓환경과 같이 **비동기로 동작하는 경우가 많을 때는 StringBuffer를 사용**하는 것이 안전하다는 것을 알수가 있다.

---

### **순수 성능 비교**

그럼 순수하게 이 두 클래스의 성능은 누가 우월할까?

다음은 String 객체 그리고 StringBuffer와 StringBuilder 객체를 선언하고 **5만번을 루프해 별문자 * 를 추가**하는 로직이다. 그리고 각 문자열 연산을 실행하는 시간을 duration1, duration2, duration3 변수에 계산해 저장한다.



```java
final int lengths = 50000;

// ------------- (1) String의 +연산을 이용해서 50,000개의 *를 이어 붙인다.
long startTime1 = System.currentTimeMillis(); // 시작시간을 기록 (millisecond단위)

String str="";
for(int i=0;i<lengths;i++){
    str=str+"*";
}

long endTime1 = System.currentTimeMillis(); // 종료시간을 기록(millisecond단위)


// ------------- (2) StringBuffer를 이용해서 50,000개의 *를 이어붙인다.              
long startTime2 = System.currentTimeMillis(); 

StringBuffer sb = new StringBuffer();
for(int i=0;i<lengths;i++){
    sb.append("*");
}

long endTime2 = System.currentTimeMillis(); 


// ------------- (3) StringBuilder를 이용해서 50,000개의 *를 이어붙인다.              
long startTime3 = System.currentTimeMillis(); 

StringBuilder sb2 = new StringBuilder();
for(int i=0;i<lengths;i++){
    sb2.append("*");
}

long endTime3 = System.currentTimeMillis(); 


// ------------- 방법(1), 방법(2), 방법(3)가 걸린 시간을 비교
long duration1 = endTime1 - startTime1;
long duration2 = endTime2 - startTime2;
long duration3 = endTime3 - startTime3;

System.out.println("String의 +연산을 이용한 경우 : "+ duration1); // 559
System.out.println("StringBuffer의 append()을 이용한 경우 : "+ duration2); // 10
System.out.println("StringBuilder의 append()을 이용한 경우 : "+ duration3); // 3
```



```shell
String의 +연산을 이용한 경우 : 559
StringBuffer의 append()을 이용한 경우 : 10
StringBuilder의 append()을 이용한 경우 : 3
```

문자열 연산 수행 시간 결과를 보면, 기본 성능은 StringBuilder 클래스가 우월하다는 것을 알 수 있다.

위에서 살펴보았던 + 연산 시, 컴파일 전에 StringBuilder 로 변환해주는 이유가 바로 이것이다.

이는 조금만 생각해보면 당연한 결과이다.

StringBuffer와 StringBuilder 의 차이는 쓰레드 안정성에 있다고 학습했는데, 아무래도 쓰레드 안전성을 버린 StringBuilder가 좀더 덜 따지고 연산을 하니 당연히 좀 더 빠를 수 밖에 없다.

[![java-StringBuffer-StringBuilder](https://blog.kakaocdn.net/dn/lPDkD/btrJMBiSxRl/cAxkMIIfw2RKoxcbmyV6k0/img.png)](https://blog.kakaocdn.net/dn/lPDkD/btrJMBiSxRl/cAxkMIIfw2RKoxcbmyV6k0/img.png)

위 그래프를 보면 10만번 이상의 연산시 String 객체는 수행시간이 기하급수적으로 늘어나지만, StringBuilder와 StringBuffer는 1000만번까지 버티며, 그 이상은 StringBuilder가 우월하다는 것을 볼 수 있다.

그래서 만약 싱글 쓰레드 환경에서나 비동기를 사용할 일이 없으면, StringBuilder를 쓰는 것이 이상적이라 할 수 있다.

하지만 **현업**에서는 자바 어플리케이션을 대부분 멀티 스레드 이상의 환경에서 돌아가기 때문에 **왠만하면 안정적인** **StringBuffer로 통일하여 코딩**하는것이 좋다. (솔직히 StringBuffer 와 StringBuilder 속도 차이는 거의 미미하다)

---

## **문자열 자료형 비교 총정리**

|   |   |   |   |
|---|---|---|---|
|**String**|**StringBuffer**|**StringBuilder**|
|가변 여부|불변|가변|가변|
|스레드 세이프|O|O|X|
|연산 속도|느림|빠름|아주 빠름|
|사용 시점|문자열 추가 연산이 적고, 스레드 세이프 환경에서|문자열 추가 연산이 많고, 스레드 세이프 환경에서|문자열 추가 연산이 많고, 빠른 연산이 필요한 경우  <br>단일 스레드 환경일경우|

- **String 을 사용해야 할 때 :**
    
    - String은 불변성
    - 문자열 연산이 적고 변하지 않는 문자열을 자주 사용할 경우
    - 멀티쓰레드 환경일 경우 
    
- **StringBuilder 를 사용 해야 할 때 :**
    
    - StringBuilder는 가변성
    - 문자열의 추가, 수정, 삭제 등이 빈번히 발생하는 경우
    - 동기화를 지원하지 않아, 단일 쓰레드이거나 동기화를 고려하지 않아도 되는 경우
    - 속도면에선 StringBuffer 보다 성능이 좋다.
    
- **StringBuffer 를 사용해야 할 때 :**
    
    - StringBuffer는 가변성
    - 문자열의 추가, 수정, 삭제 등이 빈번히 발생하는 경우
    - 동기화를 지원하여, 멀티 스레드 환경에서도 안전하게 동작




---
참조- https://12bme.tistory.com/42