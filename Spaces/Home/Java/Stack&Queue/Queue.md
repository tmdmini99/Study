### Queue란? 선입선출
Queue의 사전적 의미는 무엇을 기다리는 사람, 차량 등의 줄 혹은 줄을 서서 기다리는 것을 의미하는데 이처럼 줄을 지어 순서대로 처리되는 것이 큐라는 자료구조입니다. 큐는 데이터를 일시적으로 쌓아두기 위한 자료구조로 스택과는 다르게 FIFO(First In First Out)의 형태를 가집니다. FIFO 형태는 뜻 그대로 먼저 들어온 데이터가 가장 먼저 나가는 구조를 말합니다.

**Enqueue :** 큐 맨 뒤에 데이터 추가  
**Dequeue :** 큐 맨 앞쪽의 데이터 삭제


Java에서는 **java.util.Queue** 큐 클래스를 임포트 하면 Queue를 사용할 수 있습니다.  
큐는 리스트로 구현이 되어 있어서 **java.util.LinkedList** 클래스도 임포트 해줘야 합니다!

### Queue의 특징

**1.** 먼저 들어간 자료가 먼저 나오는 구조 FIFO(First In FIrst Out) 구조   
**2.** 큐는 한 쪽 끝은 프런트(front)로 정하여 삭제 연산만 수행함  
**3.** 다른 한 쪽 끝은 리어(rear)로 정하여 삽입 연산만 수행함  

**4.** 그래프의 넓이 우선 탐색(BFS)에서 사용  
**5.** 컴퓨터 버퍼에서 주로 사용, 마구 입력이 되었으나 처리를 하지 못할 때, 버퍼(큐)를 만들어 대기 시킴



| 메서드                           | 설명                                                                                                                                           |
|:-------------------------------- |:---------------------------------------------------------------------------------------------------------------------------------------------- |
| queue.add()                      | 데이터 추가       반환 값(boolean): 삽입 성공 시 true / 실패 시  Exception발생                                                                 |
| queue.offer()                    | 데이터 추가               반환 값(boolean): 삽입 성공 시 true / 실패 시 false 반환                                                             |
| queue.poll()                     | 데이터 삭제    queue에 첫번째 값을 반환하고 제거 비어있다면 null                                                                               |
| queue.remove()                   | 데이터 삭제     queue에 첫번째 값 제거     반환 값(삭제된 value의 자료형): 삭제된 value / 공백 큐이면 Exception("NoSuchElementException") 발생 |
| queue.clear()                    | 데이터 모든값 삭제 queue 초기화    반환값 void                                                                                                 |
| queue.peek()                     | 데이터 첫번째 값 가져옴            queue의 첫번째 값 참조    반환값 큐 헤드에 위치한 value/공백이면 null                                       |
| queue.size()                     | queue 사이즈 크기        반환값 int                                                                                                            |
| queue.contains("찾고싶은 value") | 찾고 싶음 value값이 있는지 확인 있을경우 true/ 없으면 false 반환값 boolean                                                                     |
| queue.isEmpty();                 | 공백이면 true / 값이 있으면 false 반환값 boolean                                                                                               |
| queue.element();                 | 큐의 첫번째 값 반환 공백큐이면 Exception("NoSuchElementException")발생 반환값 큐 헤드에 위치한 value                                           |
| **queue.remove(삭제할 value);**  | 반환 값(boolean): 큐에 해당 value가 존재하면 해당 값 삭제 후 true / 존재하지 않으면 false 반환                                                                                                                                               |

### Queue 선언

```java
import java.util.LinkedList; //import

import java.util.Queue; //import

Queue<Integer> queue = new LinkedList<>(); //int형 queue 선언, linkedlist 이용

Queue<String> queue = new LinkedList<>(); //String형 queue 선언, linkedlist 이용
```

자바에서 큐는 LinkedList를 활용하여 생성해야 합니다. 그렇기에 Queue와 LinkedList가 다 import되어 있어야 사용이 가능합니다. Queue Element queue = new LinkedList<>()와 같이 선언해주면 됩니다.

### Queue 값 추가

```java
CopyQueue<Integer> stack = new LinkedList<>(); //int형 queue 선언
queue.add(1);     // queue에 값 1 추가
queue.add(2);     // queue에 값 2 추가
queue.offer(3);   // queue에 값 3 추가
```

자바에서 queue에 값을 추가하고 싶다면 add(value) 또는 offer(value)라는 메서드를 활용하면 됩니다. add(value) 메소드의 경우 만약 삽입에 성공하면 true를 반환하고, 큐에 여유 공간이 없어 삽입에 실패하면 IllegalStateException을 발생시킵니다. queue에 값을 계속해서 추가해나간다면 아래 그림과 같은 형태로 데이터가 쌓이게 됩니다.


### Queue 값 삭제

```java
CopyQueue<Integer> queue = new LinkedList<>(); //int형 queue 선언
queue.offer(1);     // queue에 값 1 추가
queue.offer(2);     // queue에 값 2 추가
queue.offer(3);     // queue에 값 3 추가
queue.poll();       // queue에 첫번째 값을 반환하고 제거 비어있다면 null
queue.remove();     // queue에 첫번째 값 제거
queue.clear();      // queue 초기화
```

queue에서 값을 제거하고싶다면 poll()이나 remove라는 메서드를 사용하면 됩니다. poll()함수는 큐가 비어있으면 null을 반환합니다. pop을 하면 가장 앞쪽에 있는 원소의 값이 아래 그림과 같이 제거됩니다. queue의 모든 요소를 제거하려면 clear()메서드를 사용합니다.


### Queue 에서 가장 먼저 들어간 값 출력

```java
CopyQueue<Integer> queue = new LinkedList<>(); //int형 queue 선언
queue.offer(1);     // queue에 값 1 추가
queue.offer(2);     // queue에 값 2 추가
queue.offer(3);     // queue에 값 3 추가
queue.peek();       // queue의 첫번째 값 참조
```

Queue에서 첫번째로 저장된 값을 참조하고 싶다면 peek()라는 메서드를 사용하면 됩니다








참조 -   https://coding-factory.tistory.com/602