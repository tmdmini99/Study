List 인터페이스
순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.


# **LinkedList**  
    - 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용  
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임  
    - LinkedList란 [[Collection]] 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다
    - LinkedList란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다

## LinkedList  메서드

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


### **Linked** **List 스택 & 큐 지원**


| **메서드** | **설명** |
| ---------- | -------- |
|Object element()| LinkedList에 첫 번째 노드를 반환  |
|boolean offer(Obejct obj)| 지정된 객체(obj)를 LinkedList의 끝에 추가.  성공하면 true 실패하면 false  |
| Object peek() | LinkedList의 첫 번째 요소를 반환 |
|Object poll()| LinkedList의 첫 번째 요소를 반환  LInkedList의 요소에서는 제거된다. |
| void push(Object obj) | 맨 앞에 객체(obj)를 추가 (addFirst와 동일) |
| Iterator descendingItorator() | 역순으로 조회하기 위한 DescendingItorator를 반환 |
| Object getFrist()                         | LinkedList의 첫번째 노드를 반환                             |     | Object getLast() | LinkedList의 마지막 노드를 반환 |
| boolean offerFirst(Object obj)            | 지정된 객체(obj)를 LinkedList의 맨 앞에 추가, 성공하면 ture |     |                  |                                 |
| boolean offerLast(Object obj)             | 지정된 객체(obj)를 LinkedList의 맨 뒤에 추가, 성공하면 ture |     |                  |                                 |
| Object peakFirst()                        | 첫번째 노드를 반환                                          |     |                  |                                 |
| Object peakLast()                         | 마지막 노드를 반환                                          |     |                  |                                 |
| Object pollFirst()                        | 첫번째 노드를 반환하면서 제거                               |     |                  |                                 |
| Object pollLast()                         | 마지막 노드를 반환하면서 제거                               |     |                  |                                 |
| Object pop()                              | 첫번째 노드를 제거 (removeFirst와 동일)                     |     |                  |                                 |
| boolean removeFirstOccurrence(Obejct obj) | 첫번째로 일치하는 객체를 제거                               |     |                  |                                 |
| boolean removeLastOccurrence(Obejct obj)  | 마지막으로 일치하는 객체를 제거                             |     |                  |                                 |


### **LinkedList 동기화 처리**
```java
/* ArrayList 동기화 처리 */
List<String> l1 = Collections.synchronizedList(new ArrayList<>());
/* LinkedList 동기화 처리 */
List<String> l2 = Collections.synchronizedList(new LinkedList<>());
```


이 클래스는 데이터가 연속된 위치에 저장되지 않고 모든 데이터가 데이터 부분과 주소 부분을 별도로 가지고 있습니다

데이터는 포인터와 주소를 사용하여 연결합니다

각 데이터는 노드라 불리며 배열에서 자주 삽입, 삭제가 이루어지는 경우 용이하여 [ArrayList](#ArrayList)보다 선호됩니다

하지만 [ArrayList](#ArrayList)보다 검색에 있어서는 느리다는 단점이 있습니다

	![[Pasted image 20231211120131.png]]
	




![[Pasted image 20231214154739.png]]



```java
LinkedList li = new LinkedList(); // 타입 설정x Object로 입력
LinkedList<LinkedListDemo> demo = new LinkedList<LinkedListDemo>(); // List타입을 LinkedListDemo
LinkedList<Integer> i = new LinkedList<Integer>(); // int 타입으로 선언
LinkedList<Integer> i2 = new LinkedList<>(); // 타입 선언 생략도 가능
LinkedList<Integer> i3 = new LinkedList<Integer>(Arrays.asList(1, 2, 3)); // 초기 값 세팅
		
LinkedList<String> s = new LinkedList<String>(); // String 타입 사용
LinkedList<Character> ch = new LinkedList<Character>(); // Char 타입 사용

```

## **LinkedList 값 추가하기**
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

## **LinkedList 값 변경하기**
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



## **LinkedList 값 삭제하기**
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


## LinkedList 값 출력

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
전체 출력은 대부분 for문을 통해서 출력을 하고 [[Spaces/Home/Java/Iterator/Iterator]]를 사용해서 출력을 할 수도 있습니다.
LinkedList의 경우 인덱스를 사용하여 연산을 수행할 수 있도록 get(index) 메소드를 제공하지만, 메소드 내부의 동작은 순차 탐색으로 이루어져 있어 ArrayList의 get(index)메서드보다 속도가 느립니다.


## LinkedList 값 검색

```java
  
LinkedList<Integer> list = new LinkedList<Integer>(Arrays.asList(1,2,3));

System.out.println(list.contains(1)); //list에 1이 있는지 검색 : true

System.out.println(list.indexOf(1)); //1이 있는 index반환 없으면 -1
```

LinkedList에서 찾고자 하는 값을 검색하려면 LinkedList의 contains(value) 메소드를 사용하면 됩니다. 
만약 값이 있다면 true가 리턴되고 값이 없다면 false가 리턴됩니다. 
값을 있는 index를 찾으려면 indexOf(value) 메소드를 사용하면 되고 만약 값이 없다면 -1을 리턴합니다.




---

---
# **<font color="#ff0000">Vector</font>**  

과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음  
Vector란 [[Collection]] 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다
ArrayList와 동일한 구조를 가지며 배열의 크기가 늘어나고, 줄어듬에 따라서 자동으로 크기가 조절이 됩니다
Vector의 특이한 점이라면 항상 동기화되어있고 Collection 프레임워크에 없는 메서드들을 사용이 가능합니다
하지만 동기화라는 특징이 있어 스레드가 아닌 환경에서는 거의 사용이 되지 않습니다
그리고 항상 동기화되므로 스레드 환경에서의 안정성은 높지만 [ArrayList](#ArrayList)와 비교하여 추가, 검색, 삭제의 성능은 떨어지는 단점이 있습니다



## Vector 메서드


| 메서드                             | 설명                                       |     |                                    |                                      |
| ---------------------------------- | ------------------------------------------ | 
| boolean add() | 마지막에 데이터를 추가       |     
| void add(int index, Object object) | 지정한 인덱스의 위치에 객체를 추가함 |
| void addElement(Object objec)      | 벡터의 끝에 객체를 추가한다                |     |                                    |                                      |
| Object remove(int index)           | 지정한 위치의 객제를 벡터에서 제거         |     |                                    |                                      |
| boolean remove(Object object)      | 지정한 객체를 벡터에서 제거                |     |                                    |                                      |
| void clear()                       | 벡터의 모든 요소를 제거                    |     |                                    |                                      |
| Object elementAt(int index)        | 지정한 위치의 객체를 리턴                  |     |                                    |                                      |
| Object get(int index)              | 지정한 위치의 객체를 리턴0                 |     |                                    |                                      |
| int capcity()                      | 벡터의 현재 크기 리턴                      |     |                                    |                                      |
| boolean contains(Object object)    | 주어진 요소가 벡터에 있는지 알아낸다.      |     |                                    |                                      |
| int indexof(Object object)         | 주어진 요소의 위치를 리턴(없으면 -1)       |     |                                    |                                      |
| int size()                         | 벡터에 포함되어 갯수를 리턴                |     |                                    |                                      |
| void trimToSize()                  | 벡터의 용량을 현재 벡터의 크기에 맞게 수정 |     |                                    |                                      |



## **Vector 선언하기**
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



## **Vector 값 추가하기**
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


## **Vector 값 변경하기**
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


## **Vector 값 제거하기**
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

---
# **ArrayList**  
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남  
ArrayList란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다
ArrayList는 자바에서 기본적으로 많이 사용되는 클래스입니다.

![](https://blog.kakaocdn.net/dn/b10vWe/btq49R6wfJE/lnqP0STxU0wtvnSpXVC0U0/img.png)

표준 배열보다는 느리지만 배열에서 많은 조작이 필요한 경우 유용하게 사용할 수 있습니다
List 인터페이스에서 상속받아 사용이 됩니다
ArrayList는 객체가 추가되어 용량을 초과하면 자동으로 부족한 크기만큼 용량이 늘어납니다
ArrayList는 자바의 List 인터페이스를 상속받은 여러 클래스 중 하나입니다.
일반 배열과 동일하게 연속된 메모리 공간을 사용하며 인덱스는 0부터 시작합니다.


| 메서드                      | 설명                                                                                                                                            |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **.add((index), val)**      | 순서대로 리스트를 추가, 배열 사이즈 초과 시 초기 설정된 사이즈만큼 자동으로 사이즈가 증가함, 인덱스를 추가로 지정해주면 해당 인덱스에 값을 삽입 |
| **.get(index)**             | 해당 인덱스의 값 반환                                                                                                                           |
| **.set(index, val)**        | 인덱스로 값 변경                                                                                                                                |
| **.indexOf(val)**           | 값을 제공하면 해당 값의 첫번째 인덱스를 반환                                                                                                    |
| **.lastindexOf(val)**       | 해당 값의 마지막 인덱스 반환                                                                                                                    |
| **.remove(index or val)**   | 해당 인덱스의 값 or 해당 값 중 첫번째 값 삭제                                                                                                   |
| **.contains(val)**          | 해당 값이 배열에 있는지 검색해서 true / false 반환                                                                                              |
| .containsAll(val1, val2...) | argument로 제공한 컬렉션의 모든 값이 포함되어 있는지 여부를 true / false로 반환                                                                 |
| .toArray()                  | ArrayList 타입의 인스턴스를 일반 배열 타입으로 반환, 저장할 배열 타입에 맞춰 자동 형변환, 배열 크기 또한 자동으로 맞춰서 바꿔줌                 |
| **.clear()**                | 값 모두 삭제                                                                                                                                    |
| **.isEmpty()**              | 비었으면 true, 하나라도 값이 있으면 false 반환                                                                                                  |
| .addAll(arr2)               | 두 컬렉션을 합침                                                                                                                                |
| .retainAll(arr2)            | argument로 제공한 컬렉션 내에 들어있는 값을 제외하고 모두 지워줌                                                                                |
| **.removeAll(arr2)**        | argument로 제공한 컬렉션 내에 들어있는 값과 일치하는 값을 모두 지워줌, retainAll() 메소드와 반대                                                |
| **.size()**                 | 요소 개수 반환                                                                                                                                  |
| .forEach()                  | for문 대신 바로 사용 가능                                                                                                                                                 |

| 메서드                         | 설명                                                 |
| ------------------------------ | ---------------------------------------------------- |
| boolean [add](#add)(T value)           | 요소를 추가                                          |
| void add(int index, T value)   | 요소를 특정 위치에 추가                              |
| boolean [remove](#remove)(Object value)   | 요소를 삭제                                          |
| T remove(int index)            | 특정 위치에 있는 요소를 삭제                         |
| T get(int index)               | 요소 가져오기                                        |
| void [set](#set)(int index, T value)   | 특정 위치에 있는 요소를 새 요소로 대체               |
| boolean contains(Object value) | 특정 요소가 리스트에 있는지 여부를 확인              |
| int indexOf(Object value)      | 특정 요소가 몇 번째 위치에 있는지를 반환 (순차 검색) |
| int lastIndexOf(Object o)      | 특정 요소가 몇 번째 위치에 있는지를 반환 (역순 검색) |
| int size()                     | 요소의 개수를 반환                                   |
| boolean isEmpty()              | 요소가 비어있는지                                    |
| public void clear()            | 요소를 모두 삭제                                     |

```java
fruits.forEach(item -> System.out.println("item : " + item));
```
람다 표현식


## ArrayList 선언하기

```java
ArrayList list = new ArrayList(); // 타입 설정x Object로 사용
ArrayList<ArrayListDemo> demo = new ArrayList<ArrayListDemo>(); // 타입설정 ArrayListDemo 객체로 선언
ArrayList<Integer> i = new ArrayList<Integer>(); // int 타입으로 선언
ArrayList<Integer> i2 = new ArrayList<>(); // Integer 타입 사용
ArrayList<Integer> i3 = new ArrayList<Integer>(10); // 초기 용량 세팅
ArrayList<Integer> i4 = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4)); // 초기 값 세팅
		
ArrayList<String> s = new ArrayList<String>(); // String 타입 사용
ArrayList<Character> ch = new ArrayList<Character>(); // char 타입 사용
```


ArrayList의 선언방법입니다

주로 Integer타입으로 선언을 많이하고, 추가로 다른 타입(String, Character) 등의 타입으로 선언이 가능합니다

타입을 선언하면 해당 타입의 데이터만 추가가 가능합니다

ArrayList를 선언하면서 초기용량 및 초기값을 세팅할 수 있는데 위의 예제를 참고바랍니다

## **ArrayList 값 추가하기**

###### add


ArrayList의 값을 추가하기 위해서는 add() 메서드를 사용합니다

add()의 사용법에는 두 가지가 있습니다

**add(Object)** : ArrayList의 마지막에 데이터를 추가합니다

**add(int index, Object)** : ArrayList의 index에 데이터를 추가합니다


```java
import java.util.ArrayList;

public class ArrayListDemo {
	public static void main(String[] args)  {
		ArrayList<String> al = new ArrayList<>();
		
		al.add("Hello");
		al.add("Hello");
		al.add(1, "World");
		
		System.out.print(al);
	}
}
```

## **ArrayList 값 변경하기**

ArrayList 값 변경은 set() 메서드를 사용합니다

set()을 사용하기 위해서는 바꾸려면 데이터의 위치Index를 알아야 변경이 가능합니다

###### set
 **set(int index, Object)**를 사용합니다

```java
import java.util.ArrayList;

public class ArrayListDemo {
	public static void main(String[] args)  {
		ArrayList<String> al = new ArrayList<>();
		
		al.add("Hello");
		al.add("Hello");
		al.add("Hello");
		
		System.out.println("초기값 : " + al);
		
		al.set(1, "World");

		System.out.println("변경된 값 : " + al);
	}
}
```


## **ArrayList 값 삭제하기**
###### remove

ArrayList 값을 삭제하는 방법에는 remove()와 clear()가 있습니다

clear()는 ArrayList의 모든 값을 삭제할 때 사용됩니다

remove()는 값을 하나씩 제거할 때 사용됩니다

remove()는 두 개의 사용법이 있는데

**remove(Object)** : Object를 파라미터로 넘기는 경우 해당 ArrayList의 Object와 같은 값을 삭제합니다

 만약 같은 값이 두 개인 경우 첫 번째 같은 값을 제거합니다

**remove(int index)** : ArrayList의 index에 해당하는 값을 삭제합니다
```java
import java.util.ArrayList;

public class ArrayListDemo {
	public static void main(String[] args)  {
		ArrayList<String> al = new ArrayList<>();
		
		al.add("Hello");
		al.add("World");
		al.add("Hello");
		al.add("World");
		
		System.out.println("초기값 : " + al);
		
		al.remove("Hello");

		System.out.println("1번 삭제 : " + al);

		al.remove(1);

		System.out.println("2번 삭제 : " + al);
		
		al.clear();

		System.out.println("3번 삭제 : " + al);
	}
}
```



## **ArrayList 값 Clone**

```java
import java.util.ArrayList;

 

public class CloneArrayList {

    public static void main(String[] args) {

 

        // 1. ArrayList 준비

        ArrayList<Fruit> fruitList = new ArrayList<Fruit>();

        fruitList.add(new Fruit("Apple", 1000));

        fruitList.add(new Fruit("Banana", 2000));

 

        // 2. ArrayList 복사 - clone()

        ArrayList<Fruit> copyOfFruitList = (ArrayList<Fruit>) fruitList.clone();

        

        // 3. 복사된 결과 출력

        System.out.println("=== ArrayList 복사(clone()) ===");

        System.out.println("fruitList : " + fruitList);  // [[ Apple: 1000 ], [ Banana: 2000 ]]

        System.out.println("copyOfFruitList : " + copyOfFruitList); // [[ Apple: 1000 ], [ Banana: 2000 ]]

 

        // 4. 원본 ArrayList 변경

        fruitList.add(new Fruits("Orange", 500));

 

        // 5. 결과 출력

        System.out.println("=== 원본 ArrayList 변경 ===");

        System.out.println("fruitList : " + fruitList);  // [[ Apple: 1000 ], [ Banana: 2000 ], [ Orange: 500 ]]

        System.out.println("copyOfFruitList : " + copyOfFruitList); // [[ Apple: 1000 ], [ Banana: 2000 ]]

    }

}

 

class Fruit {

 

    private String name;

    private int price;

 

    public Fruit(String name, int price) {

        this.name = name;

        this.price = price;

    }

 

    @Override

    public String toString() {

        return "[ " + this.name + ": " + this.price + " ]";

    }

}

출처: [https://hianna.tistory.com/567](https://hianna.tistory.com/567) [어제 오늘 내일:티스토리]
```

원본을 clone으로 복사후 원본 데이터를 추가, 삭제가 아니라 원본의 원소를 변경시 clone한 list도 같이 변경
Refernce타입만 가능 주소를 따라가기 때문에 clone도 같이 수정
clone 수정되지 않으려면 새롭게 만들어야함



※제네릭스는 선언할 수 있는 타입이 객체 타입입니다. int는 기본자료형이기 때문에 들어갈 수 없으므로 int를 객체화시킨 wrapper클래스를 사용해야 합니다

---
참고- https://www.nextree.co.kr/p6506/  : Linked vs Array 차이
https://dev-coco.tistory.com/19 간단명료 이해하기 쉬움
https://crazykim2.tistory.com/566
https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0

vector  https://crazykim2.tistory.com/570
https://blog.naver.com/manymoa/150004980532

ArrayList - https://crazykim2.tistory.com/558
https://junjangsee.github.io/2019/07/25/java/arraylist-Method/
https://da2uns2.tistory.com/entry/Java-ArrayList-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%86%8C%EB%93%9C
https://kadosholy.tistory.com/118


clone - https://hianna.tistory.com/567
