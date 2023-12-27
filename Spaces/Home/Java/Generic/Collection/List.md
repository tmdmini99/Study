List 인터페이스
순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.

![[자료구조.png]]




# **LinkedList**  
    - 양방향 포인터 구조로 데이터의 삽입 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용 
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임  
    - LinkedList란 [[Collection]] 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다


 
###  ***1. LinkedList란?***

> 각 노드가 '데이터'와 '포인터'를 가지고 한 줄로 연결되어 잇는 방식으로 저장하는 자료구조

* 데이터 : 실제 값이 저장되는 장소.

* 포인터 : 다음 노드의 주소값이 저장되는 장소

 쉽게 말해 LinkedList는 엘리먼트와 엘리먼트 간의 연결(Link)을 통해서 리스트를 구현한 자료구조입니다. 배열을 통해서 리스트를 구현한 ArrayList와는 대비됩니다.

 ArrayList에선 각각의 데이터를 담은 요소들을 엘리먼트(element)라고 표현하였지만 LinkedList는 각각의 요소들이 연결이라는 특성을 가지기 때문에 "node(마디,교점)"혹은 "vertex(꼭지점, 정점)"라고 표현합니다. 이러한 노드를 표현하기 위해서 C언어는 '구조체'를 사용하고 객체지향 프로그래밍 언어에선 '객체'를 사용합니다.

![[arrayList.jpg]]

![[LinkedList.jpg]]

Data field : 실제 값이 저장되는 변수입니다.

Link field : 다음 노드의 포인터나 참조값을 저장하는 변수입니다.

node(or vertex) :  LinkedList에서 데이터가 저장되는 요소입니다. Data field와 Linke field로 구성되었습니다.

HEAD : 첫번째 노드를 가리키는 변수.

 ArrayList구조(그림 1)의 경우 하나의 주소 값에 배열을 생성하여 데이터들을 저장하였습니다. 그와 다르게 LinkedList의 경우 데이터가 저장되는 하나하나의 요소들이 주소 값을 차지합니다. 이러한 개개의 요소들은 LinkedList에서 노드라고 표현되는데 이렇게 흩어진 노드들은 각 노드들이 다음 노드를 지목함으로써 데이터들 간에 관계를 가지게 됩니다.

 LinkedList에서 저장한 값을 찾기 위해서든, 값을 추가하거나 삭제하든 반드시 첫번째 노드 값을 알아야만 데이터를 다룰 수 있습니다. 따라서 첫 번째 노드를 가리키는 HEAD는 linkedList에서 매우 중요합니다.






![[단방향.png]]

 이런 LinkedList가 존재하고 있을 때 0x01의 Node가 삭제된다면 0x00 Node가 다음 Node로 참조하고 있던 주소는 0x01이 아니라 0x02가 되어서 연결된 주소를 변경시켜 준다면 삭제가 된다.

![[LinkedList 순서 바꾸기.png]]

추가는 Node와 Node 사이에 새로운 Node를 만들고 삭제에서 했던 것처럼 주소를 이동시켜준다.


![[LinkedList2.png]]


 0x04의 주소를 가진 Node를 새로 추가를 해주었기 때문에 그 이전의 0x00 Node는 0x04 주소를 참조하고 0x04가 0x00이 참조하던 0x01을 주소를 참조한다.
 단, 이 LinkedList는 `Singly LinkedList(단일 연결 리스트)`라고 불린다. 다음 요소로 이어지는 방향이 단방향이기 때문에 이렇게 지어졌는데 단방향이라 이전 요소로 넘어가는 것이 어려워서 실제 LinkedList는 `Doubly LinkedList(이중 연결 리스트)`를 사용하며 지금도 이중 연결 리스트를 구현할 것이다.

 이중 연결 리스트라고 막 복잡한 것이아니라 Node들에 이전의 Node에 대한 주소를 추가해주는 것에 불과하다.


![[LinkedList3.png]]
Double LinkedList

  

 따라서 우리가 어떠한 node를 찾을 때 굳이 header부터 시작하지 않고 중간의 어떤 node에서 검색을 시작하더라도 검색이 가능하게 변경되었다.




### **2.  데이터 조작과정**

 이번 포스팅에선 LinkedList가 데이터를 검색, 삭제, 추가하는 과정을 이해해보고자 합니다. 왜냐하면 이 과정을 이해한다면 LinkedList가 ArrayList에 비해 데이터를 추가하고 삭제하는 것이 왜 빠른지와 LinkedList가 ArrayList에 비해 데이터를 검색하는 과정은 왜 늦는지를 알 수 있기 때문입니다. 

 LinkedList의 데이터 추가와 삭제는 visualgo.net 홈페이지를 통해서 이해해보고자 합니다.
#### **(1) 데이터 추가**

 LinkeList에서 새로운 노드를 추가하는 전체 적인 과정은 다음과 같습니다. ( 편의를 위해서 새로 추가될 노드를 'new노드', 새로추가 될 노드의 바로 앞 노드를 'before노드', 새로 추가될 노드의 바로 뒤 노드를 'after노드'라고 지칭하겠습니다.)

1. 가장 먼저 'new노드'와 'before노드'를 찾습니다.

2. 'new노드'를 생성합니다.

3. 'before노드'의 Link field(다음 노드를 가리키는 참조값을 저장하는 변수)를 'new노드'를 가리키도록 변경합니다.

4. 'new 노드'의 Linke filed를 'after노드'를 가리키도록 변경합니다.

**데이터 추가 속도 : ArrayList < LinkedList** 

 위의 과정을 보게 되면 LinkedList에서 새로운 노드를 추가하는 방식은 ArrayList에서 새로운 엘리먼트를 추가하는 방식보다 훨씬 간결하다는 것을 알 수 있습니다. ArrayList는 내부적으로 크기를 변경할 수 없는 배열을 사용하기 때문에 데이터를 추가할 시엔 크기가 더 큰 배열을 생성해서 값을 옮겨야 하는 복잡한 과정을 거치기 때문입니다.
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


## LinkedList Class 직접 구현

```java
package Week1;  
  
import java.util.*;  
  
  
  
class Node<E>{  
  
    public E data;  
    public Node<E> nextNode;  
    public Node<E> preNode;  
  
    public Node(){  
        //처음 생성시 값이 있으면 안되기 때문  
        this.data=null;  
        this.nextNode=null;  
        this.preNode=null;  
  
    }  
    public Node(E obj){  
        //처음 생성시 값이 있으면 안되기 때문  
        this.data=obj;  
        this.nextNode=null;  
        this.preNode=null;  
  
    }  
    public Node(E obj, Node next){  
        //첫번쨰  
        this.data=obj;  
        this.nextNode=next;  
        this.preNode=null;  
  
    }  
    public Node(E obj, Node next, Node pre){  
        //그 다음부터  
        this.data=obj;  
        this.nextNode=next;  
        this.preNode=pre;  
  
    }  
  
  
  
  
}  
  
public class MakeLinkedList<E> implements List<E> {  
  
  
    private Node<E> first;  
    private Node<E> last;  
    private int size;  
  
    public MakeLinkedList(){  
        size=0;  
    }  
  
  
  
  
    public void addLast(E e) {  
        //처음 일시 null값 저장  
        Node<E> l = this.last;  
        //새로운 객체 생성 객체를 만들어서 보관하는것이기 때문에 객체를 각각 생성해줘야함  
        Node<E> newNode = new Node<E>(e,null,l);  
        //이전노드값 저장  
        this.last=newNode;  
        //처음 값 입력했을때  
        if(l == null){  
            this.first = newNode;  
        }else {  
            //첫 노드 다음노드 주소 저장  
            l.nextNode=newNode;  
        }  
        size++;  
    }  
  
    public void addFirst(E e) {  
        Node<E> f = this.first;  
        Node<E> newNode = new Node<E>(e,first,null);  
        this.first=newNode;  
        if(f ==null){  
            this.last = newNode;  
        }else {  
            f.preNode=newNode;  
  
        }  
        size++;  
    }  

public E peek(){  
        return (first == null) ? null : first.data;  
}  
    @Override  
    public boolean add(E e) {  
        this.addLast(e);  
        return true;  
    }  
    public boolean offer(E e) {  
        this.addLast(e);  
        return true;  
    }  
  
    public String toString(){  
        if(size == 0){  
            return "데이터 없음";  
        }else {  
            Node<E> n = first;  
            StringBuilder sb = new StringBuilder();  
            sb.append("["+n.data);  
  
            if(first == null){  
                return "[]";  
            }else{  
                while (n.nextNode !=null){  
                    n=n.nextNode;  
                    sb.append(n.data);  
                }  
  
  
            }  
            sb.append("]");  
            return  sb.toString();  
        }  
    }  
  
  
    public E poll(){  
  
        return  (E) this.removeFirst();  
    }  
  
    @Override  
    public E remove(int index) {  
  
        return  null;  
    }  
    @Override  
    public boolean isEmpty() {  
  
        return first == null;  
    }  
    public Object removeFirst(){  
        Node<E> n = first;  
        if(size == 1){  
            first=null;  
            last=null;  
        }else{  
  
            first = first.nextNode;  
            first.preNode=null;  
        }  
            size--;  
        return n.data;  
    }  
    public Object pop(){  
        return this.removeFirst();  
    }  
  
  
    public Object removeLast(){  
        Node<E> n = last;  
        if(size == 1){  
            first=null;  
            last=null;  
        }else{  
            last = last.preNode;  
            last.nextNode=null;  
        }  
            size--;  
        return n.data;  
    }  
  
  
//````````````````````````````````````````````````````````````````````````````````````  
  
    @Override  
    public boolean remove(Object o) {  
        return false;  
    }  
    @Override  
    public int size() {  
        return size;  
    }  
    @Override  
    public void clear() {  
        last =null;  
        first=null;  
        size =0;  
    }  
  
    @Override  
    public boolean contains(Object o) {  
        boolean check = false;  
        Node<E> node = first;  
        while (node.nextNode != null){  
            if(node.data == o){  
                check = true;  
                break;  
            }  
            node = node.nextNode;  
        }  
        return check;  
    }  
  
    @Override  
    public E get(int index) {  
  
        Node<E> node = first;  
        int num=0;  
        while (node.nextNode != null){  
            if(num == index){  
                return node.data;  
            }  
            node = node.nextNode;  
        }  
        return null;  
    }  
  
    @Override  
    public E set(int index, E element) {  
        return null;  
    }  
  
    @Override  
    public ListIterator<E> listIterator() {  
        return null;  
    }  
  
    //----------------------------------------------------------  
  
  
  
    @Override  
    public Iterator<E> iterator() {  
        return null;  
    }  
  
    @Override  
    public Object[] toArray() {  
        return new Object[0];  
    }  
  
    @Override  
    public <T> T[] toArray(T[] a) {  
        return null;  
    }  
  
  
  
  
  
    @Override  
    public boolean containsAll(Collection<?> c) {  
        return false;  
    }  
  
    @Override  
    public boolean addAll(Collection<? extends E> c) {  
        return false;  
    }  
  
    @Override  
    public boolean addAll(int index, Collection<? extends E> c) {  
        return false;  
    }  
  
    @Override  
    public boolean removeAll(Collection<?> c) {  
        return false;  
    }  
  
    @Override  
    public boolean retainAll(Collection<?> c) {  
        return false;  
    }  
  
  
  
  
  
  
  
  
    @Override  
    public void add(int index, E element) {  
  
    }  
  
  
    @Override  
    public int indexOf(Object o) {  
        return 0;  
    }  
  
    @Override  
    public int lastIndexOf(Object o) {  
        return 0;  
    }  
  
  
  
    @Override  
    public ListIterator<E> listIterator(int index) {  
        return null;  
    }  
  
    @Override  
    public List<E> subList(int fromIndex, int toIndex) {  
        return null;  
    }  
}

```



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
| **.add((index), val)**      | 순서대로 리스트를 추가, 배열 사이즈 초과 시 초기 설정된 사이즈만큼 자동으로 사이즈가 증가함, <br>인덱스를 추가로 지정해주면 해당 인덱스에 값을 삽입 |
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


## ArrayList Class 직접구현

```java
package Week1;  
  
import java.util.*;  
import java.util.stream.IntStream;  
  
public class MakeArrayList<E> implements List<E> {  
    private int size;  
    //직접 제네릭 배열은 생성불가  
    private E [] list;  
    private int num;  
    public MakeArrayList(){  
        //강제 형변환을 이용하여 생성  
        //초기값을 0으로 하는 이유는 10개로 생성시 값을 집어넣지 않으면 0으로 표현되기 때문  
        list = (E[])new Object[0];  
        size=list.length;  
        num=0;  
    }  
    public boolean add(E e){  
        if(num== list.length){  
            E [] newList =(E[])new Object[list.length+1];  
            //배열을 for문을 사용하지 않고 복사하는 방법  
            //              원본  ,어디서부터 복사할지(index),  새로운 배열 이름   시작위치   원본에서 복사본으로 쓸 길이  
            System.arraycopy(list, 0,               newList, 0, list.length);  
            list=newList;  
        }  
        try {  
            list[num]=e;  
            num++;  
            return true;  
        }catch (Exception ee){  
            return false;  
        }  
    }  
    //Array.toString(list)  
    //배열의 값을 출력하기 위한 코드 그냥 String.valueof(list)하면 주소값이 나옴  
    /*public String toString() {  
        String str = "[";        for (int i = 0 ; i < size; i++) {            str += Data[i];            if(i < size -1) {                str+=",";            }        }        return str + "]";    }*/    public String toString(){  
        return Arrays.toString(list);  
    }  
    public boolean remove(Object obj){  
        //Stream을 사용하여 간단하게 출력  
        list=(E[])Arrays.stream(list).filter(item ->item.equals(obj)).toArray(Object[]::new);  
        return true;  
    }  
    public E remove(int index){  
        E e =list[index];  
        list= (E[])IntStream.range(0, list.length).filter(idx ->idx !=index).mapToObj(idx ->list[idx]).toArray(Object[]::new);  
        return e;  
    }  
    public int lastIndexOf(Object o){  
        return Arrays.asList(list).lastIndexOf(o);  
    }  
  
    @Override  
    public boolean containsAll(Collection<?> c) {  
        return false;  
    }  
  
    @Override  
    public boolean addAll(Collection<? extends E> c) {  
        return false;  
    }  
  
    @Override  
    public boolean addAll(int index, Collection<? extends E> c) {  
        return false;  
    }  
  
    @Override  
    public boolean removeAll(Collection<?> c) {  
        return false;  
    }  
  
    @Override  
    public boolean retainAll(Collection<?> c) {  
        return false;  
    }  
  
    @Override  
    public void clear() {  
  
        list =(E[]) new Object[0];  
    }  
  
    @Override  
    public E get(int index) {  
        return list[index];  
    }  
  
    @Override  
    public E set(int index, E element) {  
        E e =list[index];  
        list[index]=element;  
        return e;  
    }  
  
    @Override  
    public void add(int index, E element) {  
  
    }  
  
  
    @Override  
    public int indexOf(Object o) {  
        return 0;  
    }  
  
  
  
    @Override  
    public ListIterator<E> listIterator() {  
        return null;  
    }  
  
    @Override  
    public ListIterator<E> listIterator(int index) {  
        return null;  
    }  
  
    @Override  
    public List<E> subList(int fromIndex, int toIndex) {  
        return null;  
    }  
  
    public  int size(){  
  
  
        return list.length;  
    }  
  
    @Override  
    public boolean isEmpty() {  
        return false;  
    }  
  
    @Override  
    public boolean contains(Object o) {  
        return false;  
    }  
  
    @Override  
    public Iterator<E> iterator() {  
        return null;  
    }  
  
    @Override  
    public Object[] toArray() {  
        return new Object[0];  
    }  
  
    @Override  
    public <T> T[] toArray(T[] a) {  
        return null;  
    }  
  
  
}
```


---
참고- https://www.nextree.co.kr/p6506/  : Linked vs Array 차이
https://dev-coco.tistory.com/19 간단명료 이해하기 쉬움
https://crazykim2.tistory.com/566
https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0
https://kimvampa.tistory.com/79 

LinkedList - https://jinyoungchoi95.tistory.com/10



vector  https://crazykim2.tistory.com/570
https://blog.naver.com/manymoa/150004980532

ArrayList - https://crazykim2.tistory.com/558
https://junjangsee.github.io/2019/07/25/java/arraylist-Method/
https://da2uns2.tistory.com/entry/Java-ArrayList-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%86%8C%EB%93%9C
https://kadosholy.tistory.com/118


clone - https://hianna.tistory.com/567
