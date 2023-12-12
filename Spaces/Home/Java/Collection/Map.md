**1. Map 인터페이스**

  
# **Map이란?**
Map 인터페이스는 Collection 인터페이스와는 다른 저장 방식을 가진다.
키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,
순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.
여기서 키(key)란 실질적인 값(value)을 찾기 위한 이름의 역할을 한.
  

##  **Hashtable**  
    - HashMap보다는 느리지만 동기화 지원  
    - null불가  
      
    
## **HashMap**  
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.  
HashMap은 Map 인터페이스를 구현한 대표적인 Map 컬렉션입니다. Map 인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있습니다. Map은 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있는 자료구조입니다. 여기서 키와 값은 모두 객체입니다. 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없습니다. 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치됩니다.
###  HashMap 사용법

#### HashMap 선언

```java
CopyHashMap<String,String> map1 = new HashMap<String,String>();//HashMap생성
HashMap<String,String> map2 = new HashMap<>();//new에서 타입 파라미터 생략가능
HashMap<String,String> map3 = new HashMap<>(map1);//map1의 모든 값을 가진 HashMap생성
HashMap<String,String> map4 = new HashMap<>(10);//초기 용량(capacity)지정
HashMap<String,String> map5 = new HashMap<>(10, 0.7f);//초기 capacity,load factor지정
HashMap<String,String> map6 = new HashMap<String,String>(){{//초기값 지정
    put("a","b");
}};
```

HashMap을 생성하려면 키 타입과 값 타입을 파라미터로 주고 기본생성자를 호출하면 됩니다. HashMap은 저장공간보다 값이 추가로 들어오면 List처럼 저장공간을 추가로 늘리는데 List처럼 저장공간을 한 칸씩 늘리지 않고 약 두배로 늘립니다. 여기서 과부하가 많이 발생합니다. 그렇기에 초기에 저장할 데이터 개수를 알고 있다면 Map의 초기 용량을 지정해주는 것이 좋습니다. 해당 내용은 아래 링크에서 상세히 기술되어 있습니다.



      
    
## **TreeMap**  
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름





참조 - https://devlogofchris.tistory.com/41
