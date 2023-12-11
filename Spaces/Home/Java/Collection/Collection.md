Java에서 컬렉션(Collection)이란 데이터의 집합, 그룹을 의미하며 

  

JCF(Java Collections Framework)는 이러한 데이터, 자료구조인 컬렌션과 이를 구현하는 클래스를 정의하는 인터페이스를 제공한다.

  

다음은 Java 컬렌션 프레임워크의 상속구조를 나타낸다.





![[Pasted image 20231211115426.png]]




■ **Collection 인터페이스의 특징**

| 인터페이스 | 구현클래스                                | 특징                                                                                                                                                |
| ---------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Set        | HashSet<br><br>TreeSet<br><br>LinkedHashSet                    | 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.                                                                             |
| List       | LinkedList<br><br>Vector<br><br>ArrayList | 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.                                                                                             |
| Queue      | LinkedList<br><br>PriorityQueue           | List와 유사                                                                                                                                         |
| Map        | Hashtable<br><br>HashMap<br><br>TreeMap   | 키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,<br><br>순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다. |






**3. Map 인터페이스**

  

키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,

순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.

  

- **Hashtable**  
    - HashMap보다는 느리지만 동기화 지원  
    - null불가  
      
    
- **HashMap**  
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.  
      
    
- **TreeMap**  
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름






참조 - https://gangnam-americano.tistory.com/41




