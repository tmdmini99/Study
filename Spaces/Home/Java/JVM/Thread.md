
# **스레드란?**

**스레드(Thread)** 는 프로세스안에서 실질적으로 작업을 실행하는 단위를 말하며, 자바에서는 JVM(Java Virtual Machine)에 의해 관리됩니다. 프로세스에는 적어도 한개 이상의 스레드가 있으며, Main 스레드 하나로 시작하여 스레드를 추가 생성하게 되면 멀티 스레드 환경이 됩니다. 이러한 스레드들은 프로세스의 리소스를 공유하기 때문에 효율적이긴 하지만 잠재적인 문제점에 노출될 수도 있습니다. 

![[Thread1.png]]

#### 멀티 스레딩(multi-threading)

- 하나의 프로세스 내에서 둘 이상의 thread가 동시에 작업을 수행하는 것을 의미한다
- thread는 각각 자신만의 작업 공간을 가진다(context)
- 각 thread 사이에서 공유하는 자원이 있을 수 있다 (자바에서는 static instance)
- 여러 thread가 자원을 공유하여 작업이 수행되는 경우 서로 자원을 차지하려는 1race condition이 발생할 수 있다
- 여러 thread가 공유하는 자원중 경쟁이 발생하는 부분을 critical section이라고 한다
- critical section에 대해 동기적(순차적)인 구현하지 않으면 오류가 발생할 수 있다


### 자바에서 Thread 만들기

> Thread 클래스를 상속해서 만들기

![[Thread2.png]]
두 개의 스레드를 실행




![[Thread3.png]]





## **생성자**
| 생성자                                     | 설명                                                                           |
| ------------------------------------------ | ------------------------------------------------------------------------------ |
| **Thread( )**                             | 새로운 스레드 객체 할당                                                        |
| Thread(String name)                   | 새로운 스레드 객체가 할당되며, 스레드 이름은 name으로 설정됨                   |
| Thread(Runnable target)              |  Runnable target이 구현된 스레드 객체 할당                                      |
| Thread(Runnable target, String name)| Runnable target이 구현된 스레드 객체가 할당되면 스레드 이름은 name으로 설정됨. |


## **메소드**
| 메소드                                  | 설명                                                                                           |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- |
| void run( )                        | 스레드의 실행코드가 작성되는 메소드로 사용자는 run() 메소드를 오버라이드 하여 사용해야 합니다. |
| void start( )                      | 스레드가 시작되도록 요청하는 메소드로 JVM은 해당 스레드의 run() 메소드를 호출합니다.           |
| void interrupt( )                  | 스레드를 중지 시킵니다.                                                                        |
| void join( )                      | 이 스레드가 끝날때까지 기다립니다.                                                             |
| void join(long millis) | 최대 millis 시간동안 이 스레드가 끝날때까지 기다립니다.                                        |
| static void sleep(long millis)     | millis 시간동안 현재 스레드를 일시중지시킵니다.                                                |
| static void yield( )               | 현재 스레드의 실행시간을 다른 스레드에게 양보합니다.                                           |
| static Thread currentThread( )     | 현재 실행중인 스레드 객체의 참조값을 반환합니다.                                               |
| long getId( )                     | 스레드의 Id를 반환합니다.                                                                      |
| String getName( )                 | 스레드의 이름을 반환합니다.                                                                    |
| int getPriority( )                | 스레드의 우선순위 값을 반환합니다. (우선순위 범위 : 1 ~ 10)                                    |
| Thread.State getState( )          | 스레드의 state 값을 반환합니다.                                                                |
| ThreadGroup getThreadGroup( )      | 스레드가 속한 스레드 그룹을 반환합니다.                                                        |
| static boolean interrupted( )      | 현재 스레드의 interrupted 여부를 반환합니다.                                                   |
| boolean isInterrupted( )          | 이 스레드의 interrupted 여부를 반환합니다.                                                     |
| boolean isAlive( )                | 이 스레드가 살아있는지 여부를 반환합니다.                                                      |
| boolean isDaemon( )              | 이 스레드가 데몬 스레드인지 여부를 반환합니다.                                                 |
| void setDaemon(boolean on)         | 이 스레드를 데몬 스레드로 변경합니다.                                                          |
| void setName(String name)          | 이 스레드의 이름을 name으로 변경합니다.                                                        |
| void setPriority(int newPriority)  | 이 스레드의 우선순위를 newPriority로 변경합니다.                                               |
| String toString( )                  | 이 스레드의 이름, 우선순위, 스레드그룹등의 정보를 담은 문자열을 반환합니다.                    |

## **3. 스레드 생성방법**

자바에서 스레드를 만드는 방법은 2가지가 있습니다. 하나는 **Thread 클래스를 상속받는 방법**과 다른 하나는 **Runnable 인터페이스를 구현하는 방법**입니다. 자바는 다중 상속을 허용하지 않기 때문에 Thread 클래스를 상속받게 되면 다른 클래스를 상속받을수가 없습니다. 대신 인터페이스인 Runnable을 이용하여 작성하게 되면 이러한 문제점을 해결하실 수 있습니다. 

**스레드를 생성하고 동작시킴에 있어서 알아두어야 할 점**은 사용자가 스레드 객체를 생성하고 **실행요청을 하더라도** **스레드가 실행되는 것은 전적으로 JVM에 의한 스케쥴러에 따른다는 것**입니다. 사용자는 다만 Thread의 여러 메소드들을 통해서 JVM에 해당 명령들이 실행되도록 요청하는 것입니다. 

**1) Thread 클래스 상속 방식**

1. Thread 클래스를 상속한 클래스 정의
2. run() 메소드를 오버라이드하여 스레드 코드 작성
3. 스레드 객체 생성하기
4. start() 메소드로 스레드 시작하기

```java
public class HelloWorld {
	public static void main(String[] args) {
		MyThread mt1 = new MyThread();		// 3.스레드 객체 생성
		mt1.start();				// 4.스레드 실행
	}
}

class MyThread extends Thread {			// 1.Thread 클래스 상속한 클래스 정의
	public void run() {				// 2.run()메소드 오버라이드 및 스레드 코드 작성
		System.out.println(this.getName());
	}
}
```

**2) Runnable 인터페이스 구현 방식**

1. Runnable 인터페이스를 구현하는 클래스 정의
2. run() 메소드를 오버라이드하여 스레드 코드 작성
3. Runnable 객체 생성하기
4. Thread 객체 생성하기
5. start() 메소드로 스레드 시작하기




![[Thread4.png]]

자바는 다중 상속을 허용하지 않는다.  
그래서 이미 다른 클래스를 상속한 경우 Runnable interface를 구현해서 thread를 만든다.



```java
public class HelloWorld {
	public static void main(String[] args) {
		Runnable r1 = new MyThread();		// 3.Runnable 객체 생성
		Thread t1 = new Thread(r1);		// 4.Thread 객체 생성
		t1.start();				// 5.스레드 실행
	}
}

class MyThread implements Runnable {		// 1.Runnable 인터페이스를 구현하는 클래스 정의
	public void run() {				// 2.run()메소드 오버라이드 및 스레드 코드 작성
		System.out.println(Thread.currentThread().getName());
	}
}
```

### **4. 스레드 사용예제**

**1) 스레드 클래스 정의**

```java
public class MyThread extends Thread {

	public MyThread(String string) {
		super(string);
	}

	public void run() {
		int count = 0;
		while (!this.isInterrupted()) {
			System.out.print(this.getName());
			HelloWorld.threadSleep(this, 500);

			count++;
			if (count == 20) {
				this.interrupt();
				System.out.printf("%n[5]." + this.toString() + "-인터럽트됨]");
			}
		}
	}
}
```

**2) 스레드 객체 생성 및 실행**

```java
public class HelloWorld {
	public static void main(String[] args) {
		MyThread mt1 = new MyThread("*");		
		MyThread mt2 = new MyThread("#");		
		mt1.start();		
		mt2.start();
		
		System.out.printf("%n[1].mt1:" + mt1.isInterrupted() + ", mt2:" + mt2.isInterrupted() + "]");
		
		threadSleep(Thread.currentThread(), 2000);
		mt1.interrupt();
		System.out.printf("%n[2].mt1:" + mt1.isInterrupted() + ", mt2:" + mt2.isInterrupted() + "]");
		
		threadJoin(mt2);
		System.out.printf("%n[3].mt1:" + mt1.isInterrupted() + ", mt2:" + mt2.isInterrupted() + "]");
		
	}
	
	static void threadSleep(Thread t, long time) {
		try {
			Thread.sleep(time);
		} catch (InterruptedException e) {
			System.out.printf("%n[4]." + t.toString() + "-인터럽트됨]");
			t.interrupt();
		}
	}
	static void threadJoin(Thread t) {
		try {
			t.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```

**3) 실행결과**

```java
[1].mt1:false, mt2:false]#**#*##*
[2].mt1:true, mt2:false]
[4].Thread[*,5,main]-인터럽트됨]################
[5].Thread[#,5,main]-인터럽트됨]
[3].mt1:true, mt2:true]
```



## 스레드 상태 제어(thread state control)


![[Thread5.png]]

### sleep()
지정된 시간(밀리초) 동안 현재 thread를 일시 중단한다

```java
try{
    Thread.sleep(1000);
}catch(InterruptedException e){
    e.printStackTrace();
}
```

- wait() : 갖고 있던 고유 락을 해제하고 thread를 잠들게 한다.
- notify() : 잠들어 있던 thread 중 임의로 하나를 골라 깨운다.
- wait와 notify는 동기화된 블록 안에서 사용해야 한다. wait를 만나게 되면 해당 thread는 해당 객체의 모니터링 락에 대한 권한을 가지고 있다면 모니터링 락의 권한을 놓고 대기한다.


### Thread를 상속받은 ThreadB 클래스를 사용하여 wait()을 호출하는 예제

```java
class ThreadB extends Thread {
	// 해당 thread가 실행되면 자기 자신의 모니터링 락을 획득
	// 5번 반복하면서 1초씩 쉬면서 total에 값을 누적
	// 그후에 notify()메소드를 호출하여 wiat하고 있는 thread를 깨움
	int total;

	@Override
	public void run() {
		synchronized (this) {
			for (int i = 0; i < 5; i++) {
				System.out.println("ThreadB : " + i);
				total += i;
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			notify();
		}
	} // run
}

public class ThreadWait {

	public static void main(String[] args) {
		// 해당 thread가 실행되면, 해당 thread는 run메서드 안에서 자신의 모니터링 락을 획득
		ThreadB b = new ThreadB();
		b.start();

		// b에 대하여 동기화 블럭을 설정
		// 만약 메인 thread가 아래의 블록을 위의 Thread보다 먼저 실행되었다면 wait를 하게 되면서 모니터링 락을 놓고 대기
		synchronized (b) {
			try {
				// b.wait()메소드를 호출.
				// 메인 thread는 정지
				// ThreadB가 5번 값을 더한 후 notify를 호출하게 되면 wait에서 깨어남
				System.out.println("b 종료까지 대기");
				b.wait();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}

			// 깨어난 후 결과를 출력
			System.out.println("total : " + b.total);
		}
	}
}​
```

결과
```
**[Console]**  
b 종료까지 대기  
ThreadB : 0  
ThreadB : 1  
ThreadB : 2  
ThreadB : 3  
ThreadB : 4  
total : 10
```

### join() 
다른 thread가 멈출 때까지 기다리게 한다

![[Thread6.png]]

#### 스레드를 실행하고 해당 스레드가 종료될 때까지 기다리고 내용을 출력하는 예제

```java
class MyThread2 extends Thread {
	
	public void run() {
		for (int i = 0; i < 5; i++) {
			System.out.println("MyThread2 : " + i);
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	} // run
}

public class ThreadJoin {
	
	public static void main(String[] args) {
		
		MyThread2 thread = new MyThread2();
		// Thread 시작
		thread.start();
		System.out.println("thread 종료까지 대기");
		try {
			// 해당 스레드가 멈출 때까지 대기
			thread.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("thread 종료");
	}
	
}
```


결
```java
**[Console]**  
thread 종료까지 대기  
MyThread2 : 0  
MyThread2 : 1  
MyThread2 : 2  
MyThread2 : 3  
MyThread2 : 4  
thread 종료
```


### interrupt()
thread가 sleep(), wait(), join() 함수에 의해 non-runnable 상태일 때 interrupt()를 호출하면 다시 runnable 상태가 된다

Thread의 interrupt() 메소드

  

 Thread의 interrupt() 메소드는 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외를 발생시키는 역할을 한다. 이것을 이용하면 Thread의 run() 메소드를 정상 종료시킬 수 있다. 


  

 사용법은 아래와 같다. main 메소드에서 thread의 interrupt() 메소드를 실행하게 되면 thread가 sleep() 메소드로 일시 정지 상태가 될 때 thread 에서 InterruptedException이 발생하여 예외 처리(catch)블록으로 이동한다. 결국 thread는 while문을 빠져나와서 run() 메소드를 정상 종료하게 된다. 아래의 코드에서는 thread가 시작한 후 1초 뒤에 InterruptedException이 발생하여 thread를 멈추도록 interrupt() 메소드를 호출한다.


```java
public class InterruptExample { 
  public static void main(String[] args) {
    Thread thread = new PrintThread();
    thread.start();

    try { Thread.sleep(1000); } catch (InterruptedException e) {}

    thread.interrupt();  //스레드를 종료시키기 위해 InterruptedException을 발생시킴
  }
}

public class PrintThread extends Thread { 
  @Override
  public void run() {
    try {
      while(true) {
        System.out.println("실행중");
        Thread.sleep(1);
      }
    } catch(InterruptedException e) {
    }

    System.out.println("자원 정리");
    System.out.println("실행 종료");
    }
  }
}

  ```

 주목 할 점은 스레드가 실행 대기 또는 실행 상태에 있을 때 interrupt() 메소드가 실행되면 즉시 InterruptedException 예외가 발생하지 않고, 스레드가 미래에 일시 정지 상태가 되면 즉시 InterruptedException 예외가 발생한다는 것이다. 따라서 스레드가 일시 정지 상태가 되지 않는다면 interrupt() 메소드 호출은 아무런 의미가 없어지게 된다. 그래서 짧은 시간이나마 스레드를 일시정지 시키기 위해 Thread.sleep(1) 메소드를 호출한 것이다.

  
 일시 정지 상태를 만들지 않고도 interrupt() 호출 여부를 알 수 있는 방법이 있다. 스레드의 interrupt() 메소드가 호출되면 스레드의 interrupted() 와 isInterrupted() 메소드는 true를 반환하도록 되어 있다. Interrupted() 는 static 메소드로 현재 스레드가 interrupted 되었는지 확인하고, isInterrupted() 는 인스턴스 메소드로 현재 스레드가 interrupted 되었는지 확인 할 때 사용한다.

  boolean status = Thread.interruptec();
  boolean status = objThread.isInterrupted();

  

  

 그렇다면,  아래와 같이 응용이 가능하다.

  

 일시 정지 코드인 Thread.sleep(1)을 사용하지 않고, Thread.interrupted()를 사용해서 PrintTread의 interrupt()가 호출 되었는지 확인한 다음 while 문을 빠져나가도록 할 수 있다.
```java
public class PrintThread extends Thread { 
  @Override
  public void run() {
    while(true) {
      System.out.println("실행중");
    }
    if(Thread.interrupted()) {
      break;
    }

    System.out.println("자원 정리");
    System.out.println("실행 종료");
  }
}
```

## 3. _wait()_ 메소드

- **void wait() :** 현재 스레드를 다른 스레드가 이 객체에 대한 notify() 또는 notifyAll() 메소드를 호출할때까지 대기합니다. 


이를 위해 현재 스레드는 개체의 모니터 를 소유해야 합니다 . Javadocs에 따르면 이것은 다음과 같은 방식으로 발생할 수 있습니다.

### 3.1. _기다리다()_

_wait()_ 메서드 는 다른 스레드 가 이 객체에 대해 notify() 또는 _notifyAll(_ _)_ 을 호출할 때까지 현재 스레드가 무기한 대기하도록 합니다 .

### 3.2. _대기(긴 시간 초과)_

이 방법을 사용하여 스레드가 자동으로 깨어나는 시간 초과를 지정할 수 있습니다. _notify( )_ 또는 _notifyAll()_ 을 사용하여 시간 초과에 도달하기 전에 스레드를 깨울 수 있습니다 .

_wait(0)_ 호출 은 _wait()_ 호출과 동일합니다 .

### 3.3. _대기(긴 시간 초과, int nanos)_

이것은 동일한 기능을 제공하는 또 다른 서명입니다. 여기서 유일한 차이점은 더 높은 정밀도를 제공할 수 있다는 것입니다.

총 시간 초과 기간(나노초)은 _1_000_000*timeout + nanos_ 로 계산됩니다 .

## 4. _notify()_ 및 _notifyAll()_

이 개체의 모니터에 대한 액세스를 기다리고 있는 스레드를 깨우기 위해 _notify()_ 메서드를 사용합니다 .

대기 중인 스레드에 알리는 두 가지 방법이 있습니다.

### 4.1. _통지()_

이 객체의 모니터를 기다리는 모든 스레드에 대해( _wait()_ 메서드 중 하나를 사용하여) 메서드 _notify()_ 는 랜덤의 스레드 중 하나를 깨우도록 알립니다. 정확히 어떤 스레드를 깨울 것인지 선택하는 것은 비결정적이며 구현에 따라 다릅니다.

_notify()_ 는 단일 임의 스레드를 깨우기 때문에 스레드가 유사한 작업을 수행하는 상호 배타적 잠금을 구현하는 데 사용할 수 있습니다. _그러나 대부분의 경우 notifyAll()_ 을 구현하는 것이 더 실행 가능합니다 .

### 4.2. _모든 알림()_

이 메서드는 단순히 이 개체의 모니터에서 대기 중인 모든 스레드를 깨웁니다.

깨어난 스레드는 이 개체에서 동기화를 시도하는 다른 스레드와 마찬가지로 일반적인 방식으로 경쟁합니다.

그러나 실행을 계속하기 전에 항상 **스레드를 진행하는 데 필요한 조건에 대한 빠른 검사를 정의하십시오.** 스레드가 알림을 받지 않고 깨어난 상황이 있을 수 있기 때문입니다(이 시나리오는 나중에 예제에서 설명함).




- **void wait(long timeout) :** 현재 스레드를 다른 스레드가 이 객체에 대한 notify() 또는 notifyAll() 메소드를 호출하거나 timeout 시간동안 대기합니다. 
- **void notify() :** 이 객체에 대해 대기중인 스레드 하나를 깨웁니다. 
- **void notifyAll() :** 이 객체에 대해 대기중인 모든 스레드를 깨웁니다. 



---

sleep과 wait 차이

![[Thread7.png]]


 먼저 저는 My_object 객체 a와 b를 생성했어요. 그리고, worker를 200개 생성했는데요. 이 클래스의 내부를 봅시다.

![[Thread8.png]]

 이것은, Thread를 extend한 것인데요. 일단 My_object를 참조하는 레퍼런스 타입이 안에 들어있네요. 그리고, run 안에는 a.foo() 라는 친구를 수행하는데요. 그러면 My_object라는 클래스도 봐야 겠네요.

![[Thread9.png]]

 이 안에는 그냥 x가 start 되었다는 것과 끝났다는 것을 출력해 주기만 합니다. 그런데 내부에 wait(1)이 있네요. 이건 대체 무엇을 의미하길래 그럴까요? 일단 동기화 메소드가 있습니다. 그렇다는 이야기는, a.foo()를 Thread 1이 호출하는 순간 Thread0이 lock을 획득한다는 소리가 됩니다. a에 대한.

![[Thread10.png]]

 대충 이런 상황이에요. 그러면 일단, Thread 0이 시작되었다는 것을 출력할 겁니다. 그리고 wait(1)이 호출이 되는데요.
 
![[Thread11.png]]


 만약에, Thread 0이 foo 함수가 끝날 때 까지 계속 lock을 놓지 않았다면, 0 start 다음에 0 end가 호출이 되었을 겁니다. 그런데, 0 start가 호출이 된 다음에 다시 2 start가 찍혔어요. **이것은, wait(1)이 호출되면서 0이 가지고 있던 객체 a에 대한 lock이 풀렸다는 겁니다.**

![[Thread12.png]]

 그러면, a의 Lock을 얻기 위해 현재 queue에 있는 쓰레드들이 경쟁을 할 거에요. 스케쥴러는 이 중, thread_3을 선택했다고 칩시다.
![[Thread13.png]]


 그러면 thread_3이 lock을 획득을 했습니다. 이런 식으로 계속 갈 겁니다. wait에 아무런 인자도 넘기지 않으면, notify()나 notifyAll()로 다른 누군가가 깨울 때 까지 waiting 상태가 됩니다. **그런데 wait(m);을 넘기면, m 밀리세컨드가 지나면, 자동적으로 깨어나서 큐에 올라갑니다.**

![[Thread14.png]]

 그러면 스케쥴러에 의해서 thread_1이 선택되면, a에 대한 lock을 획득하고, 계속 실행을 할 거에요. synchronized 블록이 끝날 때 까지 lock을 가지고 있다가, 끝나면 lock을 풀 거에요. 이 부분은 그리 어렵지는 않아요. 그런데 wait(0)을 넣으면 이야기가 달라지는데요.

 0을 넣는 경우, 숫자와 상관 없이 notify나 notifyAll로 통지가 될 때 까지 기다립니다. 이것도 간단하게 봅시다.



![[Thread15.png]]
 wait(0); 을 넣었습니다. 그랬더니 실행 결과가 어떻게 나오나요?


![[Thread16.png]]
 ~ end라는 것이 출력이 되지 않습니다. 이 때에는 어떠한 thread가 notify나 notifyAll로 통지할 때 까지 기다리게 되는데, 그러한 호출 구문이 없어요. 게다가, Main 클래스 안에 worker들이 작업을 모두 끝낼 때 까지 대기하는 join()이 있는데요. worker들이 작업을 끝내지 못해요.

 그러니 worker들은 계속 대기하고 있을 겁니다. wait(0)이 0초동안 대기하는 거라 이전 포스트에서 간단히 설명했을 텐데요. 0초동안 대기하는 게 아니라, 시간과 관계 없이 통지가 될 때 까지 wait 하는 거에요. 이전 포스트에 0초동안 대기하는 거라 설명한 글은 정정하였습니다. 죄송합니다. 그러면 join 함수 안에는 wait(0)이 있는데, 이건 왜 정상적으로 동작할까요? main 쓰레드가 종료될 때 어떤 함수들이 호출되는지 뜯어보면서 이해해 보도록 합시다.

---

 그러면, 상식적으로 **획득한 lock을 해제한다고 한다면, synchronized 블록 내에 wait가 있어야 할 겁니다.** 그렇지 않다면 어떻게 될까요?

![[Thread17.png]]

 foo 함수를 이렇게 바꿔보고 실행시켜 봅시다.

![[Thread18.png]]

 그러면, illegalMoniterState 예외가 뜰 거에요. 이는 동기화 블록 안에서 wait를 호출하지 않았기 때문입니다. 정리하면 Object의 wait 메소드는, 현재 이것을 부른 Thread의 lock을 release 시킨다는 겁니다. 그리고 다시 깨어나서 실행 상태로 들어갔을 때 release 된 lock을 획득할 거고요. Thread.sleep은 어떻게 동작할까요?

![[Thread19.png]]

 이런 식으로 바꾸고 실행을 시켜 봅시다.

![[Thread20.png]]

 그러면 1이 start되었다는 것이 출력된 다음에, 1이 end 되었다는 게 출력되는데요. 이는 sleep를 호출해도 lock이 풀리지 않았다는 이야기입니다.

![[Thread21.png]]

 그러면 현재 sleeping 하고 있는 Thread이긴 하지만, lock을 가지고 있기 때문에, 다른 친구들이 a의 동기화 블록에 접근을 하지 못해요. 그렇기 때문에 xxx start가 출력이 되면, 그 다음에 xxx end가 출력이 될 거에요. **계속 LOCK을 hold 하고 있기 때문입니다.**




---
참조 - https://kadosholy.tistory.com/121


https://khys.tistory.com/15


https://codingdog.tistory.com/entry/java-wait-vs-sleep-%EC%96%B4%EB%96%A4-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EC%9E%88%EC%9D%84%EA%B9%8C%EC%9A%94