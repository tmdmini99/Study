

### **1. PIVOT(Oracle, SQL Server Only)**

**피봇 연산자는 행으로 나열되어 있는 데이터를 열로 나열하여 보기 쉽게 가공하는 것**입니다.

시간순으로 차곡차곡 쌓이는 값이나 대규모 인원의 정보는 세로로 길어 한눈에 알아보기 어렵습니다.

피봇은 세로행을 가로 열로 가독성을 향상합니다.

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

### PIVOT() 여러 칼럼 수행

PIVOT을 할때 한번에 여러 칼럼을 출력하는 것도 가능하다.  
위에 쿼리문은 job별, deptno별 sal의 합을 출력해 주었는데 SAL의 평균도 같이 출력할수 있다.


```SQL

SELECT * FROM (SELECT job, deptno , sal   FROM emp )
PIVOT(
    SUM(sal) AS 합계, AVG(sal) AS 평균  FOR deptno IN ('10', '20', '30', '40')
)

```


![[PIVOT5.png]]


### **2. 피봇 집계**

피봇의 FOR 절은 대상 칼럼, IN 절은 그 칼럼에서 열로 만들 값의 목록입니다.

집계 함수는 회전한 후 각 칸에 쓸 값을 지정합니다.

그러나 여러 값이 단일 값으로 표현되어야 하기 때문에 집계 기준이 필요합니다.

AVG, SUM, MIN 등을 모두 쓸 수 있습니다.
COUNT도 가능합니다.

```SQL
SELECT * FROM tSeason2 PIVOT (COUNT(sale) FOR season IN (봄, 여름, 가을, 겨울)) pvt;

```
![[PIVOT6.png]]



PIVOT 연산자는 대상 테이블의 모든 칼럼 중 피봇 대상 칼럼만 빼고 GROUP BY 연산을 수행합니다.

다음은 도로명이나 시간을 기준으로 피봇 해보겠습니다.

```SQL

SELECT * FROM tTraffic PIVOT (SUM(traffic) FOR line IN (경부, 호남)) pvt;

SELECT * FROM tTraffic PIVOT (SUM(traffic) FOR hour IN ([1], [2], [3])) pvt;
/* SQL Server는 숫자를 [] 안에 넣어야 합니다. */
```


![[PIVOT7.png]]


통계 기준이 여러 개인 테이블은 원하는 기준으로 통계를 자유롭게 만들 수 있습니다.

피봇 대상만 빼고 남은 필드를 전부 그룹핑합니다.

만일 일부를 그룹핑에서 제외하고 싶다면, 필드 목록을 지정해주면 될 것 같지만 에러가 발생합니다.

![[PIVOT8.png]]


**이는 SELECT보다 PIVOT이 먼저 처리되기 때문**입니다.

SELECT은 명령이고 PIVOT은 연산자여서 PIVOT의 우선순위가 더 높습니다.

**PIVOT 연산이 먼저 이루어지면 car, traffic 필드가 '승용차', '트럭' 필드로 바뀌기 때문에,**

**SELECT의 명령 필드인 car, traffic은 존재하지 않게 되며 결국 출력할 수 없습니다.**

이 문장에서 피봇 대상은 tTraffic 전체입니다.

시간으로 그룹핑하지 않으려면 피봇 하기 전에 hour를 제외해야 하며,

그러려면 tTraffic 전체 테이블 대신 쓸 서브 쿼리가 필요합니다.

**피봇 대상 테이블을 인라인 뷰로 정의한 후 피봇 하고 그 결과를 출력해야 합니다.**


```SQL
SELECT 출력대상 FROM
(
	SELECT 피봇대상 FROM tTraffic
) prepvt
PIVOT (SUM(집계할필드) FOR 피봇필드 IN (열로 만들 값)) pvt;
```

위와 같이 서브 쿼리에서 원하는 필드만 선정하여 prepvt 인라인 뷰를 정의하고,

이를 피봇 한 후 외부 쿼리에서 결과를 출력합니다.

이제 인라인 뷰의 필드 목록에서 hour를 생략해보겠습니다.

```SQL
SELECT * FROM
(
	SELECT line, car, traffic FROM tTraffic
) prepvt
PIVOT (SUM(traffic) FOR car IN (승용차, 트럭)) pvt;
```



![[PIVOT9.png]]


인라인 뷰로 피봇 대상을 선택하는 대신 전체 테이블을 피봇 한 후 GROUP BY를 따로 수행할 수도 있습니다.

```SQL
SELECT line, SUM(승용차), SUM(트럭) FROM tTraffic
PIVOT(SUM(traffic) FOR car IN (승용차, 트럭)) pvt
GROUP BY line;
``````


![[PIVOT10.png]]

서브 쿼리가 없어 구문이 짧고 직관적이지만, SELECT 절에서 집계 함수를 써야 하는 불편함이 있습니다.

**두 구문은 피봇 할 때 원하는 기준으로 그룹핑하면서 집계까지 할 것인지,**

**아니면 일단 해 놓고 그룹핑하면서 집계를 직접 할 것인지가 다릅니다.**

피봇 대상만 잘 지정하면 PIVOT 연산자가 그룹핑, 집계를 알아서 다 하도록 되어 있어 편리하고

한 번에 처리하니 속도도 빠릅니다.

외부 쿼리에서 일부 필드를 생략하면 해당 필드는 출력 대상에서 제외합니다.

### **4. 피봇의 활용**

![[PIVOT11.png]]

그냥 적재해둔 위와 같은 테이블이 존재합니다.

도시별로 인구밀도를 계산하고 싶지만, 위의 테이블로는 불가능합니다.

이럴 때 피봇을 활용하면 계산이 가능해집니다.


![[PIVOT12.png]]


```SQL
SELECT name, ROUND(popu * 10000 / area, 2) AS 인구밀도 FROM
(
	SELECT * FROM tCityStat
	PIVOT (MAX(value) FOR attr IN ('area' AS area, 'popu' AS popu)) pvt
) A;
```


![[PIVOT13.png]]


위와 같이 인라인 뷰를 통해 피봇 테이블을 부여하면,

원하는 값을 출력할 수 있습니다.

### **5. UNPIVOT**

**UNPIVOT은 이름과 같이 피봇의 반대 동작을 수행**합니다.

피봇이 값을 열로 바꾸는데 비해 언피봇은 열을 값으로 변환하여 레코드에 기록합니다.

```SQL
UNPIVOT (값컬럼 FOR 대상컬럼 IN (언피봇 대상 컬럼 목록))
```


![[PIVOT14.png]]


위와 같은 피봇팅 된 테이블을 언피봇 해보겠습니다.

```SQL
SELECT * FROM tSeasonPivot
UNPIVOT (sale FOR season IN (봄, 여름, 가을, 겨울)) unpvt;
```


![[PIVOT15.png]]


![[PIVOT16.png]]



언피봇한 결과가 동일할 수도 있지만,

중복된 레코드가 존재한다면 값이 달라질 수 있습니다.



![[PIVOT17.png]]
![[PIVOT18.png]]


이미 '짬뽕', '겨울'을 기준으로 SUM 집계를 했기 때문에 다시 UNPIVOT 하더라도 원래대로 돌아가지 않습니다.

**UNPIVOT은 PIVOT과 동작만 반대일 뿐 완전한 반대 연산자는 아닙니다.**




### 사용예제

![[PIVOT2.png]]





![[PIVOT3.png]]





![[PIVOT4.png]]







---
출처 - https://gent.tistory.com/42


https://thinpig-data.tistory.com/entry/%ED%94%BC%EB%B4%87%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-feat-PIVOT-UNPIVOT