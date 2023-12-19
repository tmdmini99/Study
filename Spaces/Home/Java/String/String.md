
String 메소드

**1) String**

먼저 String과 다른 클래스(StringBuffer, StringBuilder)의 차이점은 두 문자열 클래스의 아주 기본적인 차이는 String은 immutable(불변), StringBuffer는 mutable(변함)에 있습니다. 

  

String은 문자열을 대표하는 것으로 문자열을 조작하는 경우 유용하게 사용할 수 있습니다. 문자열, 숫자, char 등은 concat할때는 StringBuffer, StringBuilder를 사용할 수 있습니다. 단, 복잡한 경우 의미가 있고, 단순한 경우에는 굳이 StringBuffer, StringBuilder를 쓰지 않고 +연산자를 활용해 직접 합지면 됩니다.

  

String 객체는 한번 생성되면 할당된 메모리 공간이 변하지 않습니다. + 연산자 또는 concat 메서드를 통해 기존에 생성된 String 클래스 객체 문자열에 다른 문자열을 붙여도 기존 문자열에 새로운 문자열을 붙이는 것이 아니라,

  
새로운 String 객체를 만든 후, 새 String 객체에 연결된 문자열을 저장하고, 그 객체를 참조하도록 합니다. (즉, String 클래스 객체는 Heap 메모리 영역(가비지 컬렉션이 동작하는 영역)에 생성. 한번 생성된 객체의 내부 내용을 변화시킬 수 없습니다. 기존 객체가 제거되면 Java의 가비지 컬렉션이 회수합니다.)


String 객체는 이러한 이유로 문자열 연산이 많은 경우, 그 성능이 좋지 않습니다.  
하지만, Immutable한 객체는 간단하게 사용가능하고, 동기화에 대해 신경쓰지 않아도 되기때문에(Thread-safe),  내부 데이터를 자유롭게 공유 가능합니다.



## **🤔 toCharArray()란?**

String 문자열을 char형 배열로 바꿔서 반환해주는 메서드이다.

"ABCD" 라는 문자열이 있으면

arr[0] = 'A'

arr[1] = 'B'

arr[2] = 'C'

arr[3] = 'D'

위 값처럼 char 배열을 반환해준다.

# Char (Character)를 String 으로, String을 Char로 변환하기

문제를 해결해나가다 보면 String을 Char로 변환해 아스키코드 등으로 연산을 마친 후 다시 String타입으로 리턴하고 싶을 때가 많다. 

```java
String temp = "캐릭터 변환하기";

char[] array = temp.toCharArray();

String change = "";

for (int j = 0; j < array.length; j++) {

    change+= Character.toString(array[j]);

}

System.out.println(change);
```

char를 String으로 변환
**String.valueOf()** 메소드에 매개변수로 char[]를 넣으면, String으로 곧바로 변환해준다.

```java
String arrayString = String.valueOf(ary);
```






### String.valueof() 와 .toString의 차이점


두 메소드 모두 Object의 값을 String으로 변환하지만 변경하고자 하는Object가 null인 경우 다르다.

**toString()과 같은 경우 [[Exception|Null PointerException]](NPE)을 발생**시키지만 **valueOf는 "null"이라는 문자열로 처리**한다.

  

즉 비교해서 정리하자면

- String.valueOf() - 파라미터가 null이면 문자열 "null"을 만들어서 반환한다.
    
- toString() - 대상 값이 null이면 NPE를 발생시키고 Object에 담긴 값이 String이 아니여도 출력한다.

이런 차이점 때문에 valueOf의 null체크 방법은 "null".equals(string) 형태로 체크를 해야한다.

null로 인해 발생된 에러는 시간이 지나고, 타인의 소스인경우 디버깅하기 어렵고 **어떤의미를 내포하고 있는지 판단하기 어렵다.** 때문에 NPE를 방지하기 위해 toString보다는 **valueOf를 사용하는 것을 추천**한다.

```java
destItemMap.get("LOWER_VAL") 이 null 일 경우

String lowerCoatingVal1 = String.valueOf(destItemMap.get("LOWER_VAL"));

String lowerCoatingVal2 = destItemMap.get("LOWER_VAL").toString();

lowerCoatingVal1 = "null"

lowerCoatingVal2 = NullPointerException 발생

String.valueOf()의 null 체크

String lowerCoatingVal1 = String.valueOf(destItemMap.get("LOWER_VAL"));

if("null".equals(lowerCoatingVal1)) {

    // To do Somting....

}

// equals함수를 사용할때 왼쪽에 있는 것을 기준으로 비교하기 때문에 변수보다는 문자열을 왼쪽에 두는 것을 추천한다.

// 즉 strTestVal이 null인 경우 ret = "1"인 if문은 NPE를 발생시킨다.

String strTestVal = null;

String ret = "";

/* Exception 발생 */

if( !(strTestVal .equals("")) ) ret = "1";

/* 정상 */

if( !("".equals(strTestVal)) ) ret = "2";
```



참조 - https://swjeong.tistory.com/146 String.valueof() 와 .toString의 차이점

