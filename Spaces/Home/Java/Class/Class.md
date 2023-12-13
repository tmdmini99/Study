---
_links:
---


레퍼런스변수란??



클래스를 사용하기 위해서는 반드시 메모리에 생성을 해주어야 합니다.
이렇게 메모리 상에서 생성된 클래스를 클래스객체 혹은 인스턴스라 합니다.
레퍼런스 변수는 메모리상에 생성된 인스턴스를 가리키는데 사용되는 변수입니다.
모든 인스턴스는 레퍼런스 변수만을 통해서 사용이 가능한데요
레퍼런스 변수는 일반적인 데이터를 넣어두는 변수가 아니라서 인스턴스를 가리키는 값이 없습니다.
레페런스 변수는 인스턴스의 맴버변수와 메서드를 가리킬 수 있도록 되어 있지요.
인스턴스의 맴버변수나 메서드를 사용하는 방식은 다음과 같이 . ← 점을 이용합니다.
레퍼런스변수.맴버변수
레퍼런스변수.메서드()
즉, <span style="background:#fff88f">레퍼런스 변수는 참조형 변수이며, 실제 값이 아닌 주소 값을 가리키는 변수</span>라는 것.

```java
MakeStack<Integer> ms = new MakeStack<Integer>();
```
   =를 기준으로 앞에는 레퍼런스 변수 뒤에 n

```java
MakeStack<Integer> ms = new MakeStack<Integer>();
System.out.println("dfs"+ms);
```
위 코드 작성시 주소값이 아닌 배열 값이 나오게 하고 싶을때
클래스에서 
```java
public String toString(){  
    return ar.toString();  
}
```
이런식으로 return값을 바꿔주면 된다





참조 - https://jaimemin.tistory.com/1545

https://jjunii486.tistory.com/53 레퍼런스 변수
https://kadosholy.tistory.com/89 클래스및 객체 사용