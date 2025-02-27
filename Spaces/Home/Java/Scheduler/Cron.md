



## **- Cron이란?**

유닉스 계열 컴퓨터 운영 체제의 시간 기반 작업 스케줄러이며, 반복 작업을 예약하는 데 가장 적합합니다.   
Cron의 작업은 주어진 일정에 따라 주기적으로 실행되도록 셸 명령을 지정하는 구성 파일 crontab(cron 테이블)에 의해 구동됩니다.

crontab 파일의 각 줄은 작업을 나타내며 다음과 같습니다.

> # ┌───────────── 분 (0 - 59)   
> # │ ┌───────────── 시간 (0 - 23)   
> # │ │ ┌───────────── 요일(1 - 31)   
> # │ │ │ ┌───────────── 월(1 - 12)   
> # │ │ │ │ ┌───────────── 요일(0 - 6)(일요일~토요일,   
> # │ │ │ │ 7은 일부 시스템에서 일요일이기도 함)   
> # │ │ │ │ │   
> # │ │ │ │ │   
> # *   *    *   *   * <실행 명령>   
> 여기서 #은 주석 표시로 한 줄에 단독으로 있어야 합니다.

CRON 형식의 중 일부는 패턴 시작 부분에 초(sec) 필드도 있습니다.   
이 경우 CRON 표현식은 6개 또는 7개의 필드로 구성된 문자열입니다.

## 리눅스 / 유닉스 크론 표현식

|분|시|일|월|요일|
|---|---|---|---|---|
|0-59|0-23|1-31 / ?|1-12|0-6 / ?|

## 스프링 스케줄러 / 쿼츠 크론 표현식

|초|분|시|일|월|요일|연도|
|---|---|---|---|---|---|---|
|0-59|0-59|0-23|1-31 / ?|1-12|0-6 / ?|생략가능|

## 주의점

- 월은 0-11이 아닌 1-12 입니다.
- 요일은 0:일요일 ~ 6:토요일 이지만 7도 일요일로 되어있습니다.
    하지만 일관성을 위해 일요일을 한쪽으로 쓰는걸 권장합니다.

# 크론 표현식 예제

#### 기본적인 표현식은 같으나 아래부터는 스프링 기반으로 설명하도록하겠습니다.

예를들어 2016년 11월 20일 오후 2시(14시) 10분 36초에 실행하려고 합니다.
자바기반 yyyy-MM-dd HH:mm:ss 로 표현하면 2016-11-20 14:10:36 입니다.
크론 표현식으로 표현한다면 아래와 같습니다.

```
36 10 14 20 11 ? 2016
```

**여기서 보이는 ? 는 설정값 없음으로 오직 일[day-of-month:1-31], 요일[day-of-week:0-6]에서만 사용 가능합니다.**
좀 더 응용해서 매년 11월 20일 오후 2시(14시) 10분 36초 에 실행하려고한다면 아래와 같이 표현할 수 있습니다.

```
36 10 14 20 11 ? *
```

혹은

```
36 10 14 20 11 ?
```

스프링 스케줄러와 쿼츠에서는 연도를 생략할 수 있습니다.
또한 *는 모든조건 즉, 와일드카드입니다.
그럼 매일 오후 2시(14시) 10분 36초 에 실행시키는 식을 써보도록하겠습니다.

```
36 10 14 * * ?
```

그럼 다시 매일 새벽 3시 정각에 실행시키려면 어떻게 표현해야할까요?

```
* * 3 * * ?
```

위처럼 설정하면 새벽 3시부터 새벽 4시 전까지 1초마다 실행될 것입니다.
왜냐면 모든초 모든분 3시 모든일... 이기 때문입니다.

```
0 0 3 * * ?
```

그래서 위처럼 설정해야 새벽 3시 0분 0초에 실행됩니다.
그럼 새벽 3시부터 4시전까지 10분 부터 5분마다 도는 크론을 설정해보도록하겠습니다.

```
0 10/5 3 * * ? [시작시간]/[단위]
```

이렇게 3시 10분 15분 20분 25분 30분 45분 50분 55분 이 실행됩니다.
일반적으로 이런단위 배치는 정각부터 실행됩니다. 그래서 보통은 0/단위를 씁니다.
그럼 10분마다 도는 크론을 설정해보겠습니다.

```
0 0/10 * * * ?
```

이렇게 설정하면 [0분 0초], [10분 0초], [20분 0초], [30분 0초], [40분 0초], [50분 0초] 이렇게 돌게 됩니다.
그밖에도 아래와 같은 표현식들이 있습니다.

# 크론 표현식 옵션

```
? : 조건없음 [일, 요일 에서만 사용가능]
```

```
* : 모든 조건에서 참시작시간/단위 (예 0/5) : 해당 시작시간부터 해당 단위때 참시작범위-끝범위 (예 3-5) : 예제(3-5)는 3에서 5까지 (3, 4, 5) 조건일때 참.x,y,z... (예 1,3,5) : 예제(1,3,5) 1,3,5 일때만 참.
```

```
L : [일, 요일 에서만 사용가능]- 일에서 사용하면 : 예(L) 마지막 날짜입니다. 예를들어 1월이라면 31일 2월이라면 윤년에 따라 28혹은 29일 4월이라면 30일에 참.- 요일에서 사용하면 : 예(6L) 6은(토요일) 마지막 토요일에 실행됩니다. 마지막주가 토요일이 없다면 그전주 토요일에 참.
```

```
W : [일에서만 사용가능]- 가장 가까운 평일(월~토)를 찾습니다.- 15W 라고 설정했다면 15일이 월~금 범위라면 해당 날짜에서 참.- 15W 15일이 토요일이라면 가장 가까운 금요일인 14일에 참.- 15W 15일이 일요일이라면 가장 가까운 월요일인 16일에 참.
```

```
# : [요일에서만 사용가능]- 예를들어 3#2 라고 썻다면 (수요일#2번째주)라는 의미가 됩니다.- 즉 2번째주의 수요일에 참이 됩니다.
```



---

예를 들어 11시부터 10마다 스케줄을 동작시키는 로직을 Spring boot java에서 구현하면 아래와 같이 출력하게 됩니다.    
  
<application.yml>

```java
cron:  schedule: 0 10/10,0 11 * * ?​
```

\<Controller>

```java
import org.slf4j.Logger;import org.slf4j.LoggerFactory;import org.springframework.stereotype.Component;import org.springframework.scheduling.annotation.Scheduled; @Componentpublic class SchedulerController {    private final Logger LOGGER = LoggerFactory.getLogger(SchedulerController.class);    @Scheduled(cron="${cron.schedule}")public void scheduler() {        LOGGER.info("schedule run");    }}
```

\<console>

```java
...[2022-01-19] [11:00:00.000] [INFO] schedule run[2022-01-19] [11:10:00.000] [INFO] schedule run[2022-01-19] [11:20:00.000] [INFO] schedule run...
```

---

여기서 cron표현식을  '0 10/10,0 11 * * ?'로 지정해 주었는데요   
어떤 원리로 돌아가는걸까요?? 

그 비밀은 특수기호에 있습니다. **특수 기호**에 대해 설명 드리겠습니다. 

## **- 특수 기호**

**쉼표(,)**

> 쉼표는 목록의 항목을 구분하는 데 사용됩니다.    
> 예를 들어, 5번째 필드(요일)에 "MON, WED, FRI"를 사용하면 월요일, 수요일, 금요일을 의미합니다.

**대시(-)**

> 대시는 범위를 정의합니다.    
> 예를 들어, 2000–2010은 2000년에서 2010년(포함) 사이의 매년을 나타냅니다.

**퍼센트(%)**

> 백슬래시(\)로 이스케이프 되지 않는 한 퍼센트(%)는 개행 문자로 변경되고, 처음 % 이후의 모든 데이터는 명령어에 표준 입력으로 전송됩니다.

**비표준 문자**    
다음은 비표준 문자이며 Quartz Java 스케줄러와 같은 일부 cron구현에만 존재합니다.

**L**

> 'L'은 "마지막"을 의미합니다.    
> 요일 필드에 사용하면 해당 월의 "마지막 금요일"(" 5L ")과 같은 구문을 지정할 수 있습니다.    
> day-of-month 필드에서는 해당 월의 마지막 날을 지정합니다.

**W**

> 요일 필드에는 'W' 문자를 사용할 수 있습니다.    
>   
> 이 문자는 주어진 요일에 가장 가까운 요일(월요일-금요일)을 지정하는 데 사용됩니다.    
> 예를 들어 " 15W "가 day-of-month 필드의 값으로 지정된 경우 의미는 "매월 15일에 가장 가까운 요일"입니다.    
>   
> 따라서 15일이 토요일이면 14일 금요일에 트리거가 실행됩니다.    
> 15일이 일요일이면 16일 월요일에 트리거가 실행됩니다.    
> 15일이 화요일이면 15일 화요일에 실행됩니다.    
>   
> 그러나 "1W"가 day-of-month의 값으로 지정되고 1일이 토요일인 경우 트리거는    
> 월의 경계를 '점프'하지 않기 때문에 3일 월요일에 실행됩니다.    
> 'W' 문자는 날짜가 요일의 범위나 목록이 아닌 1일인 경우에만 지정할 수 있습니다.

**해시(#)**

> '#'는 주간 필드에 허용되며, 1과 5 사이의 숫자가 뒤따라야 합니다.    
> 그것은 특정 달의 "두 번째 금요일"과 같은 구문을 지정할 수 있습니다.    
> 예를 들어, 요일 필드에 "5#3"을 입력하면 매월 세 번째 금요일에 해당합니다.

**물음표(?)**

> 크론이 오전 8시 25분에 시작되어 다시 시작될 때까지 매일 이 시간에 실행될 경우.   
> 크론 데몬의 시작 시간으로 "?"를 대체하므로,    
> cron 데몬의 시작 시간으로 업데이트되므로 cron이 오전 8시 25분에 시작되면    
> 다시 시작될 때까지 매일 이 시간에 실행됩니다. '? ? * * * *''25 8 * * * *'

**슬래시(/)**

> vixie-cron에서는 슬래시를 범위와 결합하여 단계 값을 지정할 수 있습니다.    
> 예를 들어 분 필드의 '*/5'는 5분마다를 나타냅니다.    
> 더 자세한 POSIX 형식 5,10,15,20,25,30,35,40,45,50,55,00의 줄임말입니다. 

**H**

> 'H'는 Jenkins 연속 통합 시스템에서 "해시" 값이 대체됨을 나타내기 위해 사용됩니다.    
> 따라서 매시 정시 20분을 의미하는 '20 * * * *'와 같은 고정된 숫자 대신 'H * * * *'는 각 작업에 대해    
> 지정되지 않았지만 불변한 시간에 작업이 매 시간 수행됨을 나타냅니다.    
> 이를 통해 모든 작업을 동시에 시작하고 자원을 경쟁하기보다는 시간에 따라 분산할 수 있습니다.

## **- 요약**

정리하자면 cron 표현식 형태는 아래와 같이 표현할 수 있습니다. 

> <초> <분> <시> <일> <월> <요일> <년>

| 필드 | 필수 | Allowed values | 허용되는 값 | 참고 |
| ---- | ---- | ---- | ---- | ---- |
| Seconds(초) | No | 0–59 | * , - / |  |
| Minutes(분) | Yes | 0–59 | * , - / |  |
| Hours(시) | Yes | 0–23 | * , - / |  |
| Day of month(일) | Yes | 1–31 | * , - ? / L W | ? L W 일부 구현에서만 |
| Month(월) | Yes | 1–12 또는 JAN–DEC | * , - / |  |
| Day of week(요일) | Yes | 0–6 또는 SUN–SAT | * , - ? / L # | ? L # 일부 구현에서만 |
| Year(년) | No | 1970–2099 | * , - / | 이 필드는 표준 cron표현시 지원되지 않음 |






---
참조 - https://ohdaldal.tistory.com/33

https://string.tistory.com/122


https://javawork.tistory.com/entry/Job-Scheduler-%ED%81%AC%EB%A1%A0-Cron-%EA%B3%BC-%ED%81%AC%EB%A1%A0-%ED%91%9C%ED%98%84%EC%8B%9D