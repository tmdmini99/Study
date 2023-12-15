iterate : (계산, 컴퓨터 처리 절차를) 반복하다  
iterator : 반복자


자바에서 Iterator는 컬렉션 프레임워크(Collection Framework)에서 값을 가져오거나 삭제할 때 사용하는데 먼저 컬렉션 프레임워크는 List, Set, Map, Queue 등을 말한다


프로그래밍에서 반복기는 개발자가 **컨테이너, 특히 리스트를 순회할 수 있게 해주는 객체**다. 다양한 유형의 반복기는 **종종 컨테이너의 인터페이스를 통해 제공된다.**

iterator는 **ArrayList, HashSet과 같은 컬렉션을 반복하는 데 사용할 수 있는 객체**다. iterator는 반복의 기술 용어기 때문에 반복자라고 한다

#### Iterator가 만들어진 이유
1. Set은 순서가 없는(unordered) 컬렉션이기 때문에 인덱스가 없다...(중략)...iterator는 for-each 반복문이 할 수 없는 일을 할 수 있다.
2. 리스트는 또한 양방향으로 반복할 수 있는 iterator를 제공한다. for-each 반복문은 처음부터 끝까지만 반복된다

#### Iterator의 장점  
1. 컬렉션에서 요소를 제어하는 기능  
2. next() 및 previous()를 써서 앞뒤로 이동하는 기능  
3. hasNext()를 써서 더 많은 요소가 있는지 확인하는 기능


#### Iterator method
|메소드|설명|
|---|---|
|**boolean** hasNext()|해당 이터레이션(iteration)이 다음 요소를 가지고 있으면 true를 반환하고, 더 이상 다음 요소를 가지고 있지 않으면 false를 반환함.|
|**E** next()|이터레이션(iteration)의 다음 요소를 반환함.|
|**default** void remove()|해당 반복자로 반환되는 마지막 요소를 현재 컬렉션에서 제거함. (선택적 기능)|


#### Iterator 예제

```java
public class Main
{
  public class Main
{
    public static void main(String[] args)
    {
        Set<String> cars = new HashSet<>();
        cars.add("벤츠");
        cars.add("람보르기니");
        cars.add("롤스로이스");
        cars.add("페라리");

        // while문을 사용한 경우
        Iterator<String> iterator = cars.iterator();

        while(iterator.hasNext())
        {
            System.out.println("cars : " + iterator.next());
        }

        // for-each문을 사용한 경우
        for (String car : cars)
        {
            System.out.println("cars : " + car);
        }
    }

}
// >> cars : 람보르기니
// >> cars : 롤스로이스
// >> cars : 페라리
// >> cars : 벤츠

}
```

Set을 사용하여 값이 순서대로 출력하는 것이 아닌 랜덤으로 출력한다
\



참조 - https://onlyfor-me-blog.tistory.com/319