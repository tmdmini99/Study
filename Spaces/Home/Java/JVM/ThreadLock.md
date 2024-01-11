


# java - synchronized 란? 사용법?

  
  

멀티스레드를 잘 사용하면 프로그램적으로 좋은 성능을 낼 수 있지만,

멀티스레드 환경에서 반드시 고려해야할 점인 스레드간 동기화라는 문제는 꼭 해결해야합니다.

예를 들어 스레드간 서로 공유하고 수정할 수 있는 data가 있는데 스레드간 동기화가 되지 않은 상태에서 

멀티스레드 프로그램을 돌리면, data의 안정성과 신뢰성을 보장할 수 없습니다.

java에서 동기화 방식은 **메서드 앞에 `synchronized`를 선언**하여 동기화 시키는 방식과, **동기화 블럭을 만들어** 동기화를 시키는 방식 총 2가지가 존재한다.

따라서 data의 thread-safe를 하기 위해 자바에서는 **synchronized** 키워드를 제공해 스레드간 **동기화를** 시켜 data의**thread-safe** 를 가능케합니다.

> Thread-Safe
> 	스레드 안전(thread 安全, 영어: thread safety)은 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다. 보다 엄밀하게는 하나의 함수가 한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 스레드에서의 함수의 수행 결과가 올바로 나오는 것으로 정의한다.


자바에서 지원하느 Synchronized 키워드는 여러개의 스레드가 한개의 자원을 사용하고자 할 때,

**현재** **데이터를** **사용하고** **있는** **해당** **스레드를** **제외하고** **나머지** **스레드들은** **데이터에** **접근** **할** **수** **없도록** **막는** **개념**입니다.

Synchronized 키워드는 변수와 함수에 사용해서 동기화 할 수 있습니다.

하지만 **Synchronized** **키워드를** **너무** **남발하면** **오히려** **프로그램** **성능저하를** **일으킬** **수** **있습니다****.**

그 이윤 Synchronized 키워드를 사용하면 자바 내부적으로 메서드나 변수에 동기화를 하기 위해 block과 unblock을 처리하게 되는데 

이런 처리들이 만약 너무 많아지게 되면 오히려 프로그램 성능저하를 일으킬수 있는 것입니다. 

(block 과 unblock도 프로그램 내부적으로 어느정도  공수가 들어가는 작업입니다.)

따라서 적재적소에 Synchronized 키워드를 사용하는 것이 중요합니다!

**// 1.** **메서드에서** **사용하는** **경우**

**public synchronized void method(){//** **코드****}**

**// 2.** **객체** **변수에** **사용하는** **경우****(block****문****)**

**private Object obj = new Object();**

**public void exampleMethod(){ synchronized(obj){//****코드** **}}**


```java
public class ThreadSynchronizedTest {
 
    public static void main(String[] args) {
    // TODO Auto-generated method stub
        Task task = new Task();
            Thread t1 = new Thread(task);
            Thread t2 = new Thread(task);
            </p><p>
            t1.setName("t1-Thread");
            t2.setName("t2-Thread");
             
            t1.start();
            t2.start();
    }
 
}
 
class Account{
    int balance = 1000;
      
    public void withDraw(int money){
     
        if(balance >= money){
            try{
                Thread thread = Thread.currentThread();
                System.out.println(thread.getName() + " 출금 금액 ->> "+money);
                Thread.sleep(1000);
                balance -= money;
                System.out.println(thread.getName()+" balance:" + balance);
                 
            }catch (Exception e) {}
       
        }
    }
}
  
class Task implements Runnable{
    Account acc = new Account();
      
    @Override
    public void run() {
        while(acc.balance > 0){
            // 100, 200, 300 중의 한 값을 임의로 선택해서 출금(withDraw)한다.
            int money = (int)(Math.random() * 3 + 1) * 100;
            acc.withDraw(money);
       
        }
    }
}
```

위 예제에 대한 설명을 하면, 

Account 라는 클래스에는 balance 잔액과 이 잔액을 삭감시키는 인출메서드가 있습니다.

지난시간 스레드를 만들때 Runnable 인터페이스를 구현하여 Task를 만들고 이 Task를 Thread 생성시 생성자에 매개변수로 넣으면

우리가 만든 스레드가 Task에 정의되어있는 대로 일을 실행한다고 했습니다.

Runnable을 구현하여 만든 Task에 100,200,300 중 랜덤하게 값을 전달받아 moneny 변수에 할당하고 그 금액만큼 Account 인스턴스의 인출메서드를 호출해

 balance (잔액)을 출금시키는 일을 구현해놨습니다.

다음 main 메서드에서 스레드 t1, t2를 만들고 각각의 스레드 이름을 정의합니다.

t1, t2 스레드가 시작하면 잔액이 0이 될 때까지 두 스레드가 경쟁하며 출금시킬 것입니다.

결과화면

![[ThreadLock1.png]]

결과를 보면 분명 잔액이 0이 될때까지 출금을 하라고 햇는데 잔액이 마이너스가 됬습니다.

여기서 멀티스레드의 문제점이 발견됩니다. balance(잔액) thread-safe가 되지 않았기 때문에 t1 스레드가 잔액을 삭감하기 전에 

t2가 balance(잔액)에 접근해 삭감을 해버리고 다시 t1이 slee()에서 깨어나 balance(잔액) 을 삭감해버리기 때문에

잔액이 0 이하의 마이너스 값을 가지게 됩니다.

해결하는 방법은 간단합니다. 

공유데이터에 대한 접근과 수정이 이루어지는 메서드에 synchronized 키워드를 리턴타입 앞에 붙여주면 됩니다. 

t1스레드가 먼저 공유데이터나 메서드에 점유하고 있는 상태인 경우 block으로 처리하기 때문에 t1 이외의 스레드의 접근을 막습니다. 

t1스레드가 작업을 다 끝냈다면 .unblock으로 처리하여 t1 이외의 스레드의 접근을 허락합니다.

### synchronized 키워드로 멀티스레드 동기화 처리

```java
public class ThreadSynchronizedTest {
 
    public static void main(String[] args) {
        // TODO Auto-generated method stub
           Task task = new Task();
            Thread t1 = new Thread(task);
            Thread t2 = new Thread(task);
             
            t1.setName("t1-Thread");
            t2.setName("t2-Thread");
             
            t1.start();
            t2.start();
    }
 
}
 
class Account{
    int balance = 1000;
      
    public synchronized void withDraw(int money){
     
        if(balance >= money){
            try{
                Thread thread = Thread.currentThread();
                System.out.println(thread.getName() + " 출금 금액 ->> "+money);
                Thread.sleep(1000);
                balance -= money;
                System.out.println(thread.getName()+" balance:" + balance);
                 
            }catch (Exception e) {}
       
        }
    }
}
  
class Task implements Runnable{
    Account acc = new Account();
      
    @Override
    public void run() {
        while(acc.balance > 0){
            // 100, 200, 300 중의 한 값을 임의로 선택해서 출금(withDraw)한다.
            int money = (int)(Math.random() * 3 + 1) * 100;
            acc.withDraw(money);
       
        }
    }
}
```

결과화면


![[ThreadLock2.png]]
synchronized 키워드를 사용함으로써 balance 공유데이터에 대한 thread-safe를 시켰기 때문에

데이터나 메서드 점유하고 있는 스레드가 온전히 자신의 작업을 마칠 수 있습니다.  
  
  
다른 블로그에 좋은 글이 있어서 펌해봅니다  
메소드 레벨의 동기화는 해당 인스턴스 자체로 락을 걸어버린다.  

자바 동기화 블록은 메소드나 블록 코드에 동기화 영역을 표시하며 자바에서 경합 조건을 피하기 위한 방법으로 쓰인다.

**자바 synchronized 키워드**

 자바 코드에서 동기화 영역은 synchronizred 키워드로 표시된다. 동기화는 객체에 대한 동기화로 이루어지는데(synchronized on some object), 같은 객체에 대한 모든 동기화 블록은 한 시점에 오직 한 쓰레드만이 블록 안으로 접근하도록 - 실행하도록 - 한다. 블록에 접근을 시도하는 다른 쓰레드들은 블록 안의 쓰레드가 실행을 마치고 블록을 벗어날 때까지 블록(blocked) 상태가 된다.

synchronized 키워드는 다음 네 가지 유형의 블록에 쓰인다.

인스턴스 메소드

스태틱 메소드

인스턴스 메소드 코드블록

스태틱 메소드 코드블록

어떤 동기화 블록이 필요한지는 구체적인 상황에 따라 달라진다.

**인스턴스 메소드 동기화**

다음은 동기화 처리된 인스턴스 메소드이다.

```java
public synchronized void add(int value){

      this.count += value;

}
```

메소드 선언문의 synchronized 키워드를 보자. 이 키워드의 존재가 이 메소드의 동기화를 의미한다.

인스턴스 메소드의 동기화는 이 메소드를 가진 인스턴스(객체)를 기준으로 이루어진다. 그러므로, 한 클래스가 동기화된 인스턴스 메소드를 가진다면, 여기서 동기화는 이 클래스의 **한 인스턴스**를 기준으로 이루어진다. 그리고 한 시점에 오직 하나의 쓰레드만이 동기화된 인스턴스 메소드를 실행할 수 있다. 결국, 만일 둘 이상의 인스턴스가 있다면, 한 시점에, 한 인스턴스에, 한 쓰레드만 이 메소드를 실행할 수 있다. 

인스턴스 당 한 쓰레드이다. 

**스태틱 메소드 동기화**

스태틱 메소드의 동기화는 인스턴스 메소드와 같은 방식으로 이루어진다.

```java
public static synchronized void add(int value){

      count += value;

}
```

역시 선언문의 synchronized 키워드가 이 메소드의 동기화를 의미한다.

스태틱 메소드 동기화는 이 메소드를 가진 클래스의 클래스 객체를 기준으로 이루어진다. JVM 안에 클래스 객체는 클래스 당 하나만 존재할 수 있으므로, 같은 클래스에 대해서는 오직 한 쓰레드만 동기화된 스태틱 메소드를 실행할 수 있다.

만일 동기화된 스태틱 메소드가 다른 클래스에 각각 존재한다면, 쓰레드는 각 클래스의 메소드를 실행할 수 있다.

클래스 -쓰레드가 어떤 스태틱 메소드를 실행했든 상관없이- 당 한 쓰레드이다.

**인스턴스 메소드 안의 동기화 블록**

동기화가 반드시 메소드 전체에 대해 이루어져야 하는 것은 아니다. 종종 메소드의 특정 부분에 대해서만 동기화하는 편이 효율적인 경우가 있다. 이럴 때는 메소드 안에 동기화 블록을 만들 수 있다.

```java
public void add(int value){

    synchronized(this){

       this.count += value;   

    }

  }
```

이렇게 메소드 안에 동기화 블록을 따로 작성할 수 있다. 메소드 안에서도 이 블록 안의 코드만 동기화하지만, 이 예제에서는 메소드 안의 동기화 블록 밖에 어떤 다른 코드가 존재하지 않으므로, 동기화 블록은 메소드 선언부에 synchronized 를 사용한 것과 같은 기능을 한다.

동기화 블록이 괄호 안에 한 객체를 전달받고 있음에 주목하자. 예제에서는 'this' 가 사용되었다. 이는 이 add() 메소드가 호출된 객체를 의미한다. 이 동기화 블록 안에 전달된 객체를 **모니터** 객체(a monitor object) 라 한다. 이 코드는 이 모니터 객체를 기준으로 동기화가 이루어짐을 나타내고 있다. 동기화된 인스턴스 메소드는 자신(메소드)을 내부에 가지고 있는 객체를 모니터 객체로 사용한다.

같은 모니터 객체를 기준으로 동기화된 블록 안의 코드를 오직 한 쓰레드만이 실행할 수 있다.

다음 예제의 동기화는 동일한 기능을 수행한다.

```java
 public class MyClass {

    public synchronized void log1(String msg1, String msg2){

       log.writeln(msg1);

       log.writeln(msg2);

    }

    public void log2(String msg1, String msg2){

       synchronized(this){

          log.writeln(msg1);

          log.writeln(msg2);

       **}**

    }

  }
```

한 쓰레드는 한 시점에 두 동기화된 코드 중 하나만을 실행할 수 있다. 여기서 두 번째 동기화 블록의 괄호에 this 대신 다른 객체를 전달한다면, 쓰레드는 한 시점에 각 메소드를 실행할 수 있다. -동기화 기준이 달라지므로.

**스태틱 메소드 안의 동기화 블록**

다음 예제는 스태틱 메소드에 대한 것이다. 두 메소드는 각 메소드를 가지고 있는 클래스 객체를 동기화 기준으로 잡는다.

```java
 public class MyClass {

    public static synchronized void log1(String msg1, String msg2){

       log.writeln(msg1);

       log.writeln(msg2);

    }

    public static void log2(String msg1, String msg2){

       synchronized(MyClass.class){

          log.writeln(msg1);

          log.writeln(msg2);  

       }

    }

  }
```

같은 시점에 오직 한 쓰레드만 이 두 메소드 중 어느 쪽이든 실행 가능하다. 두 번째 동기화 블록의 괄호에 MyClass.class 가 아닌 다른 객체를 전달한다면, 쓰레드는 동시에 각 메소드를 실행할 수 있다.





```java
public class MusicExam {
    public static void main(String[] args) {
        MusicBox box = new MusicBox();

        MusicPlayer musicPlayer1 = new MusicPlayer(1, box);
        MusicPlayer musicPlayer2 = new MusicPlayer(2, box);

        musicPlayer1.start();
        musicPlayer2.start();
    }
}
```

```java
public class MusicBox {
    public synchronized void playMusicA( ) {
        for (int i = 0; i < 5; ++i) {
            System.out.println("MusicA !!");

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public synchronized void playMusicB() {
        for (int i = 0; i < 5; ++i) {
            System.out.println("MusicB !!");

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public class MusicPlayer extends Thread {
    int type;
    MusicBox musicBox;

    public MusicPlayer(int type, MusicBox musicBox) {
        this.type = type;
        this.musicBox = musicBox;
    }

    @Override
    public void run() {
        switch (type) {
            case 1:
                musicBox.playMusicA();
                break;
            case 2:
                musicBox.playMusicB();
                break;
        }
    }
}
```

```
MusicA !!
MusicA !!
MusicA !!
MusicA !!
MusicA !!
MusicB !!
MusicB !!
MusicB !!
MusicB !!
MusicB !!
```

위의 코드를 간단히 먼저 이해를 하고 계속 글을 따라오시면 됩니다. (간단하게 말하자면.. MusicBox를 공유하고 있습니다.) 지금 MusicBox 클래스에 playMusicA, playMusicB에 `synchronized` 키워드가 메소드 레벨에서 붙어있는 것을 볼 수 있습니다. 그러면 위의 코드 결과는 어떻게 될까요? playMusicA 찍히고 MusicB가 찍히는 것을 알 수 있습니다.

왜 그럴까요? 바로 `객체 단위로 Lock을 가지고 있기 때문입니다.` 즉, 객체당 하나의 Lock만 가질 수 있기 때문에 playMusicA가 먼저 Lock을 가지고 메소드를 실행하고 있기 때문에 playMusicB는 기다리게 되는 것입니다. 위의 개념이 이번 글의 핵심입니다. 그러면 이번에는 `synchronized 블럭`에 대해서 알아보겠습니다

## `synchronized 블럭`

```java
public class MusicBox {
    public void playMusicA() {
        synchronized(this) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicA !!");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public void playMusicB() {
        synchronized (this) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicB !!");

                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

이번에는 `synchronized 블럭`을 사용했습니다. 현재 코드로만 보면 메소드 레벨에서 쓰나 블럭으로 쓰나 차이가 없습니다. 하지만 synchronized 블럭을 쓰면 메소드 전체에 Lock을 거는 것이 아니라 본인이 필요한 부분에만 Lock을 걸 수 있다는 유연함?을 가질 수 있습니다. 이정도의 개념까지는 괜찮았는데, `this`가 정확히 의미하는게 어떤 것일까? 라는 생각을 했습니다. `this.name = name` 또는 `this.method()`와 같이 쓰는 것을 보았을 것입니다. 이 때 this의 의미는 클래스 내부 필드, 메소드를 의미합니다.

`그러면 synchronized 블럭에서 this는 어떤 것을 의미할까요?`

제가 생각한 결론은 `this`라고 해도 그냥 `객체 단위의 Lock을 거는 것`이라고 생각했습니다. 즉, MusicBox 객체의 단위의 Lock을 거는 것입니다. 그래서 위의 코드를 실행해보면 역시나 playMusicA, playMusicB가 차례대로 출력이 되는 것을 볼 수 있습니다. (이유는 위의 synchronized 메소드와 같습니다.)

> Thread Lock이란?
>	  -  일반적으로 상호 배제 정책을 통해, 하나의 스레드가 특정 자원에 접근중인 경우에는 다른 스레드가 접근하지 못하도록 제한한다. 락이 없다면 두 개 이상의 스레드가 동시에 자원에 접근할 수 있으므로, 데이터의 무결성이 보장되지 않는다. 락의 개념은 멀티스레드를 사용하는 환경이라면 어디든 사용될 수 있다.
		  lock은 기본적으로 lock을 획득한 thread만 critical section을 실행하며, 사용하고 난 뒤에 lock을 반환한다 따라서 다른 thread가 lock을 갖고 있지 않을 때 lock을 획득하는 **lock()** 함수와  critical section을 모두 수행한 뒤에 lock을 반환하는 **unlock()** 함수가 사용된다.

## 특정 객체로 Lock 걸기

``` java
public class MusicBox {
    private final Object object = new Object();

    public void playMusicA() {
        synchronized(object) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicA !!");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public void playMusicB() {
        synchronized (this) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicB !!");

                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

위와 같이 코드를 바꾼다면 결과는 어떻게 출력될까요? 이번에는 `Object 객체`를 만들어서 synchronized 블럭 괄호안에 넣었습니다. 결과는 playMusicA, playMusicB가 번갈아가면서 출력이 됩니다. 왜 그럴까요? 위에서 Lock의 개념을 이해했다면 충분히 결과를 예측할 수 있습니다.

`Object 객체도 Lock을 1개 가지고`, `MusicBox 객체도 Lock을 1개 가지기 때문에` 각자 Lock을 사용하면서 메소드에 접근할 수 있는 것입니다.

```java
public class MusicBox {
    private final Object object = new Object();
    public void playMusicA() {
        synchronized(object) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicA !!");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public void playMusicB() {
        synchronized (object) {
            for (int i = 0; i < 10; ++i) {
                System.out.println("MusicB !!");

                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

그래서 당연히 위와 같이 `Object 객체 하나로만 잠금을 설정했다면 당연히 결과는 MusicA, MusicB 차례대로 출력이 됩니다.`









Ex)
```java
import java.util.ArrayList;

// wait() & notify()
public class ThreadWaitEx3 {
    public static void main(String[] args) throws Exception {
        Table3 table = new Table3(); // 여러 쓰레드가 공유하는 객체

        new Thread(new Cook3(table), "COOK1").start();
        new Thread(new Customer3(table, "donut"), "CUST1").start();
        new Thread(new Customer3(table, "burger"), "CUST2").start();
        
        Thread.sleep(2000);
        System.exit(0); // 프로그램 전체를 종료. (모든 쓰레드가 종료됨)
    }
}


class Customer3 implements Runnable {
    private Table3 table;
    private String food;

    Customer3(Table3 table, String food) {
        this.table = table;
        this.food = food;
    }

    public void run() {
        while (true) {
            try { Thread.sleep(100); } catch (InterruptedException e){}
            String name = Thread.currentThread().getName();
            table.remove(food);
            System.out.println(name +" ate a " + food);
        }
    }
}

class Cook3 implements Runnable {
    private Table3 table;
    Cook3(Table3 table) { this.table = table; }
    public void run() {
        while (true) {
            int idx = (int) (Math.random() * table.dishNum());
            table.add(table.dishNames[idx]);
            try { Thread.sleep(10); } catch (InterruptedException e) { }
        }
    }
}

class Table3 {
    String[] dishNames ={ "donut", "donut", "burger"};
    final int MAX_FOOD = 6;

    private ArrayList<String> dishes = new ArrayList<>();
    
    public synchronized void add(String dish) {
        while(dishes.size() >= MAX_FOOD){
            String name = Thread.currentThread().getName();
            System.out.println(name+" is waiting");
            try {
                /** 음식이 가득찼으므로 COOK 쓰레드를 기다리게 한다. */
                wait();
                Thread.sleep(500);
            } catch (InterruptedException e){}
        }
        
        dishes.add(dish);
        /** 음식이 추가되면 기다리고 있는 CUST를 깨운다. */
        notify();
        
        System.out.println("Dishes : " + dishes.toString());
    }

    public void remove(String dishName) {
        synchronized(this) {
            String name = Thread.currentThread().getName();
            while (dishes.size() == 0) {
                System.out.println(name+" is waiting");
                
                try {
                    /** 원하는 음식이 없는 CUST 스레드를 기다리게한다. */
                    wait();
                    Thread.sleep(500);
                }
                catch (InterruptedException e) { }
            }

            while (true) {
                for(int i = 0; i < dishes.size(); i++) {
                    if (dishName.equals(dishes.get(i))) {
                        dishes.remove(i);
                        
                        /** 음식을 하나라도 비우면 잠자고 있는 COOK을 깨운다. */
                        notify();
                        
                        return; // 음식먹었으면 return
                    }
                }
                
                try {
                    System.out.println(name+" is waiting");
                    
                    /** 원하는 음식이 없는 CUST 쓰레드를 기다리게 한다. */
                    wait();
                    
                    Thread.sleep(500);
                } catch (InterruptedException e){}
            } 
        }
    }
    
    public int dishNum(){ return dishNames.length; }
}
```


결과



![[ThreadLock3.png]]







## 자바 모니터

자바 스레드 동기화 모델은 "모니터"라는 개념을 적용하고 있다. 모니터에 대해서 먼저 간단하게 살펴보고 자바 동기화에 대해 상세하게 살펴보자.


**Monitor(모니터)의 개념**

- 하나의 데이터(객체)마다 하나의 모니터를 결합할 수 있으며, 모니터는 그것이 결합된 데이터(객체)가 동시에 두개 이상 의 스레드에 의해 접근 할 수 없도록 막는 잠금(lock)기능을 제공함으로써 동기화를 수행한다는 것이 주된 내용이다.
- 즉, 데이터(객체)에 모니터를 결합하면 하나의 스레드가 그 데이터를 사용하는 동안에는 다른 스레드들이 그 데이터를 사용할 수 없게 된다.
- 자바에서는 synchronized 메소드가 선언된 객체와 synchronized 블럭에 의해 동기화되는 모든 객체에 고유한 모니터가 결합이 되어 동기화 작업을 수행하게 된다.

**모니터의 구성**

- 스레드 단위로 모니터락을 획득(acquire lock)하거나 반환(release lock)한다.
- 동기화 코드(동기화메소드나 블럭)를 수행할 때에는 동기화 대상 인스턴스와 결합된 Monitor Lock을 획득한 후에 진입이 가능하며, 동기화 코드를 벗어날 때에는 Monitor Lock을 반환하게 된다.
- 동기화 대상 인스턴스별로 이와 결합된 Monitor가 존재하며 해당 모니터는 현재 락을 획득한 스레드와 Lock Count정보를 관리한다.
- 모니터가 Lock Count정보를 유지한다는 것은 동일 스레드가 중복해서 lock을 걸수 있다는 의미이다.

**Monitor와 스레드 대기 자료구조 2가지**

- 모니터는 락을 획득하기 위해 시도하는 스레드를 대기시키기 위한 자료구조와 조건변수(Conditional Variable)로서의 역할을 수행하기 위해 특정조건이 만족될 때까지 스래드를 대기시키기 위한 자료구조를 가지고 있다.

​ **1) EntrySet(진입셋)**  
​ ㅇ 자바 공식 교제에서는 LOCK-POOL이라는 명칭을 사용  
​ ㅇ 모니터락을 기다리는 스레드를 담아두기 위한 자료구조  
​ **2) WaitSet(대기셋)**  
​ ㅇ 자바 공식 교제에서는 WAIT-POOL이라는 명칭을 사용  
​ ㅇ 모니터가 notify(notifyAll)해줄 때까지 기다리는 스레드를 담는 자료구조


2. ) 한 스레드가 동기화 코드 영역(synchronized method or block)에 접근하기 위해 EntrySet에 진입한다.
    
    - Runnable 상태 -> BLOCKED상태
    
    2) 모니터락을 소유한 스레드가 있다면 해당 스레드가 모니터락을 반환할 때까지 EntrySet에서 대기한다.
    
    - BLOCKED상태
    
    3) 모니터락을 소유한 스레드가 없다면 EntrySet에서 대기하던 스레드 중 하나의 스레드가 선택되어 모니터락을 획득하고 실행을 하게 된다. 동기화 코드영역을 벗어나면서 소유한 모니터락을 반환한다.
    
    - 선택된 스레드 : Runnable 상태, 나머지 스레드 : BLOCKED 상태
3. **A. 모니터 락 획득(acquire)및 반환(release) 과정**
    




1) 한 스레드가 모니터락을 획득하고 동기화 코드 영역(synchronized method or block)에 진입

- Runnable 상태
- 2) 조건이 만족되지 않아 현재 스레드를 대기(wait(), wait(long)호출)시키면 해당 스레드는 소유한 모니터락을 반환한 후 WaitSet에서 대기한다. 락을 반환하는 이유는 조건이 만족되기 위해서는 다른 스레드가 접근해서 무엇가 작업을 해주어야 하기 때문에 반환을 하는 것이다.
- Runnable 상태 -> WAITING / TIMED-WAITING 상태4) WaitSet에서 대기하던 스레드는 신호를 받으면 WaitSet에서 EntrySet으로 옮겨진다. 왜냐하면 다시 실행을 하기 위해서는 모니터락을 획득해야 하기 때문에 EntrySet으로 옮겨진 것이다. 신호를 보낸 스레드가 동기화 코드 영역을 벗어나면(모니터 락을 반환하면) EntrySet에서 대기하던 스레드 중 하나의 스레드가 선택되어 모니터락을 획득하고 실행을 하게 된다.
- 3) 다른 스레드가 모니터락을 획득하여 동기화 코드 영역에 진입, 작업을 수행한 후 WaitSet에서 대기중인 스레드에게 그 사실을 알리기 위해 신호(notify(), notifyALL())를 보낸다.
- 선택된 스레드 : Runnable상태, 나머지 스레드 : BLOCKED상태
- _주의!!! 선택이 되지 않았다고 다시 WAITING / TIMED-WAITING 상태로 가는 것이 아니다. 신호(notify()/notifyAll())를 받는 시점에서 WaitSet에서 EntrySet으로 이동을 했으므로 락을 얻기 위해 대기하는 BLOCKED상태가 되는 것이다.*_

3. 자바 Monitor가 지원하는 동기화

ㅇ 자바 모니터는 다음 두가지 동기화를 모두 지원해 주고 있다.  
​ **A. Mutual Exclusion(상호배제)**

- 상호배제라는 것은 둘 이상의 스레드가 임계영역에 동시에 접근하는 것을 막는 것을 말한다.
- 다수의 스레드가 데이터를 공유할 때 서로 간섭없이 접근하고자 할때 필요하다.
- 동기화코드(동기화 메소드 또는 블럭)를 통해 동기화 대상 인스턴스의 Lock을 얻은 스레드만이 임계영역에 접근이 가능하며, Lock을 얻지 못한 스레드는 EntrySet(Lock Pool)에서 대기하였다가 다음 기회에 Lock을 얻기 위해 경쟁하게 된다.

​ **B. Cooperation(협력)**

- 모니터가 조건변수의 역할을 수행, 멀티스레드간 공유데이터의 동시접근을 막을 뿐만이 아니라 접근 순서도 컨트롤 하는 것을 말한다.
- 같은 목적을 가진 스레드간에 협력해서 효율적으로 작업을 할 수 있도록 하겠다 라는 의미이다. 생산자-소비자 스레드를 생각해보면 단순히 상호배제만을 해서는 부족하다. 즉 동시접근도 차단하고 생산자스레드가 생산을 했을 때 소비자 스레드에게 이를 알려서 작업을 할 수 있도록 해 주는 것이 필요하다. 이런 것이 바로 스레드간의 협업이다.
- 동기화 대상 인스턴스의 wait() / notify() / notifyall()메소드를 이용하여 스레드간 접근 순서 컨트롤을 수행할 수 있다.



---


참조 - https://devlog-wjdrbs96.tistory.com/420

https://coding-start.tistory.com/68 # synchronized

https://mgyo.tistory.com/602

https://devfunny.tistory.com/855