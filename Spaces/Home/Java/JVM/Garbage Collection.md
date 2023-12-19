
## **1. Garbage Collecter(가비지 컬렉터)이란?**
- 가비지 컬렉터 : 메모리 관리를 담당하는 시스템 또는 프로그램의 구성 요소이며, 메모리에서 더 이상 사용되지 않는 객체를 찾아 제거하여 메모리를 회수하는 역할을 수행한다.
- 가비지 컬렉션 : 메모리 관리 기술 중 하나로, 가비지 컬렉터에 의해 수행되는 프로세스를 의미.
가비지 컬렉션은 프로세스 자체를 얘기하고 컬렉터는 실제 역할을 수행하는 주체를 얘기한다
---

###  **Garbage Collection(가비지 컬렉션)이란?**  

프로그램을 개발 하다 보면 유효하지 않은 메모리인 가바지(Garbage)가 발생하게 된다. C언어를 이용하면 free()라는 함수를 통해 직접 메모리를 해제해주어야 한다. 하지만 Java나 Kotlin을 이용해 개발을 하다 보면 개발자가 메모리를 직접 해제해주는 일이 없다. 그 이유는 JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리해주기 때문이다. 대신 Java에서 명시적으로 불필요한 데이터를 표현하기 위해서 일반적으로 null을 선언해준다. 예를 들어 아래와 같은 코드가 있다고 가정하자.

```
Person person = new Person();
person.setName("Mang");
person = null;

// 가비지 발생
person = new Person();
person.setName("MangKyu");
```

기존의 Mang으로 생성된 person 객체는 더이상 참조를 하지 않고 사용이 되지 않아서 Garbage(가비지)가 되었다. Java나 Kotlin에서는 이러한 메모리 누수를 방지하기 위해 가비지 컬렉터(Garbage Collector, GC)가 주기적으로 검사하여 메모리를 청소해준다.

(물론 Java에서도 System.gc()를 이용해 호출할 수 있지만, 해당 메소드를 호출하는 것은 시스템의 성능에 매우 큰 영향을 미치므로 절대 호출해서는 안된다.)

### **[ Minor GC와 Major GC ]**

JVM의 Heap영역은 처음 설계될 때 다음의 2가지를 전제(Weak Generational Hypothesis)로 설계되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성되며, 메모리에 오랫동안 남아있는 경우는 드물다는 것이다. 그렇기 때문에 객체의 생존 기간에 따라 물리적인 Heap 영역을 나누게 되었고 Young, Old 총 2가지 영역으로 설계되었다. 초기에는 Perm 영역이 존재하였지만 Java8부터 제거되었다.


#### Perm 영역은 왜 사라졌을까?
![[Pasted image 20231219164401.png]]
#### Metasapace 영역이란?

- Perm 영역에서 저장하던 Class의 Meta 정보들이 이 영역에 저장된다.
- Native Memory 영역에 위치하며, JVM이 아닌 OS 레벨에서 관리된다.
- 클래스 메타데이터와 리플렉션을 사용하는 애플리케이션에서 사용하는 일부 메모리를 저장한다.

#### 대체된 이유는?

- Perm 영역의 메모리 누수, OutOfMemoryError 등과 같은 문제
    - 클래스 로딩 및 언로딩 과정에서 메모리 할당 및 해제의 빈번한 발생으로 인해 이러한 문제가 더 심각해졌다.
- 클래스 메타데이터를 Native Memory에 저장하면서,  
    JVM에서의 OutOfMemoryError 문제가 해결되었다.



![[Pasted image 20231219100858.png]]


![](https://blog.kakaocdn.net/dn/va8qQ/btqUSpSocbS/kxTvtnmrdhf4bnVPXth0UK/img.png)

GC 영역 및 흐름

-  Young 영역(Young Generation)
    
    - 새롭게 생성된 객체가 할당(Allocation)되는 영역
    - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
    - Young 영역에 대한 가비지 컬렉션(Garbage Collection)을 Minor GC라고 부른다.
    
- Old 영역(Old Generation)
    
    - Young영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
    - Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 적게 발생한다. 
    - Old 영역에 대한 가비지 컬렉션(Garbage Collection)을 Major GC라고 부른다.
    

Old 영역이 Young 영역보다 크게 할당되는 이유는 Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않으며 큰 객체들은 Young 영역이 아니라 바로 Old 영역에 할당되기 때문이다.

또 다시 힙 영역은 더욱 효율적인 GC를 위해 **Young 영역**을 **3가지 영역(Eden, survivor 0, survivor 1)** 으로 나눈다.


![[Pasted image 20231219101425.png]]


#### **Eden** 

- new를 통해 새로 생성된 객체가 위치. 
- 정기적인 쓰레기 수집 후 살아남은 객체들은 Survivor 영역으로 보냄

#### **Survivor 0 / Survivor 1** 

- 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역
- Survivor 영역에는 특별한 규칙이 있는데, Survivor 0 또는 Survivor 1 둘 중 하나에는 꼭 비어 있어야 하는 것이다.

이렇게 하나의 힙 영역을 세부적으로 쪼갬으로서 객체의 생존 기간을 면밀하게 제어하여 가비지 컬렉터(GC)를 보다 정확하게 불필요한 객체를 제거하는 프로세스를 실행하도록 한다.


예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 것이다. 이러한 경우를 대비하여 Old 영역에는 512 bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.



![](https://blog.kakaocdn.net/dn/FOLU3/btqUOBF35cJ/BMKuD1iqfq6R0lAqMlfkC0/img.png)

카드 테이블에는 Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때 마다 그에 대한 정보가 표시된다. 카드 테이블이 도입된 이유는 간단한다. Young 영역에서 가비지 컬렉션(Minor GC)가 실행될 때 모든 Old 영역에 존재하는 객체를 검사하여 참조되지 않는 Young 영역의 객체를 식별하는 것이 비효율적이기 때문이다. 그렇기 때문에 Young 영역에서 가비지 컬렉션이 진행될 때 카드 테이블만 조회하여 GC의 대상인지 식별할 수 있도록 하고 있다.

## **2.  Garbage Collection(가비지 컬렉션)의 동작 방식**

---

### ****[ Garbage Collection(가비지 컬렉션)의 동작 방식 ]****

Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에, 세부적인 동작 방식은 다르다. 하지만 기본적으로 가비지 컬렉션이 실행된다고 하면 다음의 2가지 공통적인 단계를 따르게 된다.

1. Stop The World
2. Mark and Sweep

#### **1. Stop The World**

Stop The World는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC가 실행될 때는 GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 줄이는 작업을 하는 것이다. 또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.

#### **2. Mark and Sweep**

- Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 사용되고 있는 메모리를 식별하는데, 이러한 과정을 Mark라고 한다. 이후에 Mark가 되지 않은 객체들을 메모리에서 제거하는데, 이러한 과정을 Sweep라고 한다.
Mark라는 단어의 뜻은 표시하다 라는 뜻이 있다.

Sweep은 쓸어내리다, 소멸의 뜻이 있다고 한다.

![[Pasted image 20231219163520.png]]

GC Root는 실행중인 스레드, 정적 변수, 로컬 변수, JNI 레퍼런스와 같은 것들이 될 수 있다.

위에서 나왔던 root set과 동일한 용어이다.

![](https://velog.velcdn.com/images/yarogono/post/fb954b1e-77d7-4d49-b45f-5d2a45b78220/image.png)

GC는 객체에 Mark를 하고 Mark가 되지 않은 객체의 메모리를 해제하게 된다.

결국에 Mark가 안된 객체는 Unreachable 객체이다.

이미 Reachability에서 다뤘던 내용이기 때문에 쉽게 이해할 수 있었다.

더 자세한 내용이 궁금하면 아래의 블로그를 참고해보자

- 가비지 컬렉터(Garbage Collector)와 Mark & Sweep

### **[ Minor GC의 동작 방식 ]**


Minor GC를 정확히 이해하기 위해서는 Young 영역의 구조에 대해 이해를 해야 한다. Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역, 총 3가지로 나뉘어진다.

- Eden 영역: 새로 생성된 객체가 할당(Allocation)되는 영역
- Survivor 영역: 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역

객체가 새롭게 생성되면 Young 영역 중에서도 Eden 영역에 할당(Allocation)이 된다. 그리고 Eden 영역이 꽉 차면 Minor GC가 발생하게 되는데, 사용되지 않는 메모리는 해제되고 Eden 영역에 존재하는 객체는 (사용중인) Survivor 영역으로 옮겨지게 된다. Survivor 영역은 총 2개이지만 반드시 1개의 영역에만 데이터가 존재해야 하는데, Young 영역의 동작 순서를 자세히 살펴보도록 하자.

**1.** 처음 생성된 객체는 Young Generation 영역의 일부인 Eden 영역에 위치

[![java-Minor GC](https://blog.kakaocdn.net/dn/cXg3ZZ/btrITpQET0n/aJSAYDEziQIKlVCvxaSpg0/img.png)](https://blog.kakaocdn.net/dn/cXg3ZZ/btrITpQET0n/aJSAYDEziQIKlVCvxaSpg0/img.png)

[![java-Minor GC](https://blog.kakaocdn.net/dn/dnDTuC/btrISOQKFwn/ullhkIEtDiPY7HUahMKvW1/img.png)](https://blog.kakaocdn.net/dn/dnDTuC/btrISOQKFwn/ullhkIEtDiPY7HUahMKvW1/img.png)

**2.** 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행

[![java-Minor GC](https://blog.kakaocdn.net/dn/tySvx/btrITpwlB0D/JtcEmTbIYaPnHAAoNU8vB0/img.png)](https://blog.kakaocdn.net/dn/tySvx/btrITpwlB0D/JtcEmTbIYaPnHAAoNU8vB0/img.png)

**3.** Mark 동작을 통해 reachable 객체를 탐색

[![java-Minor GC](https://blog.kakaocdn.net/dn/cJl67l/btrIXRd6Izv/ZwTbTqbKkuaH71Llqzog6k/img.png)](https://blog.kakaocdn.net/dn/cJl67l/btrIXRd6Izv/ZwTbTqbKkuaH71Llqzog6k/img.png)

**4.** Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동

[![java-Minor GC](https://blog.kakaocdn.net/dn/bxY6Md/btrIWNwtetB/4EpRJFGM5v7Fwk9dk0Wc9K/img.png)](https://blog.kakaocdn.net/dn/bxY6Md/btrIWNwtetB/4EpRJFGM5v7Fwk9dk0Wc9K/img.png)

**5.** Eden 영역에서 사용되지 않는 객체(unreachable)의 메모리를 해제(sweep)

[![java-Minor GC](https://blog.kakaocdn.net/dn/ssSMf/btrIWMYFQlZ/KqHBu6rr18ANKo1ngOnMQk/img.png)](https://blog.kakaocdn.net/dn/ssSMf/btrIWMYFQlZ/KqHBu6rr18ANKo1ngOnMQk/img.png)

**6.** 살아남은 모든 객체들은 age값이 1씩 증가

[![java-Minor GC](https://blog.kakaocdn.net/dn/Dno6M/btrIPYfc2VC/BLLkKAXLVRYKUAu3R6Vefk/img.png)](https://blog.kakaocdn.net/dn/Dno6M/btrIPYfc2VC/BLLkKAXLVRYKUAu3R6Vefk/img.png)
**7.** 또다시 Eden 영역에 신규 객체들로 가득 차게 되면 다시한번 minor GC 발생하고 mark 한다

[![java-Minor GC](https://blog.kakaocdn.net/dn/cNoD2j/btrIT8gQrk9/vIqyTQJvZ16ByIGplen2D0/img.png)](https://blog.kakaocdn.net/dn/cNoD2j/btrIT8gQrk9/vIqyTQJvZ16ByIGplen2D0/img.png)

[![java-Minor GC](https://blog.kakaocdn.net/dn/8XX01/btrIPYsHxbi/FothJXlh95Tc6DSYSkZb10/img.png)](https://blog.kakaocdn.net/dn/8XX01/btrIPYsHxbi/FothJXlh95Tc6DSYSkZb10/img.png)

[![java-Minor GC](https://blog.kakaocdn.net/dn/cRhJ1g/btrIXPtPw5o/gN8t1V7BjNXaRqPjKueVX1/img.png)](https://blog.kakaocdn.net/dn/cRhJ1g/btrIXPtPw5o/gN8t1V7BjNXaRqPjKueVX1/img.png)

**8.** marking 한 객체들을 비어있는 Survival 1으로 이동하고 sweep

[![java-Minor GC](https://blog.kakaocdn.net/dn/dsj6Og/btrIRHqHljJ/00JVcQ3KD1E8he6qejuLik/img.png)](https://blog.kakaocdn.net/dn/dsj6Og/btrIRHqHljJ/00JVcQ3KD1E8he6qejuLik/img.png)

[![java-Minor GC](https://blog.kakaocdn.net/dn/blilhe/btrISO4eRF3/sMVxubaUB19Sm1XsVJE90k/img.png)](https://blog.kakaocdn.net/dn/blilhe/btrISO4eRF3/sMVxubaUB19Sm1XsVJE90k/img.png)

**10.** 다시 살아남은 모든 객체들은 age가 1씩 증가

[![java-Minor GC](https://blog.kakaocdn.net/dn/d8v4Vk/btrIXwVy4Uj/iGv5H94KNcZhQ0GufntWg0/img.png)](https://blog.kakaocdn.net/dn/d8v4Vk/btrIXwVy4Uj/iGv5H94KNcZhQ0GufntWg0/img.png)

**11.** 이러한 과정을 반복

[![java-Minor GC](https://blog.kakaocdn.net/dn/2WHEq/btrIUaeDqLG/qN0f490z0nGddd0Vl7ECdk/img.gif)](https://blog.kakaocdn.net/dn/2WHEq/btrIUaeDqLG/qN0f490z0nGddd0Vl7ECdk/img.gif)


1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행된다.
    
    1. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
    2. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
    
3. 1~2번의 과정이 반복되다가 Survivor 영역이 가득 차게 되면 Survivor 영역의 살아남은 객체를 다른 Survivor 영역으로 이동시킨다.(1개의 Survivor 영역은 반드시 빈 상태가 된다.)
4. 이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동(Promotion)된다.

객체의 생존 횟수를 카운트하기 위해 Minor GC에서 객체가 살아남은 횟수를 의미하는 age를 Object Header에 기록한다. 그리고 Minor GC 때 Object Header에 기록된 age를 보고 Promotion 여부를 결정한다.

또한 Survivor 영역 중 1개는 반드시 사용이 되어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아님을 파악할 수 있다.

이러한 진행 과정을 그림으로 살펴보면 다음과 같다.

![](https://blog.kakaocdn.net/dn/Cyho2/btqURvZRql6/4a7u6mMGofkpuURKQz0RT1/img.png)

HotSpot JVM에서는 Eden 영역에 객체를 빠르게 할당(Allocation)하기 위해 bump the pointer와 TLABs(Thread-Local Allocation Buffers)라는 기술을 사용하고 있다. bump the pointer란 Eden 영역에 마지막으로 할당된 객체의 주소를 캐싱해두는 것이다. bump the pointer를 통해 새로운 객체를 위해 유효한 메모리를 탐색할 필요 없이 마지막 주소의 다음 주소를 사용하게 함으로써 속도를 높이고 있다. 이를 통해 새로운 객체를 할당할 때 객체의 크기가 Eden 영역에 적합한지만 판별하면 되므로 빠르게 메모리 할당을 할 수 있다.

싱글 쓰레드 환경이라면 문제가 없겠지만 멀티쓰레드 환경이라면 객체를 Eden 영역에 할당할 때 락(Lock)을 걸어 동기화를 해주어야 한다. 멀티 쓰레드 환경에서의 성능 문제를 해결하기 위해 HotSpot JVM은 추가로 TLABs(Thread-Local Allocation Buffers)라는 기술을 도입하게 되었다. TLABs(Thread-Local Allocation Buffers)란 각각의 쓰레드마다 Eden 영역에 객체를 할당하기 위한 주소를 부여함으로써 동기화 작업 없이 빠르게 메모리를 할당하도록 하는 기술이다. 각각의 쓰레드는 자신이 갖는 주소에만 객체를 할당함으로써 동기화 없이 bump the poitner를 통해 빠르게 객체를 할당하도록 하고 있다.

### **[ Major GC의 동작 방식 ]**

Young 영역에서 오래 살아남은 객체는 Old 영역으로 Promotion됨을 확인할 수 있었다. 그리고 Major GC는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생하게 된다. Young 영역은 일반적으로 Old 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다. 하지만 Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수도 있다. 그렇기 때문에 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다. 참고로 Young 영역과 Old 영역을 동시하 처리하는 GC는 Full GC라고 한다.
![[Pasted image 20231219103743.png]]
Old Generation은 길게 살아남는 메모리들이 존재하는 공간이다.

Old Generation의 객체들은 거슬러 올라가면 처음에는 Young Generation에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 age 임계값이 차게되어 이동된 녀석들이다.

그리고 **Ma** **jor GC**는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생하게 된다.
> Tip
> Major GC는 Full GC라고도 불리운다

Minor GC와 Major GC 차이점을 표로 정리하면 다음과 같이 된다.

[![java-Major GC](https://blog.kakaocdn.net/dn/bQinW1/btrIOSeHYMd/rd1iSnwNConXoRVWiDvDS0/img.png)](https://blog.kakaocdn.net/dn/bQinW1/btrIOSeHYMd/rd1iSnwNConXoRVWiDvDS0/img.png)

---

**1.** 객체의 age가 임계값(여기선 8로 설정)에 도달하게 되면,

[![java-Major GC](https://blog.kakaocdn.net/dn/c3ZKIh/btrITnE39P2/cL0xncLtqvQxqOmxmyt8V0/img.png)](https://blog.kakaocdn.net/dn/c3ZKIh/btrITnE39P2/cL0xncLtqvQxqOmxmyt8V0/img.png)

**2.** 이 객체들은 Old Generation 으로 이동된다. 이를 promotion 이라 부른다.

[![java-Major GC](https://blog.kakaocdn.net/dn/GJGPb/btrIUmFRmQO/SZDmkA2vbBcqKcXrNiIhOk/img.png)](https://blog.kakaocdn.net/dn/GJGPb/btrIUmFRmQO/SZDmkA2vbBcqKcXrNiIhOk/img.png)

**3.** 위의 과정이 반복되어 Old Generation 영역의 공간(메모리)가 부족하게 되면 Major GC가 발생되게 된다.

[![java-Major GC](https://blog.kakaocdn.net/dn/mnBRi/btrIUm6WXIl/obIsxDpns6wEkkKfjdvLCK/img.gif)](https://blog.kakaocdn.net/dn/mnBRi/btrIUm6WXIl/obIsxDpns6wEkkKfjdvLCK/img.gif)

**Major GC**는 Old 영역은 데이터가 가득 차면 GC를 실행하는 단순한 방식이다. 

Old 영역에 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 Major GC가 실행되게 된다.

하지만 Old Generation은 Young Generation에 비해 상대적으로 큰 공간을 가지고 있어, 이 공간에서 메모리 상의 객체 제거에 많은 시간이 걸리게 된다.

예를들어 Young 영역은 일반적으로 Old 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다.

그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다.

하지만 Old 영역의 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다.

바로 여기서 본문 초반에 소개했던 **Stop-The-World** 문제가 발생하게 된다.

Major GC가 일어나면 Thread가 멈추고 Mark and Sweep 작업을 해야 해서 CPU에 부하를 주기 때문에 멈추거나 버벅이는 현상이 일어나기 때문이다.

따라서 자바 개발진들은 끊임 없이 **가비지 컬렉션 알고리즘을 발전** 시켜왔다.



## **3. Garbage Collection(가비지 컬렉션) 내용 요약**

---

### **[ Garbage Collection(가비지 컬렉션) 내용 요약 ]**

![](https://blog.kakaocdn.net/dn/dM4wqf/btqUWs2lW8H/GvRECmsUIfZ2jhDoKhSCD0/img.png)




## **1. 다양한 Garbage Collection(가비지 컬렉션) 알고리즘**

---

JVM이 메모리를 자동으로 관리해주는 것은 개발자의 입장에서 상당한 메리트이다. 하지만 문제는 GC를 수행하기 위해 Stop The World에 의해 애플리케이션이 중지되는 것에 있다. Heap의 사이즈가 커지면서 애플리케이션의 지연(Suspend) 현상이 두드러지게 되었고, 이를 막기 위해 다양한 Garbage Collection(가비지 컬렉션) 알고리즘을 지원하고 있다. 

### **[ Serial GC ]**

Serial GC의 Young 영역은 앞서 설명한 알고리즘(Mark Sweep)대로 수행된다. 하지만 Old 영역에서는 Mark Sweep Compact 알고리즘이 사용되는데, 기존의 Mark Sweep에 Compact라는 작업이 추가되었다. Compact는 Heap 영역을 정리하기 위한 단계로 유요한 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 존재하지 않는 부분으로 나누는 것이다.

#### **Serial GC 실행 명령어**

- 자바 프로그램을 실행할때 -XX:+UseSerialGC GC 옵션을 지정하여 해당 가비지 컬렉션 알고리즘으로 힙 메모리를 관리하도록 실행할 수 있다.

```java
java -XX:+UseSerialGC -jar Application.java
```


Serial GC는 서버의 CPU 코어가 1개일 때 사용하기 위해 개발되었으며, 모든 가비지 컬렉션 일을 처리하기 위해 1개의 쓰레드만을 이용한다. 그렇기 때문에 CPU의 코어가 여러 개인 운영 서버에서 Serial GC를 사용하는 것은 반드시 피해야 한다.
- 서버의 CPU 코어가 1개일 때 사용하기 위해 개발된 가장 단순한 GC
- GC를 처리하는 쓰레드가 1개 (싱글 쓰레드) 이어서 가장 stop-the-world 시간이 길다
- Minor GC 에는 Mark-Sweep을 사용하고, Major GC에는 Mark-Sweep-Compact를 사용한다.
-  보통 실무에서 사용하는 경우는 없다 (디바이스 성능이 안좋아서 CPU 코어가 1개인 경우에만 사용) 
![[Pasted image 20231219103939.png]]




### **[ Parallel GC ]**

Parallel GC는 Throughput GC로도 알려져 있으며, 기본적인 처리 과정은 Serial GC와 동일하다. 하지만 Parallel GC는 여러 개의 쓰레드를 통해 Parallel하게 GC를 수행함으로써 GC의 오버헤드를 상당히 줄여준다. Parallel GC는 멀티 프로세서 또는 멀티 쓰레드 머신에서 중간 규모부터 대규모의 데이터를 처리하는 애플리케이션을 위해 고안되었으며, 옵션을 통해 애플리케이션의 최대 지연 시간 또는 GC를 수행할 쓰레드의 갯수 등을 설정해줄 수 있다.

```
java -XX:+UseParallelGC -jar Application.java

// 사용할 쓰레드의 갯수
-XX:ParallelGCThreads=<N>

// 최대 지연 시간
-XX:MaxGCPauseMillis=<N>
```

Parallel GC가 GC의 오버헤드를 상당히 줄여주었고, Java8까지 기본 가비지 컬렉터(Default Garbage Collector)로 사용되었다. 그럼에도 불구하고 Application이 멈추는 것은 피할 수 없었고, 이러한 부분을 개선하기 위해 다른 알고리즘이 더 등장하게 되었다.

### **[ Parallel Old GC ]**

Parallel Old GC는 JDK5 update6부터 제공한 GC이며, 앞서 설명한 Parallel GC와 Old 영역의 GC 알고리즘만 다르다. Parallel Old GC에서는 Mark Sweep Compact가 아닌 Mark Summary Compaction이 사용되는데, Summary 단계에서는 앞서 GC를 수행한 영역에 대해서 별도로 살아있는 객체를 색별한다는 점에서 다르며 조금 더 복잡하다.

- Java 8의 디폴트 GC
- Serial GC와 기본적인 알고리즘은 같지만, Young 영역의 Minor GC를 멀티 쓰레드로 수행 (Old 영역은 여전히 싱글 쓰레드)
- Serial GC에 비해 stop-the-world 시간 감소

![[Pasted image 20231219104049.png]]

#### **Parallel GC 실행 명령어**

- GC 스레드는 기본적으로 cpu 개수만큼 할당된다.
- 옵션을 통해 GC를 수행할 쓰레드의 갯수 등을 설정해줄 수 있다.

```bash
java -XX:+UseParallelGC -jar Application.java 
# -XX:ParallelGCThreads=N : 사용할 쓰레드의 갯수
```


### **Parallel Old GC (Parallel Compacting Collector)**

- Parallel GC를 개선한 버전
- Young 영역 뿐만 아니라, Old 영역에서도 멀티 쓰레드로 GC 수행
- 새로운 가비지 컬렉션 청소 방식인 Mark-Summary-Compact 방식을 이용 (Old 영역도 멀티 쓰레드로 처리)


![[Pasted image 20231219104223.png]]


#### **Parallel Old GC 실행 명령어**



```bash
java -XX:+UseParallelOldGC -jar Application.java
# -XX:ParallelGCThreads=N : 사용할 쓰레드의 갯수
```



### **[ CMS(Concurrent Mark Sweep) GC ]**

CMS(Concurrent Mark Sweep) GC는 Parallel GC와 마찬가지로 여러 개의 쓰레드를 이용한다. 하지만 기존의 Serial GC나 Parallel GC와는 다르게 Mark Sweep 알고리즘을 Concurrent하게 수행하게 된다.

![](https://blog.kakaocdn.net/dn/exiT4S/btqURuUWjwY/Wsh133bXvARXfGMunpvSh1/img.png)

이러한 CMS GC는 애플리케이션의 지연 시간을 최소화 하기 위해 고안되었으며, 애플리케이션이 구동중일 때 프로세서의 자원을 공유하여 이용가능해야 한다. CMS GC가 수행될 때에는 자원이 GC를 위해서도 사용되므로 응답이 느려질 순 있지만 응답이 멈추지는 않게 된다.

하지만 이러한 CMS GC는 다른 GC 방식보다 메모리와 CPU를 더 많이 필요로 하며, Compaction 단계를 수행하지 않는다는 단점이 있다. 이 때문에 시스템이 장기적으로 운영되다가 조각난 메모리들이 많아 Compaction 단계가 수행되면 오히려 Stop The World 시간이 길어지는 문제가 발생할 수 있다.

#### **CMS GC 실행 명령어**
```
// deprecated in java9 and finally dropped in java14
java -XX:+UseConcMarkSweepGC -jar Application.java
```

만약 GC가 수행되면서 98% 이상의 시간이 CMS GC에 소요되고, 2% 이하의 시간이 Heap의 정리에 사영된다면 CMS GC에 의해 OutOfMemoryError가 던져질 것이다. 물론 이를 disable 하는 옵션이 있지만, CMS GC는 Java9 버젼부터 deprecated 되었고 결국 Java14에서는 사용이 중지되었기 때문에 굳이 알아볼 필요는 없을 것 같다.
- 어플리케이션의 쓰레드와 GC 쓰레드가 동시에 실행되어 stop-the-world 시간을 최대한 줄이기 위해 고안된 GC
- 단, GC 과정이 매우 복잡해짐.
- GC 대상을 파악하는 과정이 복잡한 여러단계로 수행되기 때문에 다른 GC 대비 CPU 사용량이 높다
- 메모리 파편화 문제
- CMS GC는 Java9 버젼부터 deprecated 되었고 결국 Java14에서는 사용이 중지



### **[ G1(Garbage First) GC ]**

G1(Garbage First) GC는 장기적으로 많은 문제를 일으킬 수 있는 CMS GC를 대체하기 위해 개발되었고, Java7부터 지원되기 시작하였다.

기존의 GC 알고리즘에서는 Heap 영역을 물리적으로 Young 영역(Eden 영역과 2개의 Survivor 영역)과 Old 영역으로 나누어 사용하였다. G1 GC는 Eden 영역에 할당하고, Survivor로 카피하는 등의 과정을 사용하지만 물리적으로 메모리 공간을 나누지 않는다. 대신 Region(지역)이라는 개념을 새로 도입하여 Heap을 균등하게 여러 개의 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분하여(Eden 지역인지, Survivor 지역인지, Old 지역인지) 객체를 할당한다.

![](https://blog.kakaocdn.net/dn/dHxPiT/btqU0xWGaDI/wriFcFKPHND5pTAsyn47X1/img.png)

G1 GC에서는 Eden, Survivor, Old 역할에 더해 Humonogous와 Availabe/Unused라는 2가지 역할을 추가하였다. Humonguous는 Region 크기의 50%를 초과하는 객체를 저장하는 Region을 의미하며, Availabe/Unused는 사용되지 않은 Region을 의미한다. 

G1 GC의 핵심은 Heap을 동일한 크기의 Region으로 나누고, 가비지가 많은 Region에 대해 우선적으로 GC를 수행하는 것이다. 그리고 G1 GC도 다른 가비지 컬렉션과 마찬가지로 2가지 GC(Minor GC, Major GC)로 나누어 수행되는데, 각각에 대해 살펴보도록 하자.

#### **1. Minor GC**

한 지역에 객체를 할당하다가 해당 지역이 꽉 차면 다른 지역에 객체를 할당하고, Minor GC가 실행된다. G1 GC는 각 지역을 추적하고 있기 때문에, 가비지가 가장 많은(Garbage First) 지역을 찾아서 Mark and Sweep를 수행한다.

Eden 지역에서 GC가 수행되면 살아남은 객체를 식별(Mark)하고, 메모리를 회수(Sweep)한다. 그리고 살아남은 객체를 다른 지역으로 이동시키게 된다. 복제되는 지역이 Available/Unused 지역이면 해당 지역은 이제 Survivor 영역이 되고, Eden 영역은 Available/Unused 지역이 된다.

#### **2. Major GC(Full GC)**

시스템이 계속 운영되다가 객체가 너무 많아 빠르게 메모리를 회수 할 수 없을 때 Major GC(Full GC)가 실행된다. 그리고 여기서 G1 GC와 다른 GC의 차이점이 두각을 보인다.

기존의 다른 GC 알고리즘은 모든 Heap의 영역에서 GC가 수행되었으며, 그에 따라 처리 시간이 상당히 오래 걸렸다. 하지만 G1 GC는 어느 영역에 가비지가 많은지를 알고 있기 때문에 GC를 수행할 지역을 조합하여 해당 지역에 대해서만 GC를 수행한다. 그리고 이러한 작업은 Concurrent하게 수행되기 때문에 애플리케이션의 지연도 최소화할 수 있는 것이다.

(G1 GC의 내용을 세부적으로 파고 들면 설명해야 할 것들이 많지만, 해당 포스팅에서는 다양한 GC 알고리즘에 대해 알아보는 것이 핵심이므로 세부적인 설명은 다른 포스팅을 통해 진행하고자 한다.)

물론 G1 GC는 다른 GC 방식에 비해 잦게 호출될 것이다. 하지만 작은 규모의 메모리 정리 작업이고 Concurrent하게 수행되기 때문이 지연이 크지 않으며, 가비지가 많은 지역에 대해 정리를 하므로 훨씬 효율적이다.


#### **G1 GC 실행 명령어**
```
java -XX:+UseG1GC -jar Application.java
```

이러한 구조의 G1 GC는 당연히 앞의 어떠한 GC 방식보다 처리 속도가 빠르며 큰 메모리 공간에서 멀티 프로레스 기반으로 운영되는 애플리케이션을 위해 고안되었다. 또한 G1 GC는 다른 GC 방식의 처리속도를 능가하기 때문에 Java9부터 기본 가비지 컬렉터(Default Garbage Collector)로 사용되게 되었다.

- CMS GC를 대체하기 위해 jdk 7 버전에서 최초로 release된 GC
- Java 9+ 버전의 디폴트 GC로 지정
- 4GB 이상의 힙 메모리, Stop the World 시간이 0.5초 정도 필요한 상황에 사용 (Heap이 너무작을경우 미사용 권장)
- 기존의 GC 알고리즘에서는 Heap 영역을 물리적으로 고정된 Young / Old 영역으로 나누어 사용하였지만,   
    G1 gc는 아예 이러한 개념을 뒤엎는 Region이라는 개념을 새로 도입하여 사용.  
    전체 Heap 영역을 Region이라는 영역으로 체스같이 분할하여 상황에 따라 Eden, Survivor, Old 등 역할을 고정이 아닌 동적으로 부여
- Garbage로 가득찬 영역을 빠르게 회수하여 빈 공간을 확보하므로, 결국 GC 빈도가 줄어드는 효과를 얻게 되는 원리


![[Pasted image 20231219104351.png]] ![[Pasted image 20231219104404.png]]

**[ G1 GC의 효율성 ]**  
Java9+ 부터 기본 GC로 자리잡은 G1 GC에서는 이전의 GC들처럼 일일히 메모리를 탐색해 객체들을 제거하지 않는다.   
대신 메모리가 많이 차있는 영역(region)을 인식하는 기능을 통해 메모리가 많이 차있는 영역을 우선적으로 GC 한다.   
즉, G1 GC는 Heap Memory 전체를 탐색하는 것이 아닌 영역(region)을 나눠 탐색하고 영역(region)별로 GC가 일어난다.  
  
또한 이전의 GC 들은 Young Generation에 있는 객체들이 GC가 돌때마다 살아남으면 Eden → Survivor0 → Survivor1으로 순차적으로 이동했지만, G1 GC에서는 순차적으로 이동하지는 않는다.   
대신 G1 GC는 더욱 효율적이라고 생각하는 위치로 객체를 Reallocate(재할당) 시킨다.   
예를 들어 Survivor1 영역에 있는 객체가 Eden 영역으로 할당하는 것이 더 효율적이라고 판단될 경우 Eden 영역으로 이동시킨다.


### **Shenandoah GC**
- Java 12에 release 
- 레드 햇에서 개발한 GC
- 기존 CMS가 가진 단편화, G1이 가진 pause의 이슈를 해결
- 강력한 Concurrency와 가벼운 GC 로직으로 heap 사이즈에 영향을 받지 않고 일정한 pause 시간이 소요가 특징

[![Shenandoah GC](https://blog.kakaocdn.net/dn/lHh4s/btrISNkkpMV/cuFxgmAT0DuafPhABn0L00/img.png)](https://blog.kakaocdn.net/dn/lHh4s/btrISNkkpMV/cuFxgmAT0DuafPhABn0L00/img.png)

#### **Shenandoah GC 실행 명령어**



```bash
java -XX:+UseShenandoahGC -jar Application.java
```

---

### **ZGC (Z Garbage Collector)**

- Java 15에 release
- 대량의 메모리(8MB ~ 16TB)를 low-latency로 잘 처리하기 위해 디자인 된 GC
- G1의 Region 처럼,  ZGC는 ZPage라는 영역을 사용하며, G1의 Region은 크기가 고정인데 비해, ZPage는 2mb 배수로 동적으로 운영됨. (큰 객체가 들어오면 2^ 로 영역을 구성해서 처리)
- ZGC가 내세우는 최대 장점 중 하나는 힙 크기가 증가하더도 'stop-the-world'의 시간이 절대 10ms를 넘지 않는다는 것

[![ZGC](https://blog.kakaocdn.net/dn/IKx8x/btrIT9f9qjM/f1g0NHSGJI7ce9rfxPZyuk/img.png)](https://blog.kakaocdn.net/dn/IKx8x/btrIT9f9qjM/f1g0NHSGJI7ce9rfxPZyuk/img.png)

#### **Z** **GC 실행 명령어**

```bash
java -XX:+UnlockExperimentalVMOptions -XX:+UseZGC -jar Application.java
```








---

참조 - https://mangkyu.tistory.com/118
https://mangkyu.tistory.com/119

https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC