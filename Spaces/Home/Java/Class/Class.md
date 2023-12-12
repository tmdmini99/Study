---
_links:
---




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




https://jaimemin.tistory.com/1545