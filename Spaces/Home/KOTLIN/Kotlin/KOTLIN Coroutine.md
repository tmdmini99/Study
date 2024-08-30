코루틴이란 together를 뜻하는 co와 작업들의 집합을 뜻하는 Routine이 합쳐져 만들어진 단어로 '협동 루틴'을 뜻한다. 일반적으로 루틴은 하나의 입구와 출구를 가지는 반면, 코루틴은 여러개의 입구와 출구를 가질 수 있다. 이런 특징으로 이전에 실행이 중단된 지점에서 다시 실행을 재개할 수 있는 기능을 가진다.



### 코루틴의 개념

코루틴을 이해하기 위해서는 3개의 기본 개념을 이해해야 한다.

1. **Coroutine Scope** (코루틴 스코프): MainScope, GlobalScope, Coroutine Scope 등이 있다.
2. **Coroutine Builders** (코루틴 빌더): 코루틴을 생성하는 메소드를 의미한다. launch, async, withContext, runBlocking 등이 있다.
3. **Coroutine Context** (코루틴 컨텍스트): key와 element를 갖는 map, element에는 서브타입으로 Job, Deferred, Dispatcher등이 들어간다.

## 코루틴과 Suspend 함수


코투린에서는 데이터베이스 또는 네트워크 작업같은 Lon-running tasks에 대해 간편한 코드를 작성할 수 있게 해준다. 특히 비동기 콜백 작업을 순차적 코드로 간단하게 작성할 수 있다는 장점이 있다. 이를 위해서는 suspend라는 변경자를 함수에 붙이면 된다. 

suspend함수는 함수에 필요한 모든 런타임 문맥을 저장하고 함수 실행을 중단한 다음, 나중에 필요할 때 다시 실행을 계속 진행할 수 있게 한 것이다.

```run-kotlin
suspend fun task() {
    println("Task 시작")
    delay(100)
    println("Task 종료")
}
```




delay() 함수는 코루틴 라이브러리에 정의된 일시 중단 함수로 Thread.sleep()과 비슷한 일을 한다. 일시 중단 함수는 일시 중단 함수와 일반 함수를 원하는대로 호출할 수 있다. 일시 중단 함수를 호출하면 해당 호출 지점이 일시 중단 지점이 되며 일시 중단 지점은 나중에 재개할 수 있는 지점이 된다. 

코틀린에서 일반 함수가 일시 중단 함수를 호출하는 것을 금지한다.

현실적인 경우에는 공통적인 생명 주기와 문맥이 정해진 몇몇 작업이 정의딘 영역 안에서만 동시성 함수를 호출한다.

### Callback 과 Corutine의 가독성

Callback은 백그라운드 스레드를 사용해 긴 작업을 실행하고, 백그라운드 스레드에서 작업이 완료되면 메인 스레드에 정의된 콜백을 호출하여 작업 결과를 알려주는 방법이다. 기본적으로 콜백 구조를 사용하면 **Main-Safe**하게 작업을 처리할 수 있다. 하지만 이런 방식은 가독성이 매우 떨어지게 된다.

> **※ Main-Safe**  
> 메인 스레드가 제 역할을 하도록, Blocking 하지 않는 것을 의미

**Callback을 사용한 함수**

```kotlin
fun getGingerBrave(api: CookieService): gingerBrave {
    api.makeDough{ dough -> 
        api.addMagicPowder(dough){ -> magicDough
            api.escapeOven(magicDough) { -> cookie
                api.fetchGingerBrave(cookie) { -> gingerBrave
                    Log.d("You can't catch me! I'm the Gingerbre.. I'm Ginger Brave!")
                    return gingerBrave
                }
            }
        }
    }
}
```

**Suspend를 사용한 함수**

```kotlin
suspend fun getGingerBrave(api: CookieService): gingerBrave {
    val dough = api.makeDough()
    val magicDough = api.addMagicPowder(dough)
    val cookie = api.escapeOven(magicDough)
    val gingerBrave = api.fetchGingerBrave(cookie)
    Log.d("You can't catch me! I'm the Gingerbre.. I'm Ginger Brave!")
    return gingerBrave
}
```

코드를 보게 되면 Suspend를 사용한 Coroutine에서 가독성이 매우 좋아졌다는 것을 알 수 있다. 기본적으로 Suspend 키워드는 내부적으로 Callback을 생성하기에 이와같이 수정할 수 있다.


## 코루틴 빌더


코루틴 빌더는 CoroutineScope 인스턴스의 확장 함수로 쓰인다. CoroutineScope에 대한 구현 중 가장 기본적인 것으로 GlobalScope 객체가 있으며, 이를 사용하면 독립적인 코루틴을 만들 수 있다.

### launch()

launch() 빌더는 코루틴을 시작하고, 코루틴을 실행 중인 작업의 상태를 추적하고 변경할 수 있는 Job 객체를 돌려준다. 이 함수는 CoroutineScope. () -> Unit 타입의 일시 중단 람다를 받는다.


```kotlin
import kotlinx.coroutines.*

fun main() {
    val time = System.currentTimeMillis()

    GlobalScope.launch {
        delay(100)
        println("Task 1 finished in ${System.currentTimeMillis() - time}")
    }

    GlobalScope.launch {
        delay(100)
        println("Task 2 finished in ${System.currentTimeMillis() - time}")
    }

    Thread.sleep(200)
}
```

위의 코드를 실행하면 아래와 같은 결과를 확인할 수 있다.
```kotlin
Task 1 finished in 197
Task 2 finished in 197
```

두 작업이 프로그램을 시작한 시점을 기준으로 거의 동시에 끝났다는 점은 두 작업이 병렬적으로 실행되었다는 것을 알 수 있는 부분이다. 마지막에 Thread.sleep(200)을 한 이유는 코틀린을 처리하는 스레드는 데몬 모드로 실행되기 때문에 메인 스레드가 종료되면 실행이 종료되는 것을 막기 위함이다.

launch() 빌더는 동시성 작업이 결과를 만들어내지 않는 경우에 사용되어야 한다. 그래서 이 빌더는 Unit 타입을 반환하는 람다를 인자로 받는다. 그럼 만약 결과를 필요로 하는 경우에는 어떻게 해야하는가? 바로 async()라는 빌더를 사용하면 된다.

### async()

async()는 코루틴을 시작하고, Deferred의 인스턴스를 돌려준다. 이 인스턴스는 Job의 하위 타입으로 await() 메서드를 통해 계산 결과에 접근할 수 있게 해준다. await() 메서드를 호출하면 계산이 완료되거나 계산 작업이 취소될 때까지 현재 코루틴을 일시 중단시키게 된다.

```kotlin
import kotlinx.coroutines.*

suspend fun main() {
    val message = GlobalScope.async {
        delay(100)
        "hello "
    }

    val count = GlobalScope.async {
        delay(100)
        3
    }

    delay(100)

    val result = message.await().repeat(count.await())
    println(result)
}
```


위의 코드를 실행하면 아래와 같은 결과를 확인할 수 있다.

```kotlin
hello hello hello
```


### runblocking()

launch() 와 async() 빌더의 경우 스레드 호출을 blocking 시키지는 않지만, 백그라운드 스레드 풀을 통해 작업을 실행한다. 위의 예시들에서는 메인 스레드가 하는 일이 없었기 때문에 sleep을 통해 백그라운드 스레드의 작업을 기다리게 만들었다. 

runblocking() 빌더는 현재 스레드에서 실행되는 코루틴을 만들고 코루틴이 완료될 때까지 현재 스레드의 실행을 블럭시킨다. 코투틴이 종료되면 일시 중단 람다의 결과가 호출의 결과값이 된다.

```kotlin
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch {
        delay(100)
        println("백그라운드 스레드 작업: ${Thread.currentThread().name}")
    }

   runBlocking {
       println("메인 스레드 작업: ${Thread.currentThread().name}")
       delay(100)
   }
}
```

위의 코드를 실행시키면 아래의 결과를 확인할 수 있다.
```kotlin
메인 스레드 작업: main
백그라운드 스레드 작업: DefaultDispatcher-worker-1
```


runblocking() 내부의 코루틴은 메인 스레드에서 실행되고 launch()로 시작한 코루틴은 공유 풀에서 백그라운드 스레드를 할당 받았음을 알 수 있다. 이런 블러킹 동작 때문에 runblocking()은 다른 코루틴 안에서 사용하면 안된다. runblocking()은 블러킹 호출과 넌 블러킹 호출 사이의 다리 역할을 하기 위해 고안된 코루틴 빌더이므로, 테스트나 메인 함수엣 최상위 빌더로 사용하는 등의 경우에만 runblocking()을 써야 한다.



---
출처 - https://everyday-develop-myself.tistory.com/188