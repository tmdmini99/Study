DECODE 함수는 오라클 쿼리에서 가장 많이 사용하는 함수 중 하나이다. 표준 SQL 함수가 아니라서 사용을 꺼려하기도 하지만 잘 사용하면 아주 편하기 때문에 유용하다. 최근에는 CASE WHEN 구문 사용을 많이 권장하기도 한다.

DECODE 함수는 프로그래밍에서의  if else 와 비슷한 기능을 수행한다. 간단한 사용방법은 아래와 같다.

예) **DECODE(컬럼, 조건1, 결과1, 조건2, 결과2, 조건3, 결과3..........)**



![[DECODE1.png]]


```sql
WITH temp AS ( 
	SELECT 'M' gender 
	FROM dual UNION ALL 
	SELECT 'F' gender 
	FROM dual UNION ALL 
	SELECT 'X' gender 
	FROM dual ) 
SELECT gender , 
	   DECODE(gender, 'M', '남자', 'F', '여자', '기타') gender2 
FROM temp
```


### 사용 예제



![[DECODE2.png]]

**▲** ELSE 부분은 생략이 가능하다. 해당 조건이 없으면 NULL


![[DECODE3.png]]



### 활용 예제

![[DECODE4.png]]

![[DECODE5.png]]


![[DECODE6.png]]

 **▲** NVL2 함수처럼 NULL 값을 체크 할 수 있다





![[DECODE7.png]]

▲ 조건이 많을 경우 줄바꿈을 하여 쿼리를 작성할 것을 권장한다


![[DECODE8.png]]

▲ DECODE 함수 내부에 또 다른 DECODE 함수를 사용할 수 있다


![[DECODE9.png]]


▲ 월별, 일별 통계를 산출하거나, 행을 열로 바꿀때 유용하다







---
출처 - https://gent.tistory.com/227
