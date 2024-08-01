
PIVOT 기능을 이용하면 DECODE의 복잡하고 비직관적인 코드를 조금 더 직관적으로 작성할 수 있습니다.

 PIVOT 기능을 사용하더라도 PIVOT을 할 컬럼을 미리 정의를 해햐한다.

![[PIVOT1.png]]

```SQL
SELECT *  
  FROM ( 피벗 대상 쿼리문 )  
 PIVOT ( 그룹합수집계컬럼) FOR 피벗컬럼 IN (피벗컬럼값 AS 별칭 ... )
```

## PIVOT 사용법

### 직군별, 월별 입사 건수

```SQL
SELECT * 
FROM ( 
	SELECT job , TO_CHAR(hiredate, 'FMMM') || '월' hire_month 
	FROM emp ) 
PIVOT ( COUNT(*) FOR hire_month IN ('1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월') )
```

피벗 컬럼 값(1월, 2월, 3월 ...)은 한번 지정하면 해당 값이 존재하지 않아도 해당 컬럼이 표시된다.

기본적인 방법으로 피벗 컬럼 값을 동적으로 바꿀 수는 없다. 해당 값을 동적으로 바꾸기 위해서는 동적 쿼리 등 다른 편법을 사용해야 한다.

```SQL
TO_CHAR('2020-09-16', 'MM') → '09'

TO_CHAR('2020-09-16', 'FMMM') → '9'
```


### 직군별, 부서코드별 급여 합계 (피벗컬럼 별칭 사용)

```SQL
SELECT job , 
	   d1 , 
	   d2 , 
	   d3 
FROM ( 
	    SELECT job , 
		        deptno , 
		        sal 
		FROM emp ) 
PIVOT ( SUM(sal) FOR deptno IN ('10' AS d1, '20' AS d2, '30' AS d3) )
```

피벗 컬럼 값에 별칭(d1, d2, d3 ...)을 지정하면 SELECT 절에서 해당 별칭을 컬럼처럼 사용할 수 있다.


### 사용예제

![[PIVOT2.png]]





![[PIVOT3.png]]





![[PIVOT4.png]]







---
출처 - https://gent.tistory.com/42


https://thinpig-data.tistory.com/entry/%ED%94%BC%EB%B4%87%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-feat-PIVOT-UNPIVOT