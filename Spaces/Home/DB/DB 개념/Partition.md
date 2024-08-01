
오라클에서 분석함수를 사용할 때 PARTITION BY를 사용하여 그룹으로 묶어서 연산을 할 수 있다. GROUP BY 절을 사용하지 않고, 조회된 각 행에 그룹으로 집계된 값을 표시할 때 OVER 절과 함께 PARTITION BY 절을 사용하면 된다.


![[Pasted image 20240801175910.png]]

위의 예제를 보면 데이터를 조회한 각 행에 분석함수로 집계한 값을 추가로 각 행에 표시하며, 조회된 데이터는 GROUP BY 절을 사용하지 않았기 때문에 데이터가 변형되지 않는다. 집계된 값은 GROUP BY 절을 사용할 때와 동일한 값이며, 분석함수를 사용하지 않고 값을 표시할 때는 서브 쿼리를 사용하여 해당 값을 표시해야 하기 때문에 쿼리문이 복잡해진다.

|   |
|---|
|**분석함수(**[칼럼]**) OVER(PARTITION BY** 칼럼1, 칼럼2... [ORDER BY 절] [WINDOWING 절]**)**|

분석함수를 사용할 때는 OVER 절을 함께 사용해야 하며, OVER 절 내부에 PATITION BY 절을 사용하지 않으면 쿼리 결과 전체를 집계하며 PARTITION BY 절을 사용하면 쿼리 결과에서 해당 칼럼을 그룹으로 묶어서 결과를 표시한다.

### 집계 함수 사용

#### SUM 함수


```SQL
SELECT empno , 
	   ename , 
	   job ,  
	   sal , 
	   SUM(sal) OVER(PARTITION BY job) 
FROM emp WHERE job IN ('MANAGER', 'SALESMAN') ORDER BY job
```



![[Pasted image 20240801180007.png]]


조회된 결과의 직군(job) 별로 합산된 급여(sal) 값을 각행에 표시한다.

**집계 분석함수 : COUNT, MAX, MIN, SUM, AVG**

#### MAX 함수

```SQL
SELECT empno , 
       ename , 
       job , 
       sal , 
       MAX(sal) OVER(PARTITION BY job) 
FROM emp WHERE job IN ('MANAGER', 'SALESMAN') ORDER BY job
```


조회된 결과의 직군(job) 별 급여(sal) 최댓값을 각행에 표시한다.

### 순위 함수 사용

#### ROW_NUMBER 함수

```SQL
SELECT empno , 
	   ename , 
	   job , 
	   sal , 
	   ROW_NUMBER() OVER(PARTITION BY job ORDER BY sal) AS rn 
FROM emp WHERE job IN ('MANAGER', 'SALESMAN') ORDER BY job
```

![[Pasted image 20240801180126.png]]



조회된 결과에서 직군(job) 별 급여(sal)가 낮은 순으로 순번을 표시한다.

급여가 동일한 경우 또 다른 기준을 부여하고 싶을 때에는 ORDER BY 절에 추가로 칼럼을 추가한다.

(예, ORDER BY sal, empno)

**순위 분석함수 : ROW_NUMBER, RANK, DENSE_RANK**

#### RANK 함수


```SQL
SELECT empno , 
	   ename , 
	   job , 
	   sal , 
	   RANK() OVER(PARTITION BY job ORDER BY sal) AS rnk
FROM emp WHERE job IN ('MANAGER', 'SALESMAN') ORDER BY job
```


![[Pasted image 20240801180158.png]]

RANK 함수를 사용하여 순위를 부여할 때는 동일한 급여(sal)인 경우 동일한 순위를 표시한다.

### 여러 개의 칼럼을 사용하여 그룹화


```SQL
SELECT empno , 
	   ename , 
	   job , 
	   sal , 
	   RANK() OVER(PARTITION BY job ORDER BY sal) AS rnk
FROM emp WHERE job IN ('MANAGER', 'SALESMAN') ORDER BY job
```


![[Pasted image 20240801180233.png]]

여러 개의 칼럼을 그룹화하고 싶을 때에는 PARTITION BY 절 뒤에 칼럼을 추가로 부여하면 된다.

|        |                                                  |
| ------ | ------------------------------------------------ |
| **구분** | **분석함수**                                         |
| 집계     | COUNT, MAX, MIN, SUM, AVG                        |
| 순위     | ROW_NUMBER, RANK, DENSE_RANK                     |
| 순서     | FIRST_VALUE, LAST_VALUE, LAG, LEAD               |
| 통계     | STDDEV, VARIANCE                                 |
| 비율     | RATIO_TO_REPORT, CUME_DIST, PERCENT_RANK, NTITLE |












---
출처 - https://gent.tistory.com/442
