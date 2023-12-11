
String 메소드

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

