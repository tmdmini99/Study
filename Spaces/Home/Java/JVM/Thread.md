
# **스레드란?**

**스레드(Thread)**는 프로세스안에서 실질적으로 작업을 실행하는 단위를 말하며, 자바에서는 JVM(Java Virtual Machine)에 의해 관리됩니다. 프로세스에는 적어도 한개 이상의 스레드가 있으며, Main 스레드 하나로 시작하여 스레드를 추가 생성하게 되면 멀티 스레드 환경이 됩니다. 이러한 스레드들은 프로세스의 리소스를 공유하기 때문에 효율적이긴 하지만 잠재적인 문제점에 노출될 수도 있습니다. 

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
|void interrupt( )                  | 스레드를 중지 시킵니다.                                                                        |
|void join( )                      | 이 스레드가 끝날때까지 기다립니다.                                                             |
| void join(long millis) | 최대 millis 시간동안 이 스레드가 끝날때까지 기다립니다.                                        |
|static void sleep(long millis)     | millis 시간동안 현재 스레드를 일시중지시킵니다.                                                |
| static void yield( )               | 현재 스레드의 실행시간을 다른 스레드에게 양보합니다.                                           |
|static Thread currentThread( )     | 현재 실행중인 스레드 객체의 참조값을 반환합니다.                                               |
| long getId( ) **                     | 스레드의 Id를 반환합니다.                                                                      |
| **String getName( ) **                 | 스레드의 이름을 반환합니다.                                                                    |
| **int getPriority( ) **                | 스레드의 우선순위 값을 반환합니다. (우선순위 범위 : 1 ~ 10)                                    |
| **Thread.State getState( ) **          | 스레드의 state 값을 반환합니다.                                                                |
| **ThreadGroup getThreadGroup( ) **     | 스레드가 속한 스레드 그룹을 반환합니다.                                                        |
| **static boolean interrupted( ) **     | 현재 스레드의 interrupted 여부를 반환합니다.                                                   |
| **boolean isInterrupted( ) **          | 이 스레드의 interrupted 여부를 반환합니다.                                                     |
| **boolean isAlive( ) **                | 이 스레드가 살아있는지 여부를 반환합니다.                                                      |
| **boolean isDaemon( ) **               | 이 스레드가 데몬 스레드인지 여부를 반환합니다.                                                 |
| **void setDaemon(boolean on) **        | 이 스레드를 데몬 스레드로 변경합니다.                                                          |
| **void setName(String name) **         | 이 스레드의 이름을 name으로 변경합니다.                                                        |
| **void setPriority(int newPriority) ** | 이 스레드의 우선순위를 newPriority로 변경합니다.                                               |
| **String toString( )  **                | 이 스레드의 이름, 우선순위, 스레드그룹등의 정보를 담은 문자열을 반환합니다.                    |






참조 - https://kadosholy.tistory.com/121