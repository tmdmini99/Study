
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
try{ Thread.sleep(1000); }catch(InterruptedException e){ e.printStackTrace(); }
```






---
참조 - https://kadosholy.tistory.com/121


https://khys.tistory.com/15