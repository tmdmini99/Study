## 1. GROUP BY

### 1.1 GROUP BY 이해

- 중복제거 (SELECT 의 DISTINCT와 같음)
- 집계기능
- 주요 집계함수 : SUM(합계), COUNT(건수), MIN(최소값), MAX(최대값)  
    집계함수는 GROUP BY없이 단독사용가능
- CASE는 GROUP BY절 ORDER BY절에도사용가능하다.
- CASE와함께 월별 컬럼만들기/조건별집계등이 가능
- 피벗(PIVOT) : 로우를 컬럼으로, 컬럼을 로우로 변환하는 기능

### 1.2 COUNT

- COUNT함수는 NULL을 0으로 카운트
    
- 아래 SQL문의 결과는 CNT_COL1은0, CNT_ALL은 2 > *은 ROW자체건수를 카운트
    
    > SELECT COUNT(COL1) CNT_COL1, COUNT(*) CNT_ALL  
    > FROM (  
    >           SELECT NULL COL1 FROM DUAL UNION ALL  
    >          SELECT NULL COL1 FROM DUAL  
    > )
    
- COUNT안에서 DISTINCT사용시 중복을 제거하고 COUNT  
    > COUNT(DISTINCT 컬럼)
    
- 컬럼 조합에 따른 종류수, 처음보는 형태 실무에응용해볼만한듯  
    한번이라도 로그인한고객수, 한번이라도 사용한 이력등등의 데이터 추출시
    
    > SELECT COUNT(DISTINCT T1.ORD_ST || '-' || T1.PAY_TP)  
    > FROM T_ORD T1;
    
    > SELECT COUNT(*)  
    > FROM (  
    >           SELECT DISTINCT T1.ORD_ST, T1.PAY_TP  
    >           FROM T_ORD T1  
    >          )T2
    
    > --한번이라도 로그인한적있는고객의 수  
    > SELECT COUNT(*)  
    > FROM 고객 T1  
    > WHERE EXISTS (SELECT 1  
    >                     FROM 로그인 A  
    >                    WHERE T1.고객ID = A.고객ID)
    

### 1.3 HAVING

- GRUOP BY가 수행된 결과 집합에 조건을 줄 때 사용
- WHERE절에서 처리가가능한경우 미리 처리하는것이 성능에좋다.
- WHERE절처럼 AND나 OR을 이용해 여러개의 조건 사용가능
- GROUP BY에서 정의한 컬럼은 그대로사용가능, GROUP BY에없는것은 집계함수를쓰거나 GROUP에추가해야 HAVING절 사용가능
- HAVING조건은 인라인뷰에 대한 WHER절에서 처리가능.
    
    > SELECT T0.*  
    > FROM (  
    >           SELECT T1.CUS_ID, T1.PAY_TP, SUM(T1.ORD_AMT) ORD_TTL_AMT  
    >           FROM T_ORD T1  
    >           WHERE T1.ORD_ST = 'COMP'  
    >           GROUP BY T1.CUS_ID, T1.PAY_TP  
    >           )T0  
    > WHERE T0.ORD_TTL_AMT >= 10000 --인라인뷰안에서 집계한 ORD_TTL_AMT를 FILTER  
    >      ORDER BY T0.ORD_TTL_AMT ASC;
    

## 2. ROLLUP

### 2.1 ROLLUP개요

- 대부분의 분석리포트는 소계(중간집계)와 전체합계가 필요 > ROLLUP이 가장효율적
- ROLLUP은 GROUP BY 뒤에 ROLLUP을 적어 사용
- ROLLUP(A, B, C, D)사용시 다음과 같은 데이터들이 조회된다.
    - GROUP BY 된 A+B+C+D 별 데이터
    - A+B+C별 소계 데이터
    - A+B별 소계 데이터
    - A별 소계데이터
    - 전체합계
- GROUP BY만 사용한 쿼리
    
    > SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM  
    > , T1.CUS_ID  
    > , SUM(T1.ORD_AMT) ORD_AMT  
    > FROM T_ORD T1  
    > WHERE T1.CUS_ID IN('CUS_0001','CUS_0002')  
    > AND T1.ORD_DT >= TO_DATE ('20170301','YYYYMMDD')  
    > AND T1.ORD_DT < TO_DATE('20170501','YYYYMMDD')  
    > GROUP BY TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1.CUS_ID  
    > ORDER BY TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1.CUS_ID;


![[GROUP BY1.png]]

ROLLUP사용 쿼리 결과

> SELECT TO_CHAR(T1.ORD_DT, 'YYYYMM') ORD_YM  
> , T1.CUS_ID  
> , SUM(T1.ORD_AMT) ORD_AMT  
> FROM T_ORD T1  
> WHERE T1.CUS_ID IN('CUS_0001','CUS_0002')  
> AND T1.ORD_DT >= TO_DATE ('20170301','YYYYMMDD')  
> AND T1.ORD_DT < TO_DATE('20170501','YYYYMMDD')  
> GROUP BY  
> ROLLUP (TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1.CUS_ID)  
> ORDER BY TO_CHAR(T1.ORD_DT, 'YYYYMM'), T1.CUS_ID;


![[GROUP BY2.png]]


ROLLUP의 컬럼순서

- 1. GROUP BY ROLLUP(A,B,C) : A+B+C별 소계, A+B별 소계, A별 소계, 전체합계
- 2. GROUP BY ROLLUP(B,A,C) : B+A+C별 소계, B+A별 소계, B별 소계, 전체합계
- 아래 사진에서 왼쪽결과에는 ORD_ST별 소계가있지만, 오른쪽엔없다,  
    반대로 오른쪽에는 ORD_YM별 소계가 있지만, 왼쪽에는 없다.
    
![[GROUP BY3.jpg]]


### 2.3 GROUPING

- NULL이집계된값과 실제 소계값이 헷갈리는 경우가 발생하는데, GROUPING함수는 해당컬럼이 ROLLUP처리되었으면 1, 아니면 0을 반환한다.
    
- 아래 SQL의결과를 보면 PAY_TP가 NULL인것과 WAIT의 소계를 GR_PAY_TP컬럼을 통해 알수있다
    
    > SELECT T1.ORD_ST, GROUPING(T1.ORD_ST) GR_ORD_ST  
    > ,T1.PAY_TP, GROUPING(T1.PAY_TP) GR_PAY_TP  
    > ,COUNT(*) ORD_CNT  
    > FROM T_ORD T1  
    > GROUP BY ROLLUP(T1.ORD_ST, T1.PAY_TP);
    

![[GROUP BY4.png]]

- ROLLUP된컬럼을 다른 값으로 치환할 때는 꼭 GROUPING을 사용해야 한다. 데이터 특성상 NULL 값이 없음을 확신하고 NULL 값을 그대로 치환할 수도 있지만, 혹시라도 나중에 NULL 값이 발생하면 잘못된 결과가 나오게 된다.
    
- GROUPING을 사용해 소계를 TOTAL로 표시하는 SQL
- 
![[GROUP BY5.png]]


### 2.4 ROLLUP 컬럼선택에 따른 결과 차이

![[GROUP BY6.jpg]]


- ROLLUP을 중첩괄호로사용할경우 필요한 소계만 뽑을수있다  
    EX) 전체합계만 뽑고싶을때 GROUP BY ROLLUP((A,B,C,D))  
    A,B,C,D가 하나의 단위로 ROLLUP되므로 전체합계만 결과에 추가
    
- GROUP BY A,B,C,D,E,F같이 여러 컬럼중 앞쪽 3개컬럼까지의 소계와 전체합계가 필요하다면  
    소계가 필요없는 D,E,F를 하나로 묶어버리면 된다.  
    EX) GROUP BY ROLLUP(A,B,C,(D,E,F))
    

## 3. 소계를 구하는 다른방법

### 3.1 UNION ALL

- GROUP BY, 소계별 GROUP BY, 전체금액 GROUP BY해서 다더해준다.
- 동일 테이블을 여러번 엑세스하기때문에 성능에서나빠지고, SELECT 절도맞춰야해서 번거롭다
    
    ### 3.2 카테시안 조인
    
- 잘안쓸듯
    
    ### 3.3 WITH절과 UNION ALL
    
- WITH절에 GROUP BY 결과를 정의하고 메인SQL에서 WITH절테이블을 UNION ALL
    
    ### 3.4 CUBE
    
- 조합가능한 모든 소계를 만들어낸다
- GROUP BY CUBE(A,B,C)형태로 사용
    
    ### 3.5 GROUPING SET
    
- GROUP BY GROUPING SETS () 형태로사용
- GROUP BY GROUPING SETS (()) 전체합계



**3. CUBE**

CUBE함수는 항목들 간의 다차원적인 소계를 계산합니다. ROLLUP과 달리 GROUP BY절에 명시한 모든 컬럼에 대해 소그룹 합계를 계산해줍니다.

```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY CUBE(상품ID, 월);
```



![[GROUP BY7.png]]

ROLLUP함수를 사용했을 때보다 결과가 좀 더 복잡합니다. 상품ID별 합계뿐만 아니라 월별 합계까지 한 번에 볼 수 있습니다.

```sql
GROUP BY CUBE(컬럼1, 컬럼2)
=
GROUP BY 컬럼1, 컬럼2
UNION ALL
GROUP BY 컬럼1
UNION ALL
GROUP BY 컬럼2
UNION ALL
모든 집합 그룹 결과
```

**4. GROUPING SETS**

GROUPING SETS는 특정 항목에 대한 소계를 계산하는 함수입니다. 

```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY GROUPING SETS(상품ID, 월);
```

![[GROUP BY8.png]]

앞의 ROLLUP, CUBE에 비해 훨씬 결과가 단순합니다.

ROLLUP과 CUBE는 GROUP BY 결과에 소그룹 합계와 토탈 합계를 보여주지만 GROUPING SETS는 각 소그룹별 합계만 간단하게 보여줍니다.

GROUPING SETS 함수는 각각의 컬럼으로 GROUP BY한 값을 UNION ALL 한 것과 동일한 결과를 보여줍니다.

```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY 상품ID
UNION ALL
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY 월;
```

**5. GROUPING** 

GROUPING은 직접적으로 그룹별 집계를 구하는 함수는 아니지만 위의 집계함수들을 지원하는 함수입니다.

집계가 계산된 결과에 대해서는 1의 값을 갖고 그렇지 않은 결과에 대해서는 0의 값을 갖습니다.

```sql
SELECT 
    CASE GROUPING(상품ID) WHEN 1 THEN '모든 상품ID' ELSE 상품ID END AS 상품ID,
    CASE GROUPING(월) WHEN 1 THEN '모든 월' ELSE 월 END AS 월, 
    SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY ROLLUP(상품ID, 월);
```

![[GROUP BY9.png]]

CASE WHEN문을 사용해서 맨 처음에 단순 ROLLUP함수만 썼을 때 NULL값으로 표시되었던 곳에 값을 넣어주었습니다. 집계가 계산된 결과에 대해서만 값을 넣어주면 되기 때문에 GROUPING(컬럼명)=1인 경우에만 '모든상품ID' 또는 '모든월' 값을 부여했고 0인 경우에는 원래대로 상품ID와 월을 써주었습니다.

CASE 함수와 ROLLUP 함수를 응용해서 다음과 같은 표현도 가능하다.

```sql
SELECT 
    CASE GROUPING(상품ID) WHEN 1 THEN '모든 상품ID' ELSE 상품ID END AS 상품ID,
    CASE GROUPING(월) WHEN 1 THEN '모든 월' ELSE 월 END AS 월, 
    SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY CUBE(상품ID, 월);
```


![[GROUP BY10.png]]

```sql
SELECT 
    CASE GROUPING(상품ID) WHEN 1 THEN '모든 상품ID' ELSE 상품ID END AS 상품ID,
    CASE GROUPING(월) WHEN 1 THEN '모든 월' ELSE 월 END AS 월, 
    CASE GROUPING(회사) WHEN 1 THEN '모든 회사' ELSE 회사 END AS 회사,
    SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY GROUPING SETS((상품ID, 월), 회사);
```


![[GROUP BY11.png]]


이는 CUBE 함수, GROUPING SETS 함수에서도 마찬가지로 응용해볼 수 있다.





---
참조 - https://velog.io/@tothek/GROUP-BY%EC%99%80-ROLLUP
https://for-my-wealthy-life.tistory.com/44

