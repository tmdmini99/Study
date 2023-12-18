
### hashcode()
- 객체가 가지는 **고유의 값**을 반환하는 메소드
- Object 클래스의 기본 메소드는 객체의 메모리 주소값을 해시코드로 만들어 반환한다
- [[Map|HashMap]]과 같은 자료구조에서 동등성 비교를 위해 사용된다.
### equals()
- 객체가 가진 정보로 객체의 동등성을 비교한 결과를 나타내는 메소드
- Object 클래스의 기본 메소드는 객체의 메모리 주소값을 비교한다
    - `==` 연산도 메모리 주소값을 비교한다

## Object
위의 정의처럼 객체마다 주소값, 즉 해시코드가 다르게 나올까?

```
Object o1 = new Object();
Object o2 = new Object();

System.out.println(o1.hashCode());  // -> 1361960727
System.out.println(o2.hashCode());  // -> 739498517

System.out.println(o1.equals(o2));  // -> false
```

위 코드를 실행시켜보면 두 객체의 해시코드가 다르게 나오는 것을 확인할 수 있다.  
재정의하지않은 해시코드는 주소값을 나타내기 때문에, 재정의하지 않은 equals 또한 동일한 결과를 나타낸다. (결국 주소값을 비교한것이기 때문에 `==`과 같은 결과인 것)

위의 정의를 본 다음 다음 예제를 살펴보자.

## String
그럼 String의 경우에는 어떨지 테스트해보자

```
String s1 = "a";
String s2 = "a"

System.out.println(s1.hashCode());  // -> 97
System.out.println(s2.hashCode());  // -> 97

System.out.println(s1.equals(s2));  // -> true
```

엥??? String의 경우에는 다른 객체여도 해시코드가 동일하게 나온다.  
equals 결과도 true로 찍힌다.

왜 이런일이 발생하는 것일까? 먼저 String의 hashcode를 살펴보자.

String의 hashcode

```
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        hash = h = isLatin1() ? StringLatin1.hashCode(value)
                              : StringUTF16.hashCode(value);
    }
    return h;
}
```

?? 해시코드 함수를 재정의한 것을 알 수 있다.  
일단 여기서 순수 주소값만 해시코드로 만들지 않는다는 것을 알 수 있다

그럼 저기 `StringLatin1`과 `StringUTF16`의 결과는 무엇일까?

```
public static int hashCode(byte[] value) {
    int h = 0;
    int length = value.length >> 1;
    for (int i = 0; i < length; i++) {
        h = 31 * h + getChar(value, i);
    }
    return h;
}
```

재정의된 결과를 보면 결국 String 내부의 char 원소의 주소값을 각각 해시값으로 만든 것을 반환한다.  
따라서 동일한 문자열의 해시코드는 동일하다는 결론이 나오게 된다.  
즉 해시코드는 **무조건 주소값을 반환하는 것이 아니라, 클래스의 ‘고유한값’을 나타내는 것이다.**

이번엔 String의 equals 메소드를 살펴보자

String의 equals

```
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String aString = (String)anObject;
        if (coder() == aString.coder()) {
            return isLatin1() ? StringLatin1.equals(value, aString.value)
                                : StringUTF16.equals(value, aString.value);
        }
    }
    return false;
}
```

우선 주소값을 먼저 비교하고 또 다른 로직이 추가되어 있다.  
hashcode 메소드와 똑같은 방식으로 추가적인 동등성 검사를 진행한다.  
결국 hashcode와 equals 메소드 둘다 같은 방식으로 동등성을 비교한다.

그럼 컬렉션의 경우에는 어떨까?

  

## [[Collections]]
컬렉션 대표로 ArrayList의 동작을 살펴보자

```
ArrayList<Integer> al1 = new ArrayList<>();
al1.add(1);
ArrayList<Integer> al2 = new ArrayList<>();
al2.add(1);

System.out.println(al1.equals(al2)); // -> true
```

같은 요소를 넣고 equals()를 때려보니 true가 나온다.  
음… 여기서도 hashcode()가 주소값을 나타내는 것이 아님을 유추할 수 있다.

그럼 ArrayList의 hashcode()는 어떻게 동작할까…?

```
Returns the hash code value for this list. 
The hash code of a list is defined to be the result of the following calculation:
 
     int hashCode = 1;
     for (E e : list)
         hashCode = 31*hashCode + (e==null ? 0 : e.hashCode());
 
This ensures that list1.equals(list2) implies that list1.hashCode()==list2.hashCode() for any two lists, list1 and list2, as required by the general contract of Object.hashCode.
```

[[List|ArrayList]]의 hashcode()에 달려있는 자바독에서는 다음과 같이 설명한다.  
`List 내부 요소의 주소값을 해시코드값으로 만든 것의 합`이 해당 객체의 해시코드값이 되는것이다.

즉, String의 hashcode()와 비슷하게 동작하는 것을 알 수 있다.  
따라서 Wrapper 클래스는 불변객체이므로, 컬렉션 타입이 Wrapper 클래스일 경우 동일하게 생긴 컬렉션은 동일한 해시코드를 반환하는 것이다.  
(만약 컬렉션 타입이 일반적인 참조 타입이라면 당연히 이렇게 사용하진 못한다)

  

## 배열

컬렉션까지 알아보고 결론을 내려고 했으나, 배열에서의 동작이 궁금해졌다.

```
int[] a = {1};
int[] b = {1};

System.out.println(a.equals(b)); // -> false
```

String이나 컬렉션과는 달리 배열에서는 동일한 배열을 비교하면 false가 나온다  
따라서 순수 배열에서의 hashcode()는 주소값을 해싱한다는 것을 알 수 있는데, `Arrays` 컬렉션을 사용하면 또 다른 결과가 나오게 된다.

```
int[] a = {1};
int[] b = {1};

System.out.println(Arrays.equals(a, b); // -> true
```

이번에는 true가 나온다.  
왜 그런지 또 hashcode()를 까보자.. 

```
public static int hashCode(int a[]) {
    if (a == null)
        return 0;

    int result = 1;

    for (int element : a)
        result = 31 * result + element;
    return result;
}
```

보아하니 [[String]]과 컬렉션에서의 hashcode()와 매우매우 비슷하다.  
내부 원소를 여러개 가지는 자료구조의 경우, 인스턴스 주소값이 아닌 내부 원소값을 기준으로 해시코드를 생성하는 특징을 가지는 것 같다.

물론 equals 메소드도 이와 비슷하게 동등성 검사를 하도록 구현되어 있다.  
즉, 두 배열의 원소값이 같은지 비교하고 싶을땐 `Arrays.equals()` 메소드를 사용하면 된다.  
(물론 원시 타입 배열만 해당)






---
## 참조
https://dreamchaser3.tistory.com/7
https://hanjo8813.github.io/java/6/

https://blog.naver.com/PostView.nhn?blogId=travelmaps&logNo=220930144030
https://blog.naver.com/travelmaps/220931531769

https://olivejua-develop.tistory.com/**65**