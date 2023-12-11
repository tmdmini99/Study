

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

HashSet의 성질
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


    
- **TreeSet**  
    - 정렬방법을 지정할 수 있음
- **LinkedHashSet**  
    - 데이터를 중복해서 저장할 수없고, 입력한 순서대로 데이터를 저장한다



참조 - https://godsu94.tistory.com/173
HashSet - https://velog.io/@acacia__u/hashSet
