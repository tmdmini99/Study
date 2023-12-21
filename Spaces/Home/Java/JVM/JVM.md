## 이번에는 **JVM에** 대해 정리할 것이다.

---

## ▶ JVM 이란?

**JVM이란 Java Virtual Machine, 자바 가상 머신**의 약자를 따서 줄여 부르는 용어이다.


직역하면 '자바를 실행하기 위한 가상 기계(컴퓨터)'라고 할 수 있다.

(가상 머신이란 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것)

**JVM**의 역할은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것이다.

그리고 **JVM은 Java와 OS(운영체제) 사이에서 중개자 역할을 수행하여 Java가 OS(운영체제)에 구애받지 않고**

**독립적으로 작동이 가능**하다. 또한 **가장 중요한 메모리 관리, Garbage collection(가비지 컬렉션)을 수행**한다.

## ▶ JVM의 특징

1. 컴파일된 바이트 코드를 기계가 이해할 수 있는 기계어로 변환
2. 스택 기반의 가상 머신
3. 메모리 관리와 GC를 수행




## ▶ JVM 구조와 작동 원리

![JVM 구조 - (출처는 맨 하단에 기재)](https://blog.kakaocdn.net/dn/cssiwB/btrDAQE2Zod/U7NTDqHKKGkKqpG3jOlBX0/img.png)


![[Pasted image 20231218121117.png]]



JVM 구조 - (출처는 맨 하단에 기재)

**JVM의 구조**에는 크게 **Class Loader, Runtime data areas, Execution Engine, GC** 으로 나누어져 있다.

**Class Loader**는 클래스 파일을 Runtime Data Area의 메서드 영역으로 불러오는 역할을 한다.
#### 1. Class Loader(클래스 로더)

JVM내로 클래스파일(.class)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다. Runtime 시점에 클래스를 로딩하게 해 주며 클래스의 인스턴스를 생성하면 클래스 로더를 통해 메모리에 로드하게 된다.

**Execution Engine은. class파일과** 같은 ByteCode를 실행 가능하도록 해석한다.
#### 2. Execution Engine(실행 엔진)

로드된 클래스의 바이트코드를 실행하는 런타임 모듈이 바로 실행 엔진이다. 클래스 로더를 통해 JVM내의 Runtime Data Areas에 배치된 바이트코드는 실행 엔진에 의해 실행되며, 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. 여기서 Interpreter(인터프리터) 방식과 JIT compiler 방식을 사용하게 된다.
##### 3. Interpreter(인터프리터)

인터프리터는 프로그래밍 언어의 소스 코드를 바로 실행하는 프로그램을 말한다. 원시 코드를 기계어로 번역하는 컴파일러와 대비된다. 자바는 인터프리터 방식을 사용하여 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. 하지만 한 줄씩 수행하기 때문에 수행 속도가 느리다는 단점이 있다.

##### 4. JIT Compiler (Just In Time Compiler)

인터프리터 방식의 단점을 보완하기 위해 JIT 컴파일러가 도입되었다. JIT 컴파일러는 바이트코드를 컴파일하여 native code(네이티브 코드)로 변환하여 사용한다. 즉 한 번 컴파일된 코드는 빠르게 수행하게 되어 수행 속도가 빠르게 된다. 하지만 컴파일하는 과정에 비용이 들게 된다. 따라서 한 번만 수행할 코드라면 컴파일하지 않고 인터프리팅 하는 것이 유리하다. 따라서 JVM은 인터프리터 방식을 사용하다가 일정한 기준이 넘어가면 JIT 컴파일러를 사용하는 혼합 방식을 사용한다. 


**GC(Garbage Collector)는**  메모리 관리 기법 중 하나로, Heap 영역에 배치된 객체들을 관리하는 모듈이다.
#### 5. Garbage Collector(가비지 컬렉터)

가비지 컬렉터는 유효하지 않은 메모리인 가비지(Garbage)를 정리해주는 역할을 한다. 즉 Garbage Collection(GC)를 담당한다.

**Runtime Data Area**는 런타임 시 클래스 데이터와 같은 메타 데이터와 실제 데이터가 저장되는 곳이다.

간단하게 말하자면 프로그램을 수행하기 위해 OS로부터 할당받은 메모리 영역을 의미한다. (**Java 메모리 공간**)

Runtime Data Area에는 또다시 **Method Area, Heap, PC Registers, Java Stacks, Native Method Stacks**로 나누어진다.

![Runtime Data Area - (출처는 맨 하단에 기재)](https://blog.kakaocdn.net/dn/XLtjO/btrDyGDpp0C/K8wEGphqloy5uKZTC08Y7k/img.png)

Runtime Data Area 

※ Java는 멀티 스레드 환경으로 모든 스레드는 **Heap, Method Area를 공유**한다.

#### **▷ PC Register**

- JVM은 스택 기반의 가상 머신으로, CPU에 직접 접근하지 않고 [[Stack]]에서 주소를 뽑아서 가져온다. 가져온 주소는  PC Register에 저장된다.
- 따라서, 현재 어떤 명령을 실행해야 할 지에 대한 기록을 담당

#### **▷ JVM Stacks**

- 호출된 메서드의 파라미터, 지역 변수, 리턴 값 및 연산 값 등이 저장되는 영역
- 프로그램 실행 시 임시로 할당되었다가 메서드를 빠져나가게 되면 소멸되는 특성의 데이터들이 저장되는 영역
- 메서드 호출 시마다 스택에 각각의 스택 프레임이 생성되고, 수행이 끝나면 스택 포인트에서 해당 프레임을 제거

#### **▷ Native Method Stacks**

- Java 이외의 언어에 제공되는 Method의 정보가 저장되는 공간 / Java Native Interface를 통해 바이트 코드로 저장
- Kernel이 자체적으로 Stack을 잡아 독자적으로 프로그램을 실행시키는 영역

#### **▷ Heap**

- [[Garbage Collection|GC(가비지 컬렉션)]]의 대상이 되는 영역
- 객체를 동적으로 생성하게 되면 인스턴스가 Heap 영역의 메모리에 할당된다.
- 단, 레퍼런스 변수의 경우, Heap에 인스턴스가 저장되는 것이 아닌 포인터가 저장된다.

#### **▷ Method Area**

- 클래스 정보를 처음 메모리에 올릴 때 초기화되는 대상을 저장하기 위한 영역
- **올라가는 정보는 다음과 같다.**

##### **Field Information**

- 멤버 변수에 대한 정보 (이름, 타입, 접근 지정자 등)

##### **Method Information**

- 메서드에 대한 정보 (이름, 리턴 타입, 파라미터, 접근 지정자 등)

##### **Type Information**

- Class 인지 Interface 인지 혹은 Type의 속성, 이름, super class의 이름 등
- 또한 Method Area에는 상수형을 저장하고 중복을 막는 Runtime Constant Pool이 존재



## **▶ JVM의 주요 장점**
1. 이식성 : JVM은 플랫폼에 독립적이므로, 한 번 작성한 자바 프로그램은 여러 운영체제와 장치에서 실행될 수 있다. JVM은 특정 운영체제에 종속되지 않고 중간 계층으로서 작동하기 때문에, 자바 프로그램은 운영체제에 구애받지 않고 이식성을 갖는다.
2. 메모리 관리 : JVM은 가비지 컬렉션(Garbage Collection)을 통해 자동으로 메모리를 관리한다. 개발자는 메모리 할당과 해제를 명시적으로 다룰 필요가 없으며, 메모리 누수와 같은 문제를 줄일 수 있다.
3. 보안 : JVM은 자바 프로그램을 보호하는 여러 가지 보안 기능을 제공한다. 바이트코드의 검증, 클래스 로딩시 보안 검사, 액세스 제어 등의 기능을 통해 안전한 실행 환경을 제공한다.
4. 동적 로딩 : JVM은 동적 클래스 로딩을 지원한다. 이를 통해 프로그램이 실행 중에 필요한 클래스를 동적으로 로드하고 사용할 수 있다. 이는 유연성과 확장성을 높여준다.

## **▶ JVM의 주요 단점**

---

1. 실행 속도 : JVM은 인터프리터와 JIT(Just-In-Time) 컴파일러를 사용하여 Java 바이트코드를 기계어로 변환한다. 이러한 변환 과정은 초기 실행 시간이 필요하며, 일부 언어와 비교하여 상대적으로 실행 속도가 느릴 수 있다. 그러나 최적화된 JIT 컴파일러를 사용하면 이러한 성능 문제를 상당부분 해결할 수 있다.
2. 메모리 사용량 : JVM은 자체적인 메모리 사용량이 상대적으로 크다. JVM 자체의 오버헤드 및 가비지 컬렉션 작업에 필요한 추가적인 메모리를 사용하기 때문에, 자원이 제한적인 환경에서는 부담이 될 수 있다.
3. 시작 시간 : JVM은 초기화 및 클래스 로딩 등의 작업이 필요하므로, 프로그램의 시작 시간이 상대적으로 오래 걸릴 수 있다. 특히 작은 규모의 애플리케이션의 경우, 이러한 시작 시간이 부담이 될 수 있다.
4. 리소스 관리 : 가비지 컬렉션은 자동으로 메모리를 관리하지만, 이로 인해 일시적인 정지나 실행 속도 저하가 발생할 수 있다. 특히 대규모 및 실시간 시스템에서는 이러한 리소스 관리 문제가 중요한 고려 사항이 될 수 있다.


## **▶ JVM의 실행 과정**
1. 자바 프로그램이 실행되면 JVM은 OS로부터 프로그램 실행에 필요한 메모리를 할당받는다. JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어 들여 자바 바이트코드(.class)로 변환시킨다.
3. 클래스 로더(Class Loader)를 통해 클래스 파일(.class)을 JVM으로 로딩한다.
4. 로딩된 클래스 파일은 실행 엔진(Execution Engine)을 통해 해석된다.
5. 해석된 바이트코드는 Runtime Data Area에 배치되어 실질적인 수행이 이루어진다.
6. 위 실행과정 속에서 JVM은 필요에 따라 스레드 동기화(Thread Synchronization)와 GC 같은 작업을 수행한다.


---
출처: https://backendcode.tistory.com/161


https://velog.io/@supark4170/Java-heap-space%EA%B0%80-%EB%B6%80%EC%A1%B1%ED%95%B4%EC%9A%94

https://wonsjung.tistory.com/574

https://velog.io/@jomminii/whiteship-java-01-what-is-jvm


https://steady-coding.tistory.com/587

https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80