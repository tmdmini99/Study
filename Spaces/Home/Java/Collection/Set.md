

** Set 인터페이스**
순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.
**셋(set :집합)** 이란 데이터 자료구조(데이터 컬렉션) 중에 하나로 특정한 값들을 저장하는 추상자료형 입니다.
이때, **값들을 순서가 존재하지 않으며 중복되지 않습니다.** 
이는 수학에서의 유한 집합을 컴퓨터 구현한 것입니다. 
다른 모음(Collection) 타입에서 특정 원소를 검색하는 것이 주 업무인 반면, 집합은 대상 원소가 집합에 소속되었는지 여부를 검사한다.

Set은 비선형 구조이기 때문에 '순서'의 개념과 '인덱스'가 존재하지 않는다.  
때문에 값을 추가 / 삭제 하는 경우 Set 내부에 해당 값을 검색하여 해당 기능을 수행해야 한다. 이로 인해 처리 속도가 List구조에 느리다는 것이 단점이다.



> **Set 특성**

1. 데이터를 비순차적으로 저장할 수 있는 순열 자료구조
2. 삽입(Insert)한 데이터가 순서대로 저장되지 않음
3. 수정 가능(mutable)
4. 중복해서 삽입이 불가능. 동일한 값이 삽입되면 하나의 값만 저장됨
5. Fast Lookup이 필요할 때 주로 쓰임


Java에서는 **java.util.Set** 클래스를 임포트 하면 Set을 사용할 수 있습니다.  
Set는 인터페이스로 직접 생성해서 사용할 수 없고, HashSet, TreeSet, LinkedHashSet 등의 클래스로 구현해서 사용해야 합니다.
```java
import java.util.HashSet; // <--HashSet 
import java.util.Set;  // <--Set 
/* init Set */ 
Set<String> set = new HashSet<>();
```

  

## **HashSet**  
    - 가장빠른 임의 접근 속도  
    - 순서를 예측할 수 없음  
	HashSet은 `Set 인터페이스`에서 지원하는 구현 클래스이다. 때문에 `Set`의 성질을 그대로 상속받는 다는 것이 특징이다.


### HashSet 메서드


| 메소드                       | 설 명                                                  |
| ---------------------------- | ------------------------------------------------------ |
| boolean add(Object obj)      | 새로운 객체를 저장한다.                                |
| boolean addAll(Collection c) | 주어진 컬렉션에 저장된 모든 객체들을 추가한다.(합집합) |
|void clear()|저장된 모든 객체를 삭제한다.|
|Object clone()|HashSet을 복사해서 반환한다.(얕은 복사)|
|boolean contains(Object obj)|지정된 객체를 포함하고 있는지 알려준다.|
|boolean containsAll(Collection c)|주어진 컬렉션에 저장된 모든 객체들을 포함하고 있는지 알려준다.|
|boolean isEmpty()|HashSet이 비었는지 알려준다.|
|Iterator iterator()|Iterator를 반환한다.|
|boolean remove(Object obj)|지정된 객체를 HashSet에서 삭제한다.  <br>(성공하면 true, 실패하면 false를 반환한다.)|
|boolean removeAll(Collection c)|주어진 컬렉션에 저장된 모든 객체와 동일한 것들을   <br>HashSet에서 삭제한다.(차집합)  <br>(성공하면 true, 실패하면 false를 반환한다.)|
|boolean retainAll(Collection c)|주어진 컬렉션에 저장된 객체와 동일한 것만 남기고 삭제한다. (교집합)|
|int size()|저장된 객체의 개수를 반환한다.|
|Object[] toArray()|저장된 객체들을 객체배열의 형태로 반환한다.|
|Object[] toArray(Object[] arr)|저장된 객체들을 주어진 객체배열(arr)에 담는다.|




### HashSet의 성질

1. HashSet은 중복된 값을 허용하지 않습니다.

  값의 존재 유무를 파악할 때 사용할 수 있다.

2. List 등과는 다르게 저장한 순서가 보장되지 않습니다.

  저장 순서를 유지해야 한다면 JDK 1.4부터 제공되는 클래스 
  LinkedHashSet을 사용해야 한다.

3. null을 값으로 허용합니다.


HashSet 내부 코드를 보면 [[Map|HashMap]]을 사용하여 구현되어 있는 것을 볼 수 있다.



|생성자|설 명|
|---|---|
|HashSet()|HashSet객체를 생성한다.|
|HashSet(Colleaction c)|주어진 컬렉션을 포함하는 HashSet객체를 생성한다.|
|HashSet(int initialCapacity)|주어진 값을 초기용량으로 하는 HashSet객체를 생성한다.|
|HashSet(int initialCapacity, float loadFactor)|초기용량과 load factor를 지정하는 생성자.|

**load factor?**

저장공간이 가득 차기 전에 미리 용량을 확보하기 위한 것

예를 들어 이 값이 0.7면 저장공간의 70%가 채워졌을 때 용량이 2배로 늘어난다.

지정 하지 않았을때의 디폴트 값은 75%로 load factor가 0.75이다.


### add, addAll 객체 추가

```java
Set hashSet1 = new HashSet();// set은 중복이 불가하기 때문에 중복된 노드들은 제거됐다.
hashSet1.add(1); // Integer 타입의 숫자 1
hashSet1.add("1"); // 문자열 1
hashSet1.add("2"); // 문자열 2
hashSet1.add("2"); // 문자열 2
hashSet1.add("3"); // 문자열 3
hashSet1.add("3"); // 문자열 3 //숫자 1과 문자 1은 타입이 다르기 때문에 제거되지 않는다.
System.out.println(hashSet1); //결과 : [1, 1, 2, 3]
```

```java
Set hashSet2 = new HashSet(); 
hashSet2.add("1");
hashSet2.add("2");
hashSet2.add("4"); //set은 중복이 불가하기 때문에 hashSet1의 노드중 
//hashSet2의 노드와 동일한 값이 있다면 제거된다.
hashSet2.addAll(hashSet1); // hashSet1의 모든 노드를 hashSet2에 저장한다.
System.out.println(hashSet2); // 결과 : [[1, 1, 2, 3, 4]
```

- HashSet 내부에 존재하지 않는다면 그 값을 HashSet에 추가하고 true 를 반환한다.
- HashSet 내부에 존재한다면 false를 반환한다.



### clear - 모든 데이터 삭제

```java
Set set = new HashSet(); 
set.add(1);set.add(2);
System.out.println(set); // 결과 : [1, 2] 
set.clear();System.out.println(set); // 결과 : []
```


### clone - 복제

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
System.out.println(set1); // 결과 : [1, 2] 
HashSet set2 = (HashSet) set1.clone();
system.out.println(set2); // 결과 : [1, 2]
```

### contains, containsAll - 데이터 확인

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
System.out.println(set1.contains(3)); // 결과 : false 
HashSet set2 = new HashSet();
set2.add(1);
set2.add(2);
System.out.println(set2.containsAll(set1)); // true
```

원하는 값에 대해 `contains(value)` 메소드를 통해 Hash 내부에 존재하는 지 확인이 가능하다.



### isEmpty() - 비었는지 확인

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
System.out.println(set1.isEmpty()); // 결과 : false 
HashSet set2 = new HashSet();
System.out.println(set2.isEmpty()); // true
```


### iterator() - Iterator반환

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2); 
Iterator it = set1.iterator(); 
while (it.hasNext()) {	
	System.out.print(it.next()); // 결과 : 12
}
```

Set 컬렉션을 그냥 'print' 처리 할 경우 대괄호( '[ ]' )로 묶여 Set의 전체값이 출력된다.  
때문에, 전체 객체를 대상으로 한번씩 반복해서 가져오는 `반복자 (Iterator)`를 사용해 출력해야 한다.

### remove, removeAll - 삭제

```java

HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);

set1.remove(2); // true;
set1.remove(3); // false;

System.out.println(set1); // 결과 : [2]

HashSet set2 = new HashSet();
set2.add(1);
set2.add(2);
set2.add(3);

set2.removeAll(set1); // 하나라도 삭제되면 true
System.out.println(set2); // 결과 : [2, 3]
```

`remove(value)` 와 `clear()` 메소드를 사용하여 Hash값을 제거할 수 있다.
만약 삭제하려는 값이

- HashSet 내부에 존재한다면 그 값을 삭제하고 true를 반환한다.
- HashSet 내부에 존재하지 않는 다면, false를 반환한다.
### retainAll - 동일한 것만 남기고 삭제

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
set1.add(3);

HashSet set2 = new HashSet();
set2.add(1);
set2.add(2);
set2.add(4);



set2.retainAll(set1); // 일치하는 1, 2 외에 다 제거|

System.out.println(set2); // 결과 : [1, 2]|

```
### size() - 저장 개수 반환

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2); 
System.out.println(set1.size()); // 결과 : 2
```

	size()` 메소드를 사용하여 Hash의 크기를 구할 수 있다.

### toArray() - 배열화

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
set1.add(3); 
Object[] objArr = set1.toArray();
```

```java
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
set1.add(3); 
Object[] tmpArr = new Object[set1.size()];
Object[] objArr = set1.toArray(tmpArr);
```



## **TreeSet**  
    - 정렬방법을 지정할 수 있음
JDK 1.2부터 제공되고 있는 TreeSet은 HashSet과 마찬가지로 Set 인터페이스를 구현한 클래스로써 객체를 중복해서 저장할 수 없고 저장 순서가 유지되지 않는다는 Set의 성질을 그대로 가지고 있습니다. 
하지만 HashSet과는 달리 TreeSet은 이진 탐색 트리(BinarySearchTree) 구조로 이루어져 있습니다. 
이진 탐색 트리는 추가와 삭제에는 시간이 조금 더 걸리지만 정렬, 검색에 높은 성능을 보이는 자료구조입니다. 
그렇기에 HashSet보다 데이터의 추가와 삭제는 시간이 더 걸리지만 검색과 정렬에는 유리합니다. 
TreeSet은 데이터를 저장할 시 이진탐색트리(BinarySearchTree)의 형태로 데이터를 저장하기에 기본적으로 nature ordering를 지원하며 생성자의 매개변수로 Comparator객체를 입력하여 정렬 방법을 임의로 지정해 줄 수도 있습니다.

### TreeSet 메소드

| 메소드                                                                                              | 설명                                                                                                                                                                  |
| --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **boolean** add(Object o)                                                                           | ·       지정된 객체(o)의 객체들을 Collection에 추가합니다.                                                                                                            |
| **boolean** addAll(Collection c)                                                                    | ·       지정된 Collection(c)의 객체들을 Collection에 추가합니다.                                                                                                      |
| Object ceiling(Object o)                                                                            | ·       지정된 객체와 같은 객체를 반환합니다.  <br>·       없으면 큰 값을 가진 객체 중 제일 가까운 값의 객체를 반환합니다. 없으면 null                                |
| **void** clear()                                                                                    | ·       저장된 모든 객체를 삭제합니다.                                                                                                                                |
| Object clone()                                                                                      | ·       TreeSet을 복제하여 반환합니다.                                                                                                                                |
| Comparator comparator()                                                                             | ·       TreeSet의 정렬기준(Comparator)를 반환합니다.                                                                                                                  |
| **boolean** contains(Object o)                                                                      | ·       지정된 객체(o)의 객체들이 포함되어 있는지 확인합니다.                                                                                                         |
| **boolean** containsAll(Collection c)                                                               | ·       지정된 Collection의 객체들이 포함되어 있는지 확인합니다.                                                                                                      |
| NavigableSet descendingSet()                                                                        | ·       TreeSet에 저장된 요소들을 역순으로 정렬해서 반환합니다.                                                                                                       |
| Object first()                                                                                      | ·       정렬된 순서에서 첫 번째 객체를 반환합니다.                                                                                                                    |
| Object floor(Object o)                                                                              | ·       지정된 객체와 같은 객체를 반환합니다.  <br>·       없으면 작은 값을 가진 객체 중 가장 가까운 값의 객체를 반환합니다. 없으면 null                              |
| SortedSet headSet(Object toElement)                                                                 | ·       지정된 객체보다 작은 값의 객체들을 반환합니다.                                                                                                                |
| NavigableSet headSet(Object toElement, boolean inclusive)                                           | ·       지정된 객체보다 작은 값으 객체들을 반환합니다.  <br>·       inclusive가 true이면 같은 값의 객체도 포함합니다.                                                 |
| Object higher(Object o)                                                                             | ·       지정된 객체보다 큰 값을 가진 객체 중 제일 가까운 값의 객체를 반환합니다. 없으면 null                                                                          |
| **boolean** isEmpty()                                                                               | ·       TreeSet이 비어 있는지 확인합니다.                                                                                                                             |
| Iterator iterator()                                                                                 | ·       TreeSet의 Iterator를 반환합니다.                                                                                                                              |
| Object last()                                                                                       | ·       정렬된 순서에서 마지막 객체를 반환합니다.                                                                                                                     |
| Object lower(Object o)                                                                              | ·       지정된 객체보다 작은 값을 가진 객체 중 제일 가까운 값의 객체를 반환합니다. 없으면 null                                                                        |
| Object pollFirst()                                                                                  | ·       TreeSet의 첫번째 요소(제일 작은 값의 객체)를 반환합니다.                                                                                                      |
| Object pollLast()                                                                                   | ·       TreeSet의 마지막 요소(제일 큰 값의 객체)를 반환합니다.                                                                                                        |
| **boolean** remove(Object o)                                                                        | ·       지정된 객체를 제거합니다.                                                                                                                                     |
| **boolean** retainAll(Collection c)                                                                 | ·       주어진 컬렉션과 공통된 요소만을 남기고 삭제합니다.(교집합)                                                                                                    |
| **int** size()                                                                                      | ·       저장된 객체의 개수를 반환합니다.                                                                                                                              |
| Spliterator spliterator()                                                                           | ·       TreeSet의 spliterator를 반환합니다.                                                                                                                           |
| SortedSet subSet(Object frontElement, Object toElement)                                             | ·       범위검색(fromElement와 toElement사이)의 결과를 반환합니다.  <br>·       끝 범위인 toElement는 범위에 포함되지 않습니다.                                       |
| NavigableSet'<'E> subSet(E fromElement, **boolean** fromInclusive, E toElement, booleantoInclusive) | ·       범위검색(fromElement와 toElement사이)의 결과를 반환합니다.  <br>·       fromInclusive가 true이면 시작값이 포함되고, toInclusive가 true이면 끝값이 포함됩니다. |
| SortedSet tailSet(Object fromElement)                                                               | ·       지정된 객체보다 큰 값의 객체들을 반환합니다.                                                                                                                  |
| Object[] toArray()                                                                                  | ·       저장된 객체를 객체배열로 반환합니다.                                                                                                                          |
| Object[] toArray(Object[] a)                                                                        | ·       저장된 객체를 주어진 객체배열에 저장하여 반환합니다.                                                                                                          |

### 레드-블랙 트리(Red-Black Tree)

![[Pasted image 20231211181537.png]]


TreeSet은 이진탐색트리 중에서도 성능을 향상시킨 레드-블랙 트리(Red-Black Tree)로 구현되어 있습니다. 
일반적인 이진 탐색 트리는 트리의 높이만큼 시간이 걸립니다. 
데이터의 값이 트리에 잘 분산되어 있다면 효율성에 큰 문제가 없으나 데이터가 들어올 때 값이 편향되게 들어올 경우 한쪽으로 크게 치우쳐진 트리가 되어 굉장히 비효율적인 퍼포먼스를 냅니다. 이 문제를 보완하기 위해 레드 블랙 트리가 등장하였습니다. 
레드 블랙 트리는  부모노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여 데이터의 추가나 삭제 시 트리가 한쪽으로 치우쳐지지 않도록 균형을 맞추어줍니다.


###  TreeSet 사용법

#### TreeSet 선언
```java
TreeSet<Integer> set1 = new TreeSet<Integer>();//TreeSet생성

TreeSet<Integer> set2 = new TreeSet<>();//new에서 타입 파라미터 생략가능

TreeSet<Integer> set3 = new TreeSet<Integer>(set1);//set1의 모든 값을 가진 TreeSet생성

TreeSet<Integer> set4 = new TreeSet<Integer>(Arrays.asList(1,2,3));//초기값 지정
```
TreeSet을 생성하기 위해서는 저장할 객체 타입을 파라미터로 표기하고 기본 생성자를 호출하면 됩니다. 
생성하는 명령어는 HashSet과 크게 다르지 않으나 선언 시 크기를 지정해줄 수는 없습니다.



```java
TreeSet<Integer> set = new TreeSet<Integer>();//TreeSet생성

set.add(7); //값추가

set.add(4);

set.add(9);

set.add(1);

set.add(5);
```
TreeSet에 값을 추가하려면 add(value) 메소드를 사용하면 됩니다. 
입력되는 값이 TreeSet 내부에 존재하지 않는다면 그 값을 추가한 뒤 true를 반환하고 내부에 값이 존재한다면 false를 반환합니다.
#### TreeSet에 값이 추가되는 과정
![[Pasted image 20231211181812.png]]
7,4,9,2,5를 차례대로 TreeSet에 저장한다면 위와같은 과정을 거치게 됩니다.


#### TreeSet 값 삭제
```java
TreeSet<Integer> set = new TreeSet<Integer>();//TreeSet생성

set.remove(1);//값 1 제거

set.clear();//모든 값 제거
```
TreeSet에 값을 삭제하려면 remove(value) 메소드를 사용하면 됩니다. 
매개변수 value의 값이 존재한다면 그 값을 삭제한 후 true를 반환하고 없으면 false를 반환합니다. 모든 값을 제거하려면 clear() 메서드를 사용하면 됩니다.


#### TreeSet 크기 구하기

```java
TreeSet<Integer> set = new TreeSet<Integer>(Arrays.asList(1,2,3));//초기값 지정

System.out.println(set.size());//크기 : 3
```
TreeSet의 크기를 구하려면 size() 메소드를 사용하면 됩니다.

#### TreeSet 값 출력

```java
TreeSet<Integer> set = new TreeSet<Integer>(Arrays.asList(4,2,3));//초기값 지정
System.out.println(set); //전체출력 [2,3,4]
System.out.println(set.first());//최소값 출력
System.out.println(set.last());//최대값 출력
System.out.println(set.higher(3));//입력값보다 큰 데이터중 최소값 출력 없으면 null
System.out.println(set.lower(3));//입력값보다 작은 데이터중 최대값 출력 없으면 null
		
Iterator iter = set.iterator();	// Iterator 사용
while(iter.hasNext()) {//값이 있으면 true 없으면 false
    System.out.println(iter.next());
}

```
TreeSet을 그냥 pirnt하게 되면 대괄호 []로 묶어서 전체 값이 출력됩니다. 
TreeSet에는 값을 리턴하는 다양한 메서드가 있는데 first() 메서드는 최솟값, last()는 최댓값, higher(value)은 입력값보다 큰 데이터중 최솟값, lower(value)은 입력값보다 작은 데이터중 최대값을 리턴합니다. 
TreeSet에는 객체 전체를 대상으로 한 번씩 반복해서 가져오는 반복자(Iterator)를 제공합니다. 
반복자 이터레이터는 iterator() 메서드를 호출하면 얻을 수 있습니다. 
iterator에서 하나의 객체를 가져올 때에는 next() 메서드를 사용합니다. 
next() 메서드를 사용하기 전에 먼저 가져올 객체가 있는지 hasNext() 메서드를 활용하여 먼저 확인하는 것이 좋습니다. 
hasNext() 메서드는 가져올 객체가 있으면 true, 없으면 false를 리턴합니다.




## **LinkedHashSet**  
    - 데이터를 중복해서 저장할 수없고, 입력한 순서대로 데이터를 저장한다
HashSet과 동일한 구조를 가지지만 HashSet은 순서를 관리하지 않아 값을 출력할 때마다 다른 순서대로 출력이 됩니다

하지만 LinkedHashSet은 삽입된 순서대로 반복합니다

HashSet과 동일한 특징들이 있는데 마찬가지로 중복 값을 허용하지 않습니다

### **LinkedHashSet 선언하기**

```java
import java.util.Arrays;
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet hs = new LinkedHashSet(); // 타입 설정x Object 입력
		LinkedHashSet<LinkedHashSetDemo> demo = new LinkedHashSet<LinkedHashSetDemo>(); // 클래스로 타입 설정
		LinkedHashSet<Integer> i = new LinkedHashSet<Integer>(); // Integer 타입 선언
		LinkedHashSet<Integer> i2 = new LinkedHashSet(); // 뒷부분 타입 선언 생략 가능
		LinkedHashSet<Integer> i3 = new LinkedHashSet<Integer>(10); // 크기 10으로 선언
		LinkedHashSet<Integer> i4 = new LinkedHashSet<Integer>(Arrays.asList(1, 2, 3, 4)); // 선언과 동시에 초기 값 설정
		
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // String 타입 선언
		LinkedHashSet<Character> ch = new LinkedHashSet<Character>(); // Char 타입 선언		
	}
}
```




참조 - https://godsu94.tistory.com/173
HashSet - https://velog.io/@acacia__u/hashSet
https://staticclass.tistory.com/104

TreeSet - https://coding-factory.tistory.com/555

LinkedHashSet - https://crazykim2.tistory.com/582
