

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

### remove, removeAll - 삭제



### retainAll - 동일한 것만 남기고 삭제


### size() - 저장 개수 반환

```
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2); System.out.println(set1.size()); // 결과 : 2
```

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



- **TreeSet**  
    - 정렬방법을 지정할 수 있음
- **LinkedHashSet**  
    - 데이터를 중복해서 저장할 수없고, 입력한 순서대로 데이터를 저장한다



참조 - https://godsu94.tistory.com/173
HashSet - https://velog.io/@acacia__u/hashSet
https://staticclass.tistory.com/104