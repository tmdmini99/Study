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

  
  

## `특정 객체로 Lock 걸기`

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