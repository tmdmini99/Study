---
{}
---
# Stack이란? 후입선출
**스택(stack)은 제한적으로 접근할 수 있는 나열 구조이다. 사전적 정의는 '쌓다' 상자에 물건을 쌓아 올리듯이 데이터를 쌓는 구조로 그 접근 방법은 언제나 목록의 끝에서만 일어난다.**

자료를 넣는것을 '밀어넣는다' 하여 push라고 하고 반대로 넣어둔 자료를 꺼내는 것을 pop이라고 하는데, 이때 꺼내지는 자료는 가장 최근에 푸쉬한 자료부터 나오게 된다. **즉, 나중에 들어간 것이 먼저 나오는 LIFO(Last In First Out)의 형태를 띄고있다.**

주로 사용하는 곳
1. 웹 브라우저 뒤로가기
2. 실행 취소


## **스택의 특징**

1. 후입선출 (LIFO : Last In First Out) 구조 : 먼저 들어온 데이터가 나중에 빠져나가는 구조
2. 단방향 입출력 구조 : 데이터의 들어오는 방향과 나가는 방향이 같다.
3. 데이터를 하나씩만 넣고 뺄 수 있다.
4. 깊이 우선 탐색(DFS)에 이용된다.
5. 재귀 함수의 동작 흐름과 같은 구조를 가진다.


![[Stack1.png]]

| **메서드**             | **설명**                                                                                                                                                                            |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **stack.push(1);**     | 스택에 값 1 추가      성공시 return data                                                                                                                                            |
| **stack.size();**      | 스택의 크기 출력                                                                                                                                                                    |
| **stack.empty();**     | 스택이 비어 있으면 true, 비어 있지 않으면 false를 반환 Stack 클래스에만 정의                                                                                                        |
| **stack.peek();**      | 스택의 제일 상단에 있는(마지막으로 저장된) 요소를 반환   성공시 return Data 만약 Stack이 비어있을 경우 throw EmptyStackException                                                    |
| **stack.pop();**       | 스택의 제일 상단에 있는(마지막으로 저장된) 요소를 반환후,  <br>**해당 요소를 스택에서 제거함. (값을 remove 할때 pop 을 사용하면된다.)**     비어있을 경우 throw EmptyStackException |
| **stack.search(1);**   | 스택에서 전달된 객체가 존재하는 위치의 인덱스를 반환  <br>이때 인덱스는 제일 상단에 있는(마지막으로 저장된) 요소의 위치부터 0이 아닌 1부터 시작함. 값이 없으면 -1 반환              |
| **stack.contains(1);** | 스택에 1이 있으면 true, 없으면 false 를 반환                                                                                                                                        |
| **stack.add(1)**       | 스택에 값 1 추가 push와 다르게 add는 true/false를 반환                                                                                                                              |
| **stack.clear()**      | 스택안에 모든 값 제거                                                                                                                                                               |
| **stack.isEmpty()**        | 스택이 비어있으면 true, 비어있지 않으면 false를 반환 Collection인터페이스에 정의                                                                                                                                                                                    |





```java
import java.util.Stack; 
Stack<Integer>stack = new Stack<Integer>(); // Integer타입 선언 
Stack<Integer>stack = new Stack<>(); // 뒤의 타입 생략 가능 
Stack<Character>stack = new Stack<>(); // Char 타입 선언 
Stack<String>stack = new Stack<>(); // String 타입 선언
```

```java
import java.util.Stack; 

public class Solution { 
	public static void main (String[] args) { 
		Stack<Integer> stack = new Stack<>();
		// 값넣기
		stack.push(1);
		stack.push(2);
		stack.push(3);
		stack.push(4);
		
		// 스택 크기 출력
		System.out.println(stack.size()); // 4
		
		// 비어 있으면 true, 비어 있지 않으면 false 
		System.out.println(stack.empty()); // false
		
		// 마지막으로 저장된 요소 반환 // stack에 변화를 주지않음
		System.out.println(stack.peek()); // 4 
		System.out.println(stack); // [1, 2, 3, 4]
		// 마지막으로 저장된 요소 반환후 해당요소 제거 
		System.out.println(stack.pop()); // 4 
		System.out.println(stack); // [1, 2, 3]
		
		/* 스택에서 전달된 값이 존재하는 위치의 인덱스 반환 
		(마지막으로 저장된 요소의 위치에서 0이 아닌 1부터 시작) 값이 없으면 -1 반환 */ 
		System.out.println(stack.search(4)); // -1 
		System.out.println(stack.search(3)); // 1 
		System.out.println(stack.search(2)); // 2
		
		// 스택에 전달된 값이 있으면 true, 없으면 false 
		System.out.println(stack.contains(4)); // false 
		System.out.println(stack.contains(3)); // true
	}
}
```



```java
import java.io.BufferedReader; 
import java.io.InputStreamReader; 
import java.util.Stack; 
import java.io.IOException; 
public class Main{ 
	public static void main(String[] args) throws IOException{ 
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); 
	StringBuilder sb = new StringBuilder(); 
	int T = Integer.parseInt(br.readLine()); 
	for(int i = 0; i < T; i++){ 
		Stack<String> stack = new Stack<>(); 
		String temp = br.readLine(); 
		for(char c : temp.toCharArray()){ 
			if(!stack.isEmpty() && stack.peek().equals("(") && Character.toString(c).equals(")")){ 
				stack.pop(); 
			} else { 
				stack.add(Character.toString(c)); 
				} 
		} 
		if(stack.isEmpty()){ 
			sb.append("YES" +"\n"); 
		} else{
			sb.append("NO" + "\n"); 
		} 
		} 
		System.out.println(sb); 
		} 
	}
```


처음 stack에 값이 없다.

(()())((()))
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12)  
(()())((()))
true and 마지막이 "(" 이고 가져온게 ")"일때
1번째
마지막이 ) 이ㅏ기때문에 false
stack에 추가
(
추가 
stack.size(1);
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12) 
2번째 
지금 2번째 char가 (이기 때문에 false
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 ) 
((
stack.size(2);
3번째
마지막이 ( 이고 지금 위치가 )이기때문에 true
pop()으로 마지막 지움
(
stack.size(1);
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 )
4번째 지금 (이기때문에 false
((
stack.size(2);
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 ) 
5번째
지금)이고 마지막이 (이기때문에  true
(
stack.size(1);
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 ) 
6번째 )이고 마지막이 (이기때문에 true
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 )  
stack.size(0);
7번째
1( 2(   3)   4(   5)   6)   7(   8(   9(  10)  11)  12 )
false
(
stack.size(1);
8번째
((
9번째
(((
10번째
지금 )이니깐 stack에서 하나 지움
11번째
)로 stack에서 하나 지움
12번째 )로 stack에서 하나 지움


## Stack 직접 구현

```java
package Week1;  
  
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.EmptyStackException;  
import java.util.Vector;  
  
public class MakeStack<E> extends Vector<E> {  
     MakeArrayList<E> ar;  
     int size;  
    public MakeStack(){  
        this.ar=new MakeArrayList<E>();  
  
    }  
    public String toString(){  
        return String.valueOf(ar);  
    }  
    //push  
    public E push(E e){  
        ar.add(e);  
        return  e;  
    }  
    //pop  
    public E pop(){  
        try{  
            return ar.remove(ar.size()-1);  
        }catch(EmptyStackException e){  
            System.out.println("배열 안에 값이 없음");  
            return null;  
        }  
    }  
    public int size(){  
        return ar.size();  
    }  
    public boolean add(E e){  
        return ar.add(e);  
    }  
    public boolean empty(){  
        return ar.isEmpty();  
    }  
//    public void clear(){  
//        ar.clear();  
//    }  
    public E peek(){  
        return  ar.get(ar.size()-1);  
    }  
}
```









참고 - https://yunamom.tistory.com/232



https://velog.io/@1_dohyeon/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-3-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A1%9C-%EC%8A%A4%ED%83%9Dstack-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-with-java


https://hbase.tistory.com/122 