# <font color="#ff0000">1. List 인터페이스</font>


순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.

  

# **LinkedList**  
    - 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용  
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임  
    - LinkedList란 [[Collection]] 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다
    - LinkedList란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다



| 메서드                                   | 설명                                                                   |
|:---------------------------------------- |:---------------------------------------------------------------------- |
| boolean add(object)                      | LinkedList의 마지막에 데이터 추가                                      |
| void add(index, object)                  | LinkedList의 index위치에 데이터 추가                                   |
| void addFirst(object)                    | 가장 앞에 데이터 추가                                                  |
| void addLast(object)                     | 가장 뒤에 데이터 추가                                                  |
| void set(int Index, Object)              | index위치에 데이터 변경                                                |
| Object remove(int index)                 | 첫번째 데이터 삭제                                                     |
| Obejct removeFirst()                     | 첫번째 데이터 삭제                                                     |
| Object removeLast()                      | 마지막 데이터 삭제                                                     |
| void clear()                             | 모든 데이터 삭제                                                       |
| boolean removeAll(Collection c)          | 지정한 컬렉션에 저장된 것과 동일한 노드들을 LinkedList에서 제거한다    |
| boolean remove(Object obj)               | 지정된 객체를 제거한다. (성공하면 true)                                |
| int size()                               | LinkedList에 저장된 객체의 개수를 반환한다.                            |
| boolean isEmpty()                        | LinkedList가 비어있는지 확인한다.                                      |
| boolean contains(Object obj)             | 지정된 객체(obj)가 LinkedList에 포함되어 있는지 확인한다.              |
| boolean containsAll(Collection c)        | 지정된 컬렉션의 모든 요소가 포함되었는지 알려줌.                       |
| int indexOf(Object obj)                  | 지정된 객체(obj)가 저장된 위치를 찾아 반환한다.                        |
| int lastIndexOf(Object obj)              | 지정된 객체(obj)가 저장된 위치를 뒤에서 부터 역방향으로 찾아 반환한다. |
| Object get(in index)                     | 지정된 위치(index)에 저장된 객체를 반환한다.                           |
| List subList(int fromIndex, int toIndex) | fromIndex부터 toIndex사이에 저장된 객체를 List로 반환한다.             |
| Object set(int index, Object obj)        | 지정한 위치(index)의 객체를 주어진 객체로 바꾼다.                      |
| Object[] toArray()                       | LinkedList에 저장된 모든 객체들을 객체배열로 반환한다.                 |
| Object[] toArray(Obejct[] objArr)        | LinkedList에 저장된 모든 객체들을 객체배열 objArr에 담아 반환한다.     |


#### **Linked**  **List 이터레이터**


| 메서드                               | 설명                                                          |
|:------------------------------------ |:------------------------------------------------------------- |
| Iterator iterator()                  | LinkedList의 Iterator객체를 반환한다                          |
| ListIterator listIterator()          | LinkedList의 ListIterator를 반환한다                          |
| ListIterator listIterator(int index) | LinkedList의 지정된 위치부터 시작하는 ListIterator를 반환한다 |


### **Linked****List 스택 & 큐 지원**


| **메서드** | **설명** |
| ---------- | -------- |
|            |          |
|Object element()| LinkedList에 첫 번째 노드를 반환  |
|boolean offer(Obejct obj)| 지정된 객체(obj)를 LinkedList의 끝에 추가.  성공하면 true 실패하면 false  |

| Object peek() |
LinkedList의 첫 번째 요소를 반환 |


|Object poll()| LinkedList의 첫 번째 요소를 반환  
LInkedList의 요소에서는 제거된다. |

| void push(Object obj) |
맨 앞에 객체(obj)를 추가 (addFirst와 동일) |

| Iterator descendingItorator() | 역순으로 조회하기 위한 DescendingItorator를 반환 |

| Object getFrist()|
LinkedList의 첫번째 노드를 반환 |

| Object getLast()|
LinkedList의 마지막 노드를 반환|

| boolean offerFirst(Object obj) |
지정된 객체(obj)를 LinkedList의 맨 앞에 추가, 성공하면 ture |

| boolean offerLast(Object obj) |
지정된 객체(obj)를 LinkedList의 맨 뒤에 추가, 성공하면 ture |

Object peakFirst() |
첫번째 노드를 반환 |

Object peakLast()
마지막 노드를 반환

Object pollFirst()
첫번째 노드를 반환하면서 제거

Object pollLast()
마지막 노드를 반환하면서 제거

Object pop()
첫번째 노드를 제거 (removeFirst와 동일)


boolean removeFirstOccurrence(  
Obejct obj)
첫번째로 일치하는 객체를 제거


boolean removeLastOccurrence(  
Obejct obj)
마지막으로 일치하는 객체를 제거

출처: [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0) [Inpa Dev 👨‍💻:티스토리]




이 클래스는 데이터가 연속된 위치에 저장되지 않고 모든 데이터가 데이터 부분과 주소 부분을 별도로 가지고 있습니다

데이터는 포인터와 주소를 사용하여 연결합니다

각 데이터는 노드라 불리며 배열에서 자주 삽입, 삭제가 이루어지는 경우 용이하여 [ArrayList](#ArrayList)보다 선호됩니다

하지만 [ArrayList](#ArrayList)보다 검색에 있어서는 느리다는 단점이 있습니다

	![[Pasted image 20231211120131.png]]
	
	

```java
LinkedList li = new LinkedList(); // 타입 설정x Object로 입력
LinkedList<LinkedListDemo> demo = new LinkedList<LinkedListDemo>(); // List타입을 LinkedListDemo
LinkedList<Integer> i = new LinkedList<Integer>(); // int 타입으로 선언
LinkedList<Integer> i2 = new LinkedList<>(); // 타입 선언 생략도 가능
LinkedList<Integer> i3 = new LinkedList<Integer>(Arrays.asList(1, 2, 3)); // 초기 값 세팅
		
LinkedList<String> s = new LinkedList<String>(); // String 타입 사용
LinkedList<Character> ch = new LinkedList<Character>(); // Char 타입 사용

```




### **LinkedList 값 추가하기**
```java
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		ll.add("Hello");
		ll.add("Hello");
		ll.add(1, "World");
		ll.addFirst("11");
		ll.addLast("22");
		
		System.out.print(ll);
	}
}


```

LinkedList의 값을 추가하기 위해서는 add() 메서드를 사용합니다

add() 메서드의 사용방법은 두 가지가 있습니다

add(Object) : 기본적으로 add를 사용하여 추가하면 LinkedList의 마지막에 데이터를 추가합니다

add(int Index, Object) : LinkedList의 Index에 데이터를 추가합니다

addFirst(Object) : 가장 앞에 데이터 추가

addLast(Object): 가장 뒤에 데이터 추가

위의 코드를 실행하면 ll.add(1, "World");를 하여 "Hello"와 "Hello" 사이에 "World"가 추가된 것을 확인할 수 있습니다

### **LinkedList 값 변경하기**
```java
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		ll.add("Hello");
		ll.add("Hello");
		ll.add(1, "World"); //index 1에 데이터 10 추가가
		
		System.out.println(ll);
		
		ll.set(1, "Hello");

		System.out.println(ll);
	}
}

```

LinkedList의 값을 변경하는 방법은 set() 메서드를 사용합니다

값을 바꾸는 것이기 때문에 값을 바꾸려면 Index를 알아야 변경이 가능합니다

set(int Index, Object)로 변경할 수 있습니다

위의 예제를 실행하면 "Hello", "World", "Hello"를 set(1, "Hello")를 사용하여

"Hello", "Hello", "Hello"로 변경합니다



### **LinkedList 값 삭제하기**
```java
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		System.out.println(ll);

		ll.removeFirst(); // 첫 번째 데이터 삭제
		ll.removeLast(); // 마지막 데이터 삭제

		System.out.println(ll);
		
		ll.remove(); // remove로 삭제하면 첫 번째 데이터를 삭제

		System.out.println(ll);

		ll.remove(2); // Index순번의 데이터를 삭제

		System.out.println(ll);

		ll.clear(); // List의 모든 데이터를 삭제
		
		System.out.println(ll);
	}
}
```

값을 제거하는 방법에는 여러 가지가 있습니다

removeFirst(), removeLast() : LinkedList에서 첫 번째와 마지막 데이터를 삭제합니다

remove() : 첫 번째 데이터를 삭제합니다

remove(int Index) : Index 위치의 데이터를 삭제

clear() : List의 모든 데이터를 삭제 -> removeAll(LinkedList)로도 모든 데이터 삭제가 가능합니다

size() : [LinkedList](#LinkedList)의 크기


### LinkedList 값 출력

```java
import java.util.Iterator;
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		/* get(i) 메서드를 사용하여 값 출력 */
		for(int i = 0; i < ll.size(); i++)
			System.out.print(ll.get(i) + " ");
		
		System.out.println();

		/* 향상된for문을 사용하여 값 출력 */
		for(String str : ll)
			System.out.print(str + " ");
		
		System.out.println();

		/* Iterator를 사용하여 값 출력 */
		Iterator iter = ll.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}

```
LinkedList의 <span style="background:#fff88f">get(index)</span> 메소드를 사용하면 LinkedList의 원하는 index의 값이 리턴됩니다. 
전체 출력은 대부분 for문을 통해서 출력을 하고 [[Iterator]]를 사용해서 출력을 할 수도 있습니다.
LinkedList의 경우 인덱스를 사용하여 연산을 수행할 수 있도록 get(index) 메소드를 제공하지만, 메소드 내부의 동작은 순차 탐색으로 이루어져 있어 ArrayList의 get(index)메서드보다 속도가 느립니다.


### LinkedList 값 검색

```java
  
LinkedList<Integer> list = new LinkedList<Integer>(Arrays.asList(1,2,3));

System.out.println(list.contains(1)); //list에 1이 있는지 검색 : true

System.out.println(list.indexOf(1)); //1이 있는 index반환 없으면 -1
```

LinkedList에서 찾고자 하는 값을 검색하려면 LinkedList의 contains(value) 메소드를 사용하면 됩니다. 
만약 값이 있다면 true가 리턴되고 값이 없다면 false가 리턴됩니다. 
값을 있는 index를 찾으려면 indexOf(value) 메소드를 사용하면 되고 만약 값이 없다면 -1을 리턴합니다.




---

# **<font color="#ff0000">Vector</font>**  

과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음  
Vector란 [[Collection]] 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다
ArrayList와 동일한 구조를 가지며 배열의 크기가 늘어나고, 줄어듬에 따라서 자동으로 크기가 조절이 됩니다
Vector의 특이한 점이라면 항상 동기화되어있고 Collection 프레임워크에 없는 메서드들을 사용이 가능합니다
하지만 동기화라는 특징이 있어 스레드가 아닌 환경에서는 거의 사용이 되지 않습니다
그리고 항상 동기화되므로 스레드 환경에서의 안정성은 높지만 [ArrayList](#ArrayList)와 비교하여 추가, 검색, 삭제의 성능은 떨어지는 단점이 있습니다


### **Vector 선언하기**
```java
Vector V = new Vector(); // 타입 설정x Object로 사용 
Vector<VectorDemo> demo = new Vector<VectorDemo>(); // 타입설정 VectorDemo 객체로 선언 
Vector<Integer> i = new Vector<Integer>(); // int 타입으로 선언 
Vector<Integer> i2 = new Vector<>(); // 타입 선언 생략
Vector<Integer> i3 = new Vector<Integer>(10); // 초기 용량 세팅 
Vector<Integer> i4 = new Vector<Integer>(Arrays.asList(1, 2, 3, 4)); // 초기 값 세팅 
		
Vector<String> s = new Vector<String>(); // String 타입 사용 
Vector<Character> ch = new Vector<Character>(); // char 타입 사용

출처: [https://crazykim2.tistory.com/570](https://crazykim2.tistory.com/570) [차근차근 개발일기+일상:티스토리]
```

Vector의 선언방법입니다

위의 예제와 같이 Class, Integer, String, Character 등의 다양한 타입으로 선언이 가능합니다



### **Vector 값 추가하기**
```java
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("Hello");
		V.add(1, "World");
		V.add(null);
		
		System.out.print(V);
	}
}
```

Vector의 값을 추가하기 위해서는 add() 메서드를 사용합니다

add() 메서드의 사용방법은 두 가지가 있습니다

add(Object) : 기본적으로 add를 사용하여 추가하면 Vector의 마지막에 데이터를 추가합니다

add(int Index, Object) : Vextor의 Index위치에 데이터를 추가합니다

참고사항으로 Vector는 null을 허용하여 null값도 추가할 수 있습니다

위의 코드를 실행하면 V.add(1, "World");를 하여 "Hello"와 "Hello" 사이에 "World"가 추가된 것을 확인할 수 있습니다


### **Vector 값 변경하기**
```java
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("Hello");
		V.add(1, "World");
		
		System.out.println(V);
		
		V.set(1, "Hello");

		System.out.println(V);
	}
}
```

Vector의 값을 변경하는 방법은 set() 메서드를 사용합니다

값을 바꾸려면 조건이 필요한데 Index를 알아야 원하는 값을 변경이 가능합니다

set(int Index, Object)로 변경할 수 있습니다

위의 예제를 실행하면 "Hello", "World", "Hello"를 set(1, "Hello")를 사용하여

"Hello", "Hello", "Hello"로 변경합니다



### **Vector 값 제거하기**
```java
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("World");
		V.add("Hello");
		V.add("World");
		
		System.out.println(V);
		
		V.remove(1); // Index 1의 값 제거

		System.out.println(V);

		V.removeAllElements(); // 모든 데이터 제거 

		System.out.println(V);

		V.add("Hello");
		V.add("World");
		V.clear(); // 모든 데이터 제거

		System.out.println(V);
	}
}
```



Vector의 값을 삭제하는 방법입니다

원하는 값을 삭제하는 방법은 remove(int Index)를 사용하여 삭제합니다

값을 한꺼번에 삭제하려면 removeAllElements(), clear() 메서드를 사용하여 삭제할 수 있습니다



---

# **ArrayList**  
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남  










※제네릭스는 선언할 수 있는 타입이 객체 타입입니다. int는 기본자료형이기 때문에 들어갈 수 없으므로 int를 객체화시킨 wrapper클래스를 사용해야 합니다


참고- https://www.nextree.co.kr/p6506/  : Linked vs Array 차이
https://dev-coco.tistory.com/19 간단명료 이해하기 쉬움
https://crazykim2.tistory.com/566
https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0

vector  https://crazykim2.tistory.com/570

