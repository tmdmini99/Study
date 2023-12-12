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


### HashMap Method


주요 메소드

| 메소드                                         | 설명                                                                                                                                  |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| void clear()                                   | 해당 맵(map)의 모든 매핑(mapping)을 제거함.                                                                                       |
| boolean containsKey(Object key)                | 해당 맵이 전달된 키를 포함하고 있는지를 확인함.                                                                                   |
| boolean containsValue(Object value)            | 해당 맵이 전달된 값에 해당하는 하나 이상의 키를 포함하고 있는지를 확인함.                                                         |
| V get(Object key)                              | 해당 맵에서 전달된 키에 대응하는 값을 반환함.<br><br>만약 해당 맵이 전달된 키를 포함한 매핑을 포함하고 있지 않으면 null을 반환함. |
| boolean isEmpty()                              | 해당 맵이 비어있는지를 확인함.                                                                                                    |
| Set'<'K> keySet()                              | 해당 맵에 포함되어 있는 모든 키로 만들어진 Set 객체를 반환함.                                                                     |
| V put(K key, V value)                          | 해당 맵에 전달된 키에 대응하는 값으로 특정 값을 매핑함.                                                                           |
| V remove(Object key)                           | 해당 맵에서 전달된 키에 대응하는 매핑을 제거함.                                                                                   |
| boolean remove(Object key, Object value)       | 해당 맵에서 특정 값에 대응하는 특정 키의 매핑을 제거함.                                                                           |
| V replace(K key, V value)                      | 해당 맵에서 전달된 키에 대응하는 값을 특정 값으로 대체함.                                                                         |
| boolean replace(K key, V oldValue, V newValue) | 해당 맵에서 특정 값에 대응하는 전달된 키의 값을 새로운 값으로 대체함.                                                             |
| int size()                                     | 해당 맵의 매핑의 총 개수를 반환함.                                                                                                |







| 메소드                                                                                            | 설명                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **void** clear()                                                                                  | HashMap 안에 들어있던 기존에 요소들을 모두 지운다. 반환값은 없다.                                                                                                                                                                                                                                                         |
| **boolean** isEmpty()                                                                             | HashMap 에 element가 있는지를 판단한다. 없다면 true를 있다면 false를 반환한다.                                                                                                                                                                                                                                            |
| **int** size()                                                                                    | size 메소드는 Map의 갯수를 리턴한다.                                                                                                                                                                                                                                                                                      |
| **boolean** containsKey(Object Key)                                                               | 인자로 주어진 Key가 현재 HashMap에 존재하는지를 판단하여 boolean값을 반환한다.                                                                                                                                                                                                                                            |
| **boolean** containsValue(Object value)                                                           | 인자로 주어진 Value를 가진 Key가 현재 HashMap에 존재하는지를 판단하여 boolean값을 반환한다.                                                                                                                                                                                                                               |
| **Set<Map.Entry<K, V>>** entrySet()                                                               | HashMap의 모든 요소를 'Key=Value' 형태로 묶어 Set으로 반환한다.                                                                                                                                                                                                                                                           |
| **Set'<'K>** keySet()                                                                             | HashMap의 모든 요소의 키만 'Key' 형태로 묶어 Set으로 반환한다.                                                                                                                                                                                                                                                            |
| **Collection'<'V>** values()                                                                      | HashMap의 모든 요소의 값만 묶어 반환한다.                                                                                                                                                                                                                                                                                 |
| **V** get(Object key)                                                                             | 인자로 주어진 key와 매핑되는 value를 반환해준다. HashMap에 key가 없다면 null을 반환한다.                                                                                                                                                                                                                                  |
| **V** put(K key, V value)                                                                         | 인자로 주어진 key=value 쌍을 HashMap에 추가한다. 만약 이미 HashMap안에 key가 존재할 경우, 나중에 put된 value가 들어간다.                                                                                                                                                                                                  |
| **V** remove(Object key)                                                                          | HashMap에 주어진 key가 있으면 그 key=value 쌍을 제거하고 value를 반환한다. 주어진 key가 HashMap에 없다면 null을 반환한다.                                                                                                                                                                                                 |
| **V** replace(K key, V value)                                                                     | 기존에 존재하던 HashMap의 key=old_value를 새로운 key=value로 바꾼다.  replace에 성공하면 기존에 존재하던 old_value를 반환하고, key가 존재하지않아 replace에 실패하면 null을 반환한다.                                                                                                                                     |
| **void** forEach(BiConsumer'<''? super K,? super~~ V> action)                                     | - forEach를 사용하여 HashMap의 각 key=value 쌍에 접근할 수 있다. lambda식도 사용가능하므로 Iterator를 통한 순회보다 더욱 더 간단하게 코드를 짤 수 있다.  keySet()이나 entrySet(), values()를 통하여 만들어진 Set들도 forEach문으로 접근이 가능하다.                                                                       |
| **V** getOrDefault(Object key, V defaultValue)                                                    | 기존의 HashMap.get(K key) 메소드는 해당 key가 존재하지 않을 경우 null을 반환했다. 그러나 HashMap.getOrDefault() 메소드는 key가 존재할 경우 key에 매핑되는 value를 반환하고, key가 존재하지 않는다면 defaultValue를 반환한다.                                                                                              |
| **V** putIfAbsent(K key, V value)                                                                 | putIfAbsent의 성공시 반환값은 put과 동일하다. (key가 존재하지 않아서 성공하면 null 반환) 그러나 put은 key가 이미 존재하면 새로운 value로 업데이트를 해버린다. putIfAbsent는 key가 기존에 없을 때만 put이 진행된다. 기존에 이미 key가 존재한다면, 그 key에 매핑되는 value를 반환하고, 새로운 value를 업데이트 하지 않는다. |
| **V** computeIfAbsent(K key, Function'<''? super K, ? extends V> mappingFunction)                 | computeIfAbsent()는 HashMap에 파라미터로 전달된 key가 없으면, mappingFunction이 호출되는 구조이다. 만약 key가 있다면, mappingFunction은 호출되지 않는다. key가 없으면 값을 구하기 위하여 mappingFunction을 호출하여 가져온 후, key=value 값을 HashMap에 추가한다.                                                         |
| **V** computeIfPresent(K key, BiFunction'<''? super K, ? super V, ? extends V> remappingFunction) | computeIfPresent()는 HashMap에 파라미터로 전달된 key가 있으면, remappingFunction이 호출되는 구조이다. 만약 key가 없다면, remappingFunction은 호출되지 않는다. key가 있으면 값을 구하기 위하여 remappingFunction을 호출하여 가져온 후, key=value 값을 HashMap에 갱신한다.                                                  |




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


#### HashMap 값 추가

```java
CopyHashMap<Integer,String> map = new HashMap<>();//new에서 타입 파라미터 생략가능
map.put(1,"사과"); //값 추가
map.put(2,"바나나");
map.put(3,"포도");
```

HashMap에 값을 추가하려면 put(key,value) 메소드를 사용하면 됩니다. 선언 시 HashMap에 설정해준 타입과 같은 타입의 Key와 Value값을 넣어야 하며 만약 입력되는 키 값이 HashMap 내부에 존재한다면 기존의 값은 새로 입력되는 값으로 대치됩니다.


#### HashMap 값 삭제

```java
CopyHashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
}};
map.remove(1); //key값 1 제거
map.clear(); //모든 값 제거
```

HashMap에 값을 제거하려면 remove(key) 메소드를 사용하면 됩니다. 오직 키값으로만 Map의 요소를 삭제할 수 있습니다. 모든 값을 제거하려면 clear() 메소드를 사용하면 됩니다.


### HashMap 값 출력

```java
CopyHashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
}};
		
System.out.println(map); //전체 출력 : {1=사과, 2=바나나, 3=포도}
System.out.println(map.get(1));//key값 1의 value얻기 : 사과
		
//entrySet() 활용
for (Entry<Integer, String> entry : map.entrySet()) {
    System.out.println("[Key]:" + entry.getKey() + " [Value]:" + entry.getValue());
}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:바나나
//[Key]:3 [Value]:포도

//KeySet() 활용
for(Integer i : map.keySet()){ //저장된 key값 확인
    System.out.println("[Key]:" + i + " [Value]:" + map.get(i));
}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:바나나
//[Key]:3 [Value]:포도
```

HashMap을 출력하는 방법에는 다양한 방법이 있습니다. 그냥 print하게 되면 {}로 묶어 Map의 전체 key값, value가 출력됩니다. 특정 key값의 value를 가져오고싶다면 get(key)를 사용하면 되고 전체를 출력하려면 entrySet()이나 keySet()메소드를 활용하여 Map의 객체를 반환받은 후 출력하면 됩니다. entrySet()은 key와 value 모두가 필요할 경우 사용하며 keySet()은 key 값만 필요할 경우 사용하는데 key값만 받아서 get(key)를 활용하여 value도 출력할 수도 있기에 어떤 메소드를 선택하든지 간에 큰 상관이 없어 대부분 코드가 간단한 keySet을 활용하시던데 key값을 이용해서 value를 찾는 과정에서 시간이 많이 소모되므로 많은 양의 데이터를 가져와야 한다면 entrySet()이 좋습니다.(약 20%~200% 성능 저하가 있음)



### Iterator 사용

```java
CopyHashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
}};
		
//entrySet().iterator()
Iterator<Entry<Integer, String>> entries = map.entrySet().iterator();
while(entries.hasNext()){
    Map.Entry<Integer, String> entry = entries.next();
    System.out.println("[Key]:" + entry.getKey() + " [Value]:" +  entry.getValue());
}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:바나나
//[Key]:3 [Value]:포도
		
//keySet().iterator()
Iterator<Integer> keys = map.keySet().iterator();
while(keys.hasNext()){
    int key = keys.next();
    System.out.println("[Key]:" + key + " [Value]:" +  map.get(key));
}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:바나나
//[Key]:3 [Value]:포도
```

HashMap의 전체출력 시 반복문을 사용하지 않고 Iterator를 사용하여도 됩니다. iterator로 Map안의 전체 요소를 출력하는 방법은 위와 같습니다.

      
    
## **TreeMap**  
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름





참조 - https://devlogofchris.tistory.com/41 HashMap 
https://gre-eny.tistory.com/97
