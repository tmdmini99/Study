```java
Create : INSERT
READ   : SELECT
UPDATE : UPDATE
DELETE : DELETE
```

### 1. SELECT

```java
- Table의 데이터를 조회(Read)
- SELECT의 결과물은 ResultSet에 보관
- ResultSet의 결과물을 보여주는 화면을 View라고 함

1. 기본 문법

SELECT     [DISTINCT] *(모든 컬럼), 컬럼명, 계산식, 함수 호출, 별칭(Alias)
FROM       테이블명
[WHERE]    조건식
[GROUP BY] 그룹으로 묶을 컬럼명
[HAVING]   그룹의 조건식
[ORDER BY] 컬럼명 정렬방식

1) DISTINCT
	- 중복제거
	- DISTINCT 컬럼명
	- SELECT 절에서 딱 한번만 사용가능

2) 별칭 (Alias)
	- 조회 결과의 컬럼명 또는 계산식, 함수결과를 대신해서 출력할 컬럼명
	- 별칭의 글자사이에 공백이나 기호문자가 포함 될때는 쌍따옴표로 감쌈
	- 테이블명도 별칭 가능

	a. 컬럼명(계산식, 함수호출) AS 별칭
	b. 컬럼명(계산식, 함수호출) 별칭
	c. 테이블명 별칭

3) 연결 연산자
	- 조회 결과를 문장 형식으로 보고 싶을 때 많이 사용
	- 컬럼명 || '문자열' || 컬럼명 || '문자열' 
	- "a"+"b" => "ab"

실행 순서(읽는 순서)
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY

--------------------------------------------------------------------------------

2. WHERE
	- WHERE 컬럼명(또는 값) 연산자 컬럼명(또는 값)

	1) 비교연산자
			>, <, >=, <= , =, != ( ^=, <> )

	2) 논리연산자
			and ,  or	

	3) NULL 연산자
			- 알수없는 DATA, 무한대
			- 0 이 X, 비어있다 X  
			- null+3 => null
		a. IS NULL     : 컬럼명 IS NULL
		b. IS NOT NULL : 컬럼명 IS NOT NULL

	4) BETWEEN (and)
			- WHERE A >= 10 and A<=100
			- WHERE 컬럼명 between 작은값 and 큰값
			- WHERE A between 10 and 100
			- WHERE A NOT BETWEEN 10 and 100

			- WHERE A>=10 and B<=100 => 컬럼이 2개이상은 between으로 표현 할 수 없음

	5) IN (OR)
			- WHERE A = 10 or A = 20
			- WHERE 컬럼명 IN (값1, 값2,...)
			- WHERE A IN (10, 20)
			- WHERE A NOT IN (10, 20)

	6) LIKE
		- 특정 문자열에 일치하는 데이터를 조회
		- WHERE 컬럼명 LIKE 검색어

		a.  '_'     : 글자수 1개, 어떤 글자든 상관 없음

				WHERE 컬럼명 LIKE '_K'    : 총 2글자중에서 K로 끝나는 것들
				WHERE 컬럼명 LIKE '_K_'   : 총 3글자중 중간에 K가 있는것들
				WHERE 컬럼명 LIKE 'K_ _'  : 총 3글자중에서 K로 시작하는 것들

				ex)JUMIN : 941225-1234567
			  ex) 1900년대에 태어난 여성만 검색
				ex) WHERE JUMIN LIKE '______-2______'

		b.  '%'     : 글자수 0개 이상, 어떤 글자든 상관 없음
				WHERE 컬럼명 LIKE '%K'     : 글자수 상관X , 뭐로 시작하든 K로 끝나는 것들
				WHERE 컬럼명 LIKE '%K%'    : 글자수 상관X , K가 포함되어 있는 모든 것들
				WHERE 컬럼명 LIKE 'K%'     : 글자수 상관X, K로 시작하는 모든 것들
        WHERE 컬럼명 LIKE '%%%'     : 글자수 상관X, 모든것

		c. ESCAPE 옵션
				- %, _ 를 문자열에 포함해서 검색하고 싶을 때

				WHERE 컬럼명 LIKE '%#%%' ESCAPE '#'
				WHERE 컬럼명 LIKE '%$%%' ESCAPE '$'

--------------------------------------------------------------------------------

3. GROUP BY
		- GROUP BY 그룹으로 묶을 컬럼명

4. HAVING
		- 그룹의 조건

SELECT DEPARTMENT_ID, SUM(SALARY)   -- 5
FROM EMPLOYEES						          -- 1
WHERE DEPARTMENT_ID >=50			      -- 2
GROUP BY DEPARTMENT_ID				      -- 3
HAVING SUM(SALARY)>=50000			      -- 4
ORDER BY SUM(SALARY) DESC;	        -- 6

--------------------------------------------------------------------------------

5. Order By
	- Table에 Data는 순서대로 저장 되지 않음
	- 조회할때 순서를 지정하고 싶을 때 사용

	1) 정렬방식
		a. 오름차순 : asc      - 작은것부터 큰것
		b. 내림차순 : desc     - 큰것부터 작은것

	2) 문법
		- ORDER BY 컬럼명 정렬방식
		- ORDER BY 컬럼의Index번호 정렬방식

		- ORDER BY 컬럼명1 정렬방식, 컬럼명2 정렬방식,...
		- ORDER BY 컬럼의Index번호 정렬방식, 컬럼의Index번호 정렬방식
			-- 컬럼명(Index번호)1로 정렬하고, 동등한 값이 있는 row들은 컬럼명(Index번호)2로                 다시 정렬
```


#### 2. INSERT

```jsx
- INSERT, UPDATE, DELETE는 Query의 결과로 성공한 ROW 갯수 만큼 숫자가 리턴.
- 0이면 실패, 1이상 양의 정수는 성공

- DB에 새로운 데이터를 추가


1. 컬럼을 나열 하는 방식
INSERT INTO 테이블명 (컬럼명1, 컬럼명2,...)
VALUES (값1, 값2,...)

- 테이블의 컬럼순서와 달라도 상관 없음
- 단, 선언한 컬럼명의 순서와 값의 순서는 일치
- NULL이 허용된 컬럼에 NULL을 INSERT 할 때 컬럼명을 생략 하고 INSERT


2. 컬럼을 생략 하는 방식
INSERT INTO 테이블명
VALUES (값1, 값2,...)

- value는 테이블의 컬럼의 순서와 일치시켜야 하고, 갯수도 같아야 함

3. INSERT 의 VALUES에 SubQuery를 이용해서 데이터를 입력 할 수 있다.
```

​

#### 3. UPDATE

```jsx
- Table에 있는 컬럼의 값을 수정 할 때

1. 문법
UPDATE 테이블명 SET 수정할컬럼명1=수정할값1, 수정할컬럼명2=수정할값, ...             : 테이블의 모든 ROW가 수정
UPDATE 테이블명 SET 수정할컬럼명1=수정할값1, 수정할컬럼명2=수정할값, ... WHERE 조건식 : 특정한 ROW 수정
```



​

#### 4. DELETE

```jsx
- Table에 있는 ROW를 삭제

DELETE 테이블명              : Table의 모든 ROW 삭제
DELETE 테이블명 WHERE 조건식 : 특정한 ROW 삭제

- MYSQL 같은 경우
- DELETE FROM 테이블명 WHERE 조건식
```

