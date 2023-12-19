**1. Map 인터페이스**

ToDoList

# **Map이란?**
Map 인터페이스는 Collection 인터페이스와는 다른 저장 방식을 가진다.
키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,
순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.
여기서 키(key)란 실질적인 값(value)을 찾기 위한 이름의 역할을 한다.
  

##  **Hashtable**  
    - HashMap보다는 느리지만 동기화 지원  
    - null불가  

### **HashTable 선언하기**

```java
import java.util.Hashtable;

public class HashTableDemo {
	public static void main(String[] args)  {
		Hashtable ht = new Hashtable(); // 타입 설정x Object 설정
		Hashtable<Integer, Integer> i = new Hashtable<Integer, Integer>(); // Integer, Integer 타입 선언
		Hashtable<Integer, Integer> i2 = new Hashtable<>(); // new는 타입 생략 가능
		Hashtable<Integer, Integer> i3 = new Hashtable<Integer, Integer>(i); // i의 Hashtable을 i3으로 값 이전
		Hashtable<Integer, Integer> i4 = new Hashtable<Integer, Integer>(10); // 초기용량 지정
		Hashtable<Integer, Integer> i5 = new Hashtable<Integer, Integer>() {{ // 변수 선언 + 초기값 지정
			put(1, 100);
			put(2, 200);
		}};
		
		Hashtable<String, String> str = new Hashtable<String, String>(); // String, String 타입 선언
		Hashtable<Character, Character> ch = new Hashtable<Character, Character>(); // Char, Char 타입 선언
	}
}
```

HashTable을 선언하는 방법은 여러가지가 있습니다

HashTable은 하나의 Entry에 Key, Value 2개를 가지고 있습니다

타입을 선언하려면 두 개를 동시에 선언해줘야합니다

Hashtable<타입, 타입> 변수명 = new Hashtable<타입, 타입>(); 으로 선언을 해줍니다

나머지 HashTable의 선언 방법들은 위의 예제와 주석을 참고바랍니다

### **HashTable 값 추가하기**

```java
import java.util.Hashtable;

public class HashTableDemo {
	public static void main(String[] args)  {
		Hashtable<String, String> ht = new Hashtable<String, String>(); // Hashtable 선언
		
		// 값 추가
		ht.put("1", "Hello1");
		ht.put("2", "World2");
		ht.put("3", "Hello3");
		ht.put("4", "World4");
		ht.put("2", "WorldWorld2");
		
		System.out.println(ht); // 결과출력
	}
}
```

HashTable의 값을 추가하는 방법은 put(Key, Value) 메서드를 사용하여 값을 추가합니다

put() 메서드를 사용하여 Key가 같고 Value가 다른 값을 중복해서 넣으면 나중에 넣은 Value값으로 변경됩니다

**결과**

![](https://blog.kakaocdn.net/dn/bdUlXE/btq5MuaDAh2/RL6W74XnP2esJ5EgnbrYu0/img.png)


### **HashTable 값 삭제하기**

```java
import java.util.Hashtable;

public class HashTableDemo {
	public static void main(String[] args)  {
		Hashtable<String, String> ht = new Hashtable<String, String>(); // Hashtable 선언
		
		// 값 추가
		ht.put("1", "Hello1");
		ht.put("2", "World2");
		ht.put("3", "Hello3");
		ht.put("4", "World4");
		
		System.out.println(ht); // 결과출력
		
		ht.remove("2");
		System.out.println(ht); // 결과출력

		ht.clear();
		System.out.println(ht); // 결과출력
	}
}
```

HashTable의 값을 삭제하는 방법은 여러가지가 있습니다

그 중에서 remove(Key값) 메서드는 Key값에 해당하는 값을 하나 삭제해줍니다

clear() 메서드는 HashTable의 모든 값을 삭제할 때 사용합니다

---
## **HashMap**  
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.  
HashMap은 Map 인터페이스를 구현한 대표적인 Map 컬렉션입니다. Map 인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있습니다. Map은 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있는 자료구조입니다. 여기서 키와 값은 모두 객체입니다. 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없습니다. 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치됩니다.


### HashMap Method


주요 메소드

| 메소드                                         | 설명                                                                                                                              |
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
| V getKey()                                     | key값                                                                                                                           |
| V getValue()                                   | value값                                                                                                                                  |







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







>key.hashCode()는 key 객체의 해시코드를 반환하는 메서드이다. 자바에서는 모든 객체가 hashCode() 메서드를 가지고 있으며, 기본적으로는 객체의 메모리 주소를 기반으로 한 해시코드를 반환한다. 하지만 순차적으로 0, 1, 2, 3, ... 순서로 저장되는 것은 아니다.
그렇기 때문에 맵에 저장된 순서와 toString() 메서드로 출력된 순서가 일치하지 않을 수 있다.
만약 순서를 보장하고 싶다면, LinkedHashMap과 같은 자료구조를 사용하는 것이 좋다.
이를 통해 데이터를 순서대로 저장하고 검색할 수 있다.
순차적으로 0, 1, 2, 3, ... 순서로 저장되는 것은 아니지만 인덱스로 변환하는 이유는 두 가지가 있는데
> 1. 빠른 데이터 접근: 해시 맵은 배열을 사용하여 데이터를 저장하는데, 배열은 인덱스를 사용하여 상수 시간(O(1))에 데이터에 접근할 수 있는 장점이 있다. 만약 키를 인덱스로 변환하지 않고, 리스트나 연결 리스트 등 다른 자료구조를 사용한다면, 데이터를 검색하는 데에 선형 시간(O(n))이 걸릴 수 있지만, 키를 인덱스로 변환하여 배열에 저장함으로써 빠른 데이터 접근을 가능케 합니다.
> 2. 데이터 분산: 해시 맵은 키를 해시코드로 변환하여 배열의 인덱스로 매핑하는데, 이를 통해 데이터를 균일하게 분산시킬 수 있다. 해시 함수는 키의 고유한 해시코드를 생성하는데, 이 해시코드를 배열의 인덱스로 사용하면 데이터가 배열의 각 슬롯에 균등하게 분산된다. 이를 통해 데이터를 효율적으로 저장하고 검색할 수 있다.
따라서, 키를 해시코드로 변환하고, 이를 배열의 인덱스로 매핑하여 데이터를 저장하는 것은 빠른 데이터 접근과 데이터 분산을 위한 목적을 가지고 있다. 






      


---
## **TreeMap**  
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름

TreeMap은 이진트리를 기반으로 한 Map 컬렉션이다.  
   
같은 Tree구조로 이루어진 TreeSet과의 차이점은 TreeSet은 그냥 값만 저장한다면, TreeMap은 키와 값이 저장된 Map, Entry를 저장한다는 점이다.  
   
TreeMap에 객체를 저장하면 자동 정렬되는데, 키는 저장과 동시에 자동 오름차순으로 정렬되고 타입이 숫자일 경우 값으로, 문자열일 경우 유니코드로 정렬한다.   
   
정렬 순서는 기본적으로 부모 키값과 비교해서 키값이 낮은 것은 왼쪽 자식 노드에 키 값이 높은 것은 오른쪽 자식 노드에 Map.Entry 객체를 저장한다.  
   
TreeMap은 일반적으로 Map으로써 성능이 HashMap보다 떨어진다.  
TreeMap은 데이터를 저장할 때 즉시 정렬하기에 추가나 삭제가 HashMap보다 오래 걸린다.  
하지만 정렬된 상태로 Map을 유지해야 하거나 정렬된 데이터를 조회해야 하는 범위 검색이 필요한 경우 TreeMap을 사용하는 것이 효율성면에서 좋다.

### **레드-블랙 트리(Red-Black Tree)**
![[Pasted image 20231211181537.png]]


TreeSet은 이진탐색트리의 문제점을 보완한 레드-블랙 트리(Red-Black Tree)로 구현되어 있다.  
   
일반적인 이진 탐색 트리는 트리의 높이만큼 시간이 걸린다.  
   
데이터의 값이 트리에 잘 분산되어 있다면 효율성에 큰 문제가 없으나 데이터가 들어올 때 값이 편향되게 들어올 경우  
   
한 쪽으로 크게 치우쳐진 트리가 되어 굉장히 비효율적인 퍼포먼스를 낸다.  
   
이 문제를 보완하기 위해 레드 블랙 트리가 등장했다.  
   
레드 블랙 트리는 부모노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여  
   
데이터의 추가나 삭제 시 트리가 한 쪽으로 치우쳐지지 않도록 균형을 맞추어준다.


### **TreeMap 사용법**

**TreeMap 선언**

```java
TreeMap<Integer,String> map1 = new TreeMap<Integer,String>();//TreeMap생성 
TreeMap<Integer,String> map2 = new TreeMap<>();//new에서 타입 파라미터 생략가능 
TreeMap<Integer,String> map3 = new TreeMap<>(map1);//map1의 모든 값을 가진 TreeMap생성
TreeMap<Integer,String> map6 = new TreeMap<Integer,String>(){{//초기값 설정   
put(1,"a");}};
```

생성하는 명령어는 HashMap과 크게 다르지 않으나 **선언 시 크기를 지정할 수 없다.**

---

### **TreeMap 값 추가**

```java
TreeMap<Integer,String> map = new TreeMap<Integer,String>();//TreeMap생성
map.put(1, "사과");//값 추가
map.put(2, "복숭아");
map.put(3, "수박");
```

TreeMap은 구조만 HashMap과 다를 뿐 기본적으로 Map 인터페이스를 같이 상속받고 있으므로 기본적인 메소드의 사용법 자체는 HashMap과 동일하다.

---

### **TreeMap 값 삭제**

```java
TreeMap<Integer, String> map = new TreeMap<Integer,String>(){{//초기값 설정    
put(1, "사과");//값 추가    
put(2, "복숭아");    
put(3, "수박");}};
map.remove(1); //key값 1 제거
map.clear(); //모든 값 제거
```

---

### **TreeMap 단일 값 출력**

```java
TreeMap<Integer,String> map = new TreeMap<Integer,String>(){{//초기값 설정    
put(1, "사과");//값 추가    
put(2, "복숭아");   
put(3, "수박");}};		
System.out.println(map); //전체 출력 : {1=사과, 2=복숭아, 3=수박}
System.out.println(map.get(1));//key값 1의 value얻기 : 사과
System.out.println(map.firstEntry());//최소 Entry 출력 : 1=사과
System.out.println(map.firstKey());//최소 Key 출력 : 1
System.out.println(map.lastEntry());//최대 Entry 출력: 3=수박
System.out.println(map.lastKey());//최대 Key 출력 : 3
```

TreeMap을 그냥 print하게 되면 { }로 묶어 Map의 전체 key값, value가 출력된다.  
특정 key값의 value를 가져오고 싶다면 get(key)를 사용하면 된다.  
TreeMap은 HashMap과 달리 Tree구조로 이루어져 있기에 항상 정렬이 되어있어 최소값,최대값을 바로 가져오는 메소드를 지원한다.  
**firstEntry는 최소 Entry값**, **firstKey는 최소 Key값**,   
**lastEntry는 최대 Entry값**, **lastKey는 최대 Key값**을 리턴한다.

---

### **TreeMap 전체 값 출력**

```java
TreeMap<Integer,String> map = new TreeMap<Integer,String>(){{//초기값 설정    
	put(1, "사과");//값 추가    
	put(2, "복숭아");    
	put(3, "수박");
}}; //entrySet() 활용
for (Entry<Integer, String> entry : map.entrySet()) {    
System.out.println("[Key]:" + entry.getKey() + " [Value]:" + entry.getValue());}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:복숭아
//[Key]:3 [Value]:수박 //KeySet() 활용
for(Integer i : map.keySet()){ //저장된 key값 확인    
System.out.println("[Key]:" + i + " [Value]:" + map.get(i));}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:복숭아
//[Key]:3 [Value]:수박
```

TreeMap의 전체요소를 출력하려면 HashMap과 마찬가지로 entrySet()이나 keySet() 메소드를 활용한다.  
   
**특정 key값의 value를 가져오고 싶다면 get(key)를 사용**하면 되고,  
**전체를 출력하려면 entrySet()이나 keySet()메소드를 활용하여 Map의 객체를 반환받은 후 출력**하면 된다.  
   
**entrySet()은 key와 value가 모두 필요할 경우 사용**하며  
**keySet()은 key 값만 필요할 경우 사용**하는데 **key값만 받아서 get(key)를 활용하여 value를 출력**할 수도 있다.  
하지만, key 값을 이용해 value를 찾는 과정에서 시간이 많이 소모되므로 많은 양의 데이터를 가져와야 한다면 entrySet()이 좋다.  
(약 20%~200% 성능 저하가 있다고 한다.)

---

   
### **Iterator 사용**

```java
TreeMap<Integer,String> map = new TreeMap<Integer,String>(){{//초기값 설정    
put(1, "사과");//값 추가    
put(2, "복숭아");    
put(3, "수박");}};		//entrySet().iterator()
Iterator<Entry<Integer, String>> entries = map.entrySet().iterator();
while(entries.hasNext()){    
Map.Entry<Integer, String> entry = entries.next();   
System.out.println("[Key]:" + entry.getKey() + " [Value]:" +  entry.getValue());}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:복숭아
//[Key]:3 [Value]:수박		
//keySet().iterator()
Iterator<Integer> keys = map.keySet().iterator();
while(keys.hasNext()){    
int key = keys.next();    
System.out.println("[Key]:" + key + " [Value]:" +  map.get(key));}
//[Key]:1 [Value]:사과
//[Key]:2 [Value]:복숭아
//[Key]:3 [Value]:수박
```



### Map Class 직접 구현

```java
package Week1;  
  
  
class Nodes<K, V> {  
    private final K key;  
    private V value;  
    private Nodes<K, V> next;  
  
    public Nodes(K key, V value) {  
        this.key = key;  
        this.value = value;  
        this.next = null;  
    }  
  
    public K getKey() {  
        return key;  
    }  
  
    public V getValue() {  
        return value;  
    }  
  
    public void setValue(V value) {  
        this.value = value;  
    }  
  
    public Nodes<K, V> getNext() {  
        return next;  
    }  
  
    public void setNext(Nodes<K, V> next) {  
        this.next = next;  
    }  
}  
  
public class MakeMap<K, V> {  
    private int capacity;  
    private int size;  
    private Nodes<K, V>[] buckets;  
  
  
  
    public MakeMap() {  
        this.capacity = 16; //용량 자동지정  
        this.size = 0;  
        this.buckets = new Nodes[capacity];  
    }  
  
    public MakeMap(int capacity) {  
        this.capacity = capacity;  
        this.size = 0;  
        this.buckets = new Nodes[capacity];  
    }  
    private int getIndex(K key) {  
        int hashCode = key.hashCode();  
        int index = hashCode % capacity;  
        return index;  
    }  
    public void put(K key, V value) {  
        //hashMap으로 키값 지정  
        int index = getIndex(key);  
        Nodes<K, V> node = buckets[index];  
        while (node != null) {//hash code가 같지만 안에 내용이 다를경우가 있기때문에  
            if (node.getKey().equals(key)) {  
                node.setValue(value);  
                return;  
            }  
            node = node.getNext(); //다음 노드와 비교하기위해 다음 노드 주소지를 입력  
        }  
        Nodes<K, V> newNode = new Nodes<>(key, value);  
        newNode.setNext(buckets[index]); //값이 첫번째로 오게 됨 그로 인해 다음 노드를 현재노드의 다음 노드로 설정  
        buckets[index] = newNode;     //지금 인덱스에 새로운 값 지정  
        size++;  
    }  
  
    public V get(K key) {  
        int index = getIndex(key);  
        Nodes<K, V> node = buckets[index];  
        while (node != null) {  
            if (node.getKey().equals(key)) {  
                return node.getValue();  
            }  
            node = node.getNext();  
        }  
        return null;  
    }  
    //hashMap같은 경우 첫번쨰 인덱스에 값이 들어오는 구조  
    public void remove(K key) {  
        int index = getIndex(key);  
        Nodes<K, V> node = buckets[index]; //index에 값이 있는지 확인  
        Nodes<K, V> prev = null;  
        while (node != null) { //값이 있을 경우  
            if (node.getKey().equals(key)) { //그 값이 일치할경우  
                // 삭제할 노드가 첫 번째 노드인 경우  
                if (prev == null) { //이전 노드가 null인지 파악  
                    //이러면 원래 해쉬코드 값이 변하나?  
                    buckets[index] = node.getNext(); //지금 인덱스에 node의 다음 값 대입  
                } else {  
                    //이게 한번이라도 들어오면 이전노드가 지금 노드의 다음 노드 주소값을 받음  
                    prev.setNext(node.getNext()); //이전 노드의 다음 주소값에 지금 노드의 다음 주소값 대입  
                }  
                size--;  
                //일치할 경우 remove를 종료 while이 아닌 메소드 전체가 종료  
                return;  
            }  
            //해쉬코드는 같지만 값이 다를때  
            prev = node; //이전 노드에 지금 노드 대입  
            node = node.getNext(); //노드에 다음 노드 대입  
        }  
    }  
  
    public boolean containsKey(K key) {  
        int index = getIndex(key);  
        Nodes<K, V> node = buckets[index];  
        while (node != null) {  
            if (node.getKey().equals(key)) {  
                return true;  
            }  
            node = node.getNext();  
        }  
        return false;  
    }  
  
    public boolean isEmpty() {  
        return size == 0;  
    }  
  
    public int size() {  
        return size;  
    }  
  
  
}
```

---
## 참조
참조 - https://devlogofchris.tistory.com/41 HashMap 
https://gre-eny.tistory.com/97

HashTable - https://crazykim2.tistory.com/589

TreeMap- https://dev-coco.tistory.com/39







객체 비교 
https://blog.naver.com/travelmaps/220836526277


https://velog.io/@dltlsgh5/javascript%ED%95%B4%EC%8B%9C%EB%A7%B5HashMap-%EA%B5%AC%ED%98%84 js 