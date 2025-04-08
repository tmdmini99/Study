
@Transactional

`@Transactional` 환경에서 `try-catch`를 잘못 쓰면 롤백이 안 될 수 있음


### 왜 try-catch 쓰면 롤백이 안 되냐?

Spring의 `@Transactional`은 **런타임 예외 (`RuntimeException` 또는 그 하위)** 가 발생했을 때 자동으로 롤백됩니다.  
하지만 `try-catch`로 예외를 잡아버리면, **Spring은 예외가 발생한 줄 모르기 때문에 롤백을 하지 않습니다.**


롤백 잘 되는 경우

```java
@Transactional
public void insertMsg(BasicCUDParamVo paramVo, String requestUri) {
    someRepository.save(...);
    throw new RuntimeException("에러 발생"); // 예외 던지면 → 롤백됨
}
```


롤백 안 되는 경우 (예외를 catch만 하고 무시함)

```java
@Transactional
public void insertMsg(BasicCUDParamVo paramVo, String requestUri) {
    try {
        someRepository.save(...);
        throw new RuntimeException("에러 발생");
    } catch (Exception e) {
        // 예외는 잡았지만 Spring은 모름 → 커밋됨
        System.out.println("에러 무시: " + e.getMessage());
    }
}
```



### 해결 방법

#### 방법 1. 예외 다시 던지기


```java
@Transactional
public void insertMsg(BasicCUDParamVo paramVo, String requestUri) {
    try {
        // 로직 수행
    } catch (Exception e) {
        // 로그 남기고 다시 던지기
        log.error("Insert 오류", e);
        throw e;  // 반드시 예외를 다시 던져야 rollback 됨
    }
}
```


방법 2. `TransactionAspectSupport`로 수동 롤백


```java
@Transactional
public void insertMsg(BasicCUDParamVo paramVo, String requestUri) {
    try {
        // 로직 수행
    } catch (Exception e) {
        log.error("Insert 오류", e);
        TransactionAspectSupport.currentTransactionStatus().setRollbackOnly(); // 수동 롤백
    }
}
```