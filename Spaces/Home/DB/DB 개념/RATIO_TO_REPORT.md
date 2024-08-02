오라클에서 전체 합계 대비 비율 또는 백분율을 구하기 위해서는 **RATIO_TO_REPORT** 함수를 이용하면 된다. RATIO_TO_REPORT 이용하면 비율이 반환되는데, 여기에 100을 곱하면 백분율(%)로 바꿀 수 있다.

#### 비율 구하기

```SQL
SELECT ename
     , sal
     , RATIO_TO_REPORT(sal) OVER() AS sal_ratio
  FROM emp
 WHERE job = 'MANAGER'
```


![[RATIO_TO_REPORT1.png]]

조회된 전체 급여(sal) 합계 대비 해당 행의 비율이 반환된다.

#### 백분율 구하기

```SQL
SELECT ename
     , sal
     , ROUND(RATIO_TO_REPORT(sal) OVER(), 2) * 100 || '%'  AS sal_rate
  FROM emp
 WHERE job = 'MANAGER'
```

![[RATIO_TO_REPORT2.png]]

비율에서 소수점 둘째자리 자른다음 100을 곱하면 백분율로 변환된다.

#### GROUP BY  비율 구하기

```SQL
SELECT job
     , SUM(sal) AS total_sal
     , RATIO_TO_REPORT(SUM(sal)) OVER() AS sal_ratio
  FROM emp
 GROUP BY job 
```


![[RATIO_TO_REPORT3.png]]

그룹함수를 사용한 컬럼인 경우 RATIO_TO_REPORT 함수 인자에도 그룹함수 값을 대입해야 한다.

#### ROLLUP 백분율 구하기

```SQL
SELECT DECODE(GROUPING(job), 1, '합계', job) AS job
     , SUM(total_sal)                        AS total_sal
     , ROUND(SUM(sal_ratio), 2) * 100 || '%' AS sal_ratio
  FROM (
         SELECT job
              , SUM(sal) AS total_sal
              , RATIO_TO_REPORT(SUM(sal)) OVER() AS sal_ratio
           FROM emp
          GROUP BY job 
       )
 GROUP BY ROLLUP(job)
```

![[RATIO_TO_REPORT4.png]]



ROLLUP 함수를 사용하여 합계를 구하고 싶을 경우, SELECT 문을 다시한번 감싸서 결과를 산출하면 된다.

#### PARTITION BY 사용하여 백분율 구하기

```SQL
SELECT ename
     , job 
     , sal
     , ROUND(RATIO_TO_REPORT(sal) OVER(PARTITION BY job), 2) * 100 || '%' AS sal_rate
  FROM emp
 WHERE job IN ('ANALYST', 'MANAGER')
```

![[RATIO_TO_REPORT5.png]]

OVER 함수에 PARTITION BY를 사용하면 해당 컬럼의 그룹별 비율이 계산된다.