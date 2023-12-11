---
{}
---
**스택(stack)은 제한적으로 접근할 수 있는 나열 구조이다. 사전적 정의는 '쌓다' 상자에 물건을 쌓아 올리듯이 데이터를 쌓는 구조로 그 접근 방법은 언제나 목록의 끝에서만 일어난다.**

자료를 넣는것을 '밀어넣는다' 하여 push라고 하고 반대로 넣어둔 자료를 꺼내는 것을 pop이라고 하는데, 이때 꺼내지는 자료는 가장 최근에 푸쉬한 자료부터 나오게 된다. **즉, 나중에 들어간 것이 먼저 나오는 LIFO(Last In First Out)의 형태를 띄고있다.**


### **스택의 특징**

1. 후입선출 (LIFO : Last In First Out) 구조 : 먼저 들어온 데이터가 나중에 빠져나가는 구조
2. 단방향 입출력 구조 : 데이터의 들어오는 방향과 나가는 방향이 같다.
3. 데이터를 하나씩만 넣고 뺄 수 있다.
4. 깊이 우선 탐색(DFS)에 이용된다.
5. 재귀 함수의 동작 흐름과 같은 구조를 가진다.


![[Pasted image 20231211101644.png]]

| **메서드**             | **설명**                                                                                                                                           |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **stack.push(1);**     | 스택에 값 1 추가      성공시 return data                                                                                                                             |
| **stack.size();**      | 스택의 크기 출력                                                                                                                                   |
| **stack.empty();**     | 스택이 비어 있으면 true, 비어 있지 않으면 false를 반환                                                                                             |
| **stack.peek();**      | 스택의 제일 상단에 있는(마지막으로 저장된) 요소를 반환   성공시 return Data 만약 Stack이 비어있을 경우 throw EmptyStackException                                                                                          |
| **stack.pop();**       | 스택의 제일 상단에 있는(마지막으로 저장된) 요소를 반환후,  <br>**해당 요소를 스택에서 제거함. (값을 remove 할때 pop 을 사용하면된다.)**            |
| **stack.search(1);**   | 스택에서 전달된 객체가 존재하는 위치의 인덱스를 반환  <br>이때 인덱스는 제일 상단에 있는(마지막으로 저장된) 요소의 위치부터 0이 아닌 1부터 시작함. 값이 없으면 -1 반환|
| **stack.contains(1);** | 스택에 1이 있으면 true, 없으면 false 를 반환                                                                                                       |



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









참고 - https://yunamom.tistory.com/232