## **Redis란 무엇인가?**

### **✅ Redis (Remote Dictionary Server)란?**

**Redis는 데이터를 메모리에 저장하는 NoSQL 데이터베이스로, 빠른 속도의 키-값(Key-Value) 저장소**입니다.  
MySQL 같은 전통적인 데이터베이스보다 훨씬 빠르게 데이터를 읽고 쓸 수 있어 **캐싱, 세션 관리, 실시간 메시지 저장** 등에 많이 사용됩니다.

---

## **1. Redis의 주요 특징**

|특징|설명|
|---|---|
|**초고속 속도**|데이터를 **메모리(RAM)**에 저장하여 빠름 (일반 DB보다 100~1000배 빠름)|
|**키-값 저장 방식**|`key-value` 구조로 데이터를 저장|
|**NoSQL 데이터베이스**|테이블 형식이 아닌 **JSON, 리스트, 해시, 셋(set) 등 다양한 자료구조** 지원|
|**세션 저장 가능**|로그인 세션, 캐시 데이터 저장에 유용|
|**오프라인 메시지 저장 가능**|사용자가 오프라인일 때 메시지를 저장하고, 접속하면 전달 가능|
|**Pub/Sub 지원**|**실시간 메시지 브로커** 역할 가능 (카카오톡, WhatsApp 같은 채팅 기능 구현 가능)|

---

## **2. Redis의 자료구조**

### **✅ Redis는 다양한 데이터 구조를 지원**

|자료구조|설명|예제|
|---|---|---|
|**String**|단순 문자열 저장|`SET user:1 "홍길동"`|
|**List**|여러 개의 값을 순서대로 저장|`LPUSH messages "Hello"`|
|**Hash**|JSON 같은 객체 저장|`HSET user:1 name "홍길동" age 30`|
|**Set**|중복 없는 데이터 저장|`SADD online_users user1`|
|**Sorted Set (ZSet)**|점수(랭킹)과 함께 저장|`ZADD leaderboard 100 user1`|

💡 **이 기능들을 조합하면 다양한 서비스(채팅, 캐시, 세션 관리 등)를 구축할 수 있습니다!**

---

## **3. Redis를 활용하는 주요 사례**

### **✅ (1) 캐싱 (Cache)**

- **예시:** 자주 사용되는 데이터를 Redis에 저장해 **DB 부담을 줄이고 속도를 높임**


```sh
SET user:123 "홍길동"
GET user:123  # 결과: "홍길동"

```


### **(2) 세션 관리**

- **예시:** 로그인한 사용자의 세션을 Redis에 저장하여 관리

```sh
HSET session:abc123 user_id 123 last_login 2024-03-11
```

### **(3) 오프라인 메시지 저장**

- **예시:** 사용자가 오프라인이면 메시지를 Redis에 저장 후 로그인 시 전달
```sh
RPUSH offline:user123 "안녕!"
```

### **(4) 실시간 채팅 (Pub/Sub)**

- Redis는 **Pub/Sub(발행-구독) 모델**을 지원하여 **실시간 메시지 브로커** 역할을 수행할 수 있음

```sh
PUBLISH chat "user123: 안녕!"

```


## **. Redis 설치 및 사용 방법**

### ✅ **1) Redis 설치 (Windows, Mac, Linux)**

#### 📌 **Windows**

1. [Redis 공식 다운로드](https://github.com/microsoftarchive/redis/releases)에서 Redis 설치
2. `redis-server.exe` 실행하여 서버 시작
3. `redis-cli.exe` 실행하여 Redis 명령어 테스트

#### 📌 **Mac (Homebrew 사용)**

```sh
brew install redis
brew services start redis
redis-cli
```


📌 **Linux (Ubuntu 기준)**

```sh
sudo apt update
sudo apt install redis-server -y
sudo systemctl start redis
redis-cli
```


**2) Redis 기본 명령어**


```sh
SET key "value"   # 데이터 저장
GET key           # 데이터 가져오기
DEL key           # 데이터 삭제
EXPIRE key 60     # 60초 후 데이터 자동 삭제 (TTL 설정)
LPUSH messages "Hello"  # 리스트에 값 추가
LRANGE messages 0 -1    # 리스트 값 조회
```


## **5. Redis와 MySQL의 차이**

|**비교 항목**|**Redis**|**MySQL (RDBMS)**|
|---|---|---|
|**저장 방식**|**메모리(RAM)**|**디스크(HDD/SSD)**|
|**데이터 구조**|Key-Value|테이블(관계형 DB)|
|**속도**|**매우 빠름 (ms 단위)**|비교적 느림 (쿼리 실행 필요)|
|**영구 저장 여부**|기본적으로 비영구적 (옵션으로 영구 저장 가능)|영구 저장|
|**사용 사례**|캐싱, 실시간 데이터, 채팅, 세션|트랜잭션 처리, 관계형 데이터|

💡 **즉, MySQL은 데이터 저장을, Redis는 빠른 데이터 처리(캐싱, 세션, 메시징)용으로 활용!** 🚀

---

## **6. Redis를 Java(Spring)에서 사용하는 방법**

✅ **Spring 프로젝트에서 Redis를 사용하려면?**

### **1) `pom.xml`에 Redis 의존성 추가**



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.3.1</version>
</dependency>
```


2) Redis 설정 추가 (`application.properties`)


```properties
spring.redis.host=localhost
spring.redis.port=6379
```


3) Redis 활용 예제 (Java 코드)


```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Service;

@Service
public class RedisService {
    @Autowired
    private StringRedisTemplate redisTemplate;

    public void saveMessage(String userId, String message) {
        redisTemplate.opsForList().rightPush("offline:" + userId, message);
    }

    public String getMessage(String userId) {
        return redisTemplate.opsForList().leftPop("offline:" + userId);
    }
}
```

✅ **이제 Spring에서 Redis를 활용하여 오프라인 메시지를 저장하고 불러올 수 있음!** 🚀