### Queue란? 선입선출
Queue의 사전적 의미는 무엇을 기다리는 사람, 차량 등의 줄 혹은 줄을 서서 기다리는 것을 의미하는데 이처럼 줄을 지어 순서대로 처리되는 것이 큐라는 자료구조입니다. 큐는 데이터를 일시적으로 쌓아두기 위한 자료구조로 스택과는 다르게 FIFO(First In First Out)의 형태를 가집니다. FIFO 형태는 뜻 그대로 먼저 들어온 데이터가 가장 먼저 나가는 구조를 말합니다.

**Enqueue :** 큐 맨 뒤에 데이터 추가  
**Dequeue :** 큐 맨 앞쪽의 데이터 삭제


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


```java

```














참조 -   https://coding-factory.tistory.com/602