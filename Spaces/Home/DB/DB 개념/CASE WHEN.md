오라클에서 if 문과 비슷한 기능을 하는 DECODE 함수가 있다. 그러나 DECODE 함수는 조건이 많아지면 가독성이 떨어지고 복잡해지며, 가장 큰 문제는 오라클 SQL에서만 사용할 수 있는 비표준 함수이다.

오라클에서 DECODE 함수를 대체할 수 있는 기능이 CASE 표현식이며 가독성이 좋고 더 많은 기능을 제공한다. 조건이 복잡한 경우 DECODE 함수 보다 CASE 표현식을 사용할 것을 권장한다.



![[CASE WHEN1.png]]



#### If 문 방식

```sql
SELECT ename
     , deptno
     , CASE WHEN deptno = '10' THEN 'New York'
            WHEN deptno = '20' THEN 'Dallas'
            ELSE 'Unknown'
       END AS loc_name
  FROM emp
 WHERE job = 'MANAGER'
```

CASE 표현식에서 **ELSE 부분은 생략**이 가능하며, 만족하는 조건이 없으면 **NULL**을 리턴한다. CASE 표현식은 SELECT 절, WHERE 절, PL/SQL 등 많은 부분에서 사용이 가능하다.




#### Switch 문 방식

```sql
SELECT ename
     , deptno
     , CASE deptno 
            WHEN 10 THEN 'New York'
            WHEN 20 THEN 'Dallas'
            ELSE 'Unknown'
       END AS loc_name
  FROM scott.emp
 WHERE job = 'MANAGER'
```


CASE 표현식은 C, JAVA의 Swith문과 비슷한 방식으로 사용이 가능하다. CASE 뒤에 비교할 컬럼을 입력하고 WHEN 뒤에 값을 입력해 놓으면 된다. 단순 값만 비교할 때는 조금 더 쿼리문을 단순하게 표현할 수 있다.



### 사용 예제

**예제 1 - 일반적인 CASE 표현식**


![[CASE WHEN2.png]]


**예제 2 - ELSE를 생략 후 만족하는 조건이 없으면 NULL 리턴**


![[CASE WHEN3.png]]


**예제 3 - 비교 연산자, 범위 연사자 등 사용이 가능**

![[CASE WHEN4.png]]

**예제 4 - WHERE 절에 사용 가능**


![[CASE WHEN5.png]]


**예제 5 - 오라클 내장 함수를 조건으로 사용 가능**



![[CASE WHEN6.png]]


**예제 6 - 사용자 정의 함수를 조건으로 사용 가능**

![[CASE WHEN7.png]]


**예제 7 - THEN 절에서 중첩 CASE 등 추가 연산 작업 가능**

![[CASE WHEN8.png]]

### CASE WHEN 여러개 칼럼 조건 부여 (다중 칼럼)

```sql
SELECT ename
     , job
     , deptno
     , sal
     , CASE WHEN job = 'ANALYST'  AND deptno = 20 AND sal >= 3000 THEN 'CASE 1'
            WHEN job = 'MANAGER'  AND deptno = 10 AND sal >= 2000 THEN 'CASE 2'
            WHEN job = 'SALESMAN' AND deptno = 30 AND sal >= 1500 THEN 'CASE 3'
       END AS case_result
  FROM emp
 WHERE job IN ('ANALYST', 'MANAGER', 'SALESMAN')
```


![[CASE WHEN9.png]]


여러개의 칼럼을 비교하는 조건을 부여해야 할 경우 AND 연산자를 사용하여 조건을 부여하면 된다.

모든 칼럼의 AND 조건을 만족해야 해당 값을 반환한다.

#### OR 연산자를 사용하여 여러개의 칼럼 조건 부여

```sql
SELECT ename
     , job
     , deptno
     , sal
     , CASE WHEN job = 'ANALYST'  OR deptno = 20 OR sal >= 3000 THEN 'CASE 1'
            WHEN job = 'MANAGER'  OR deptno = 10 OR sal >= 2000 THEN 'CASE 2'
            WHEN job = 'SALESMAN' OR deptno = 30 OR sal >= 1500 THEN 'CASE 3'
       END AS case_result
  FROM emp
 WHERE job IN ('ANALYST', 'MANAGER', 'SALESMAN')
```


![[CASE WHEN10.png]]


여러개의 칼럼을 OR 연산자로 조건을 부여할 수 있으며, OR 조건 중 하나만 만족해도 해당 값을 반환하고 WHEN~THEN 조건 탐색을 종료한다.








---
출처 - https://gent.tistory.com/311

https://gent.tistory.com/596