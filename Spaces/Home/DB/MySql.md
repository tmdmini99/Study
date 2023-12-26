
```sql
show databases; -- database 조회--
select user, host from user; -- 사용자 조회--

```

###  User 생성 
```sql
create user 유저명@host identified by 비밀번호; 
create user 'user01'@'%' identified by 'user01';
create user 'test'@host identified by '1234';
```


```sql
-- host 
localhost : localhost 에서만 접근 가능 
% : 모든 외부 ip에서 접근 가능 
211.211.211.211 : 211.211.211.211 에서만 접근 가능 
211.211.211.% : 211.211.211.xxx(/24) 대역대에서만 접근 가능
```

### 권한 적용
```sql
grant all privileges on database명.* to 유저명@host; 
grant all privileges on user01.* to 'user01'@'%';


//모든 DB에 모든 권한 부여 
GRANT ALL PRIVILEGES ON *.* TO '계정아이디'@'호스트'; 
ex) grant all privileges on *.* to 'testId1'@'loalhost'; 

//특정 DB에 모든 권한 부여 
GRANT ALL PRIVILEGES ON 데이터베이스명.* TO '계정아이디'@'호스트'; 
ex) grant all privileges on board.* to 'testId1'@'loalhost'; 


//특정 DB에 특정 권한 부여 
GRANT SELECT, INSERT, UPDATE ON 데이터베이스명.* TO '계정아이디'@'호스트'; 
ex) grant select, insert, update on board.* to 'testId1'@'loalhost';
```


```
//권한 적용
FLUSH PRIVILEGES;

//권한 부여 확인
SHOW GRANTS FOR '계정아이디'@'호스트';
```

권한 부여 이후 **flush privileges** 명령어를 통해 최종적으로 권한을 적용시켜야 하며, 위 **show grants for** 명령어를 통해 권한이 부여되었는지 확인할 수도 있습니다.

### 권한 삭제
```sql

revoke {권한} privileges on {스키마}.{테이블} from {username}@{ip};

REVOKE ALL ON *.* FROM test@host;

```


### 계정 삭제

![](https://blog.kakaocdn.net/dn/bkD58K/btrPHygFlZF/u43WuxsQpkV08H1atJWfu1/img.jpg)

drop user

```sql
DROP USER '계정아이디'@'호스트';
ex) drop user 'testId1'@'localhost';

DELETE FROM USER WHERE USER = '계정아이디';
ex) delete from user where user = 'testId1';
```

계정을 삭제하고 싶은 경우에는 다음 **두 가지 명령어(drop, delete)**를 통해 삭제할 수 있습니다.



### 데이터베이스 생성
```sql
CREATE DATABASE [database name] CHARACTER SET [character set];

CREATE DATABASE Account CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

CREATE DATABASE study_db default CHARACTER SET UTF8;
--한글을 사용할 수 있는 UTF8로 문자열을 저장--

```


### 데이터베이스 선택

**USE [database name];**

```sql
예시) 
    
USE Account;
```

### 데이터베이스 삭제

**DROP DATABASE [database name];**

```sql
예시) 
    
DROP DATABASE Account;
```

### 테이블 생성

**CREATE TABLE [table name] ([column1 name][datatype], …);**

```sql
예시
 
CREATE TABLE User
(

    ID INT,

    Name VARCHAR(30),

    BirthDay DATE,
    
    Age INT

);
```

### 테이블 삭제

**DROP TABLE [table name];**

```sql
예시
 
DROP TABLE User;
```

---

### 테이블에 필드(열) 추가

**ALTER TABLE [table name] ADD [column name][datatype];**

```sql
예시) 
    
ALTER TABLE User ADD PhoneNumber INT;
```

### 테이블 필드(열) 타입 변경

**ALTER TABLE [table name] MODIFY COLUMN [column name][datatype];**

```sql
예시) 
    
ALTER TABLE User MODIFY ID VARCHAR(20);
```

### 테이블 필드(열) 삭제

**ALTER TABLE [table name] DROP [column name];**

```sql
예시) 
    
ALTER TABLE User DROP Age;
```

---

### 테이블에 레코드(행) 추가

**INSERT INTO [table name] VALUES (value1, value2, value3…);**

```sql
예시) 
    
INSERT INTO User(ID, Name, BirthDay) VALUES(1, '김태하', '1992-11-04');
```

### 테이블의 레코드(행) 선택

**SELECT * FROM [table];**

```sql
예시) 
    
SELECT * FROM User;
```

### 테이블의 레코드(행) 내용 수정

**UPDATE [table] SET [column]=[value] WHERE [condition];**

```sql
예시) 
    
UPDATE User SET Age = 30 WHERE Name = '김태하';
```

### 테이블의 레코드(행) 삭제

**DELETE FROM [table] WHERE [condition];**

```sql
예시) 
    
DELETE FROM User WHERE Name = '김태하';
```

---
###  WHERE -  특정 조건만 조회하기

```sql
-- member 테이블에서 mem_number 컬럼 값이 5이상인 데이터 조회
SELECT * FROM member 
	WHERE mem_number >= 5;
```

- WHERE절을 사용해 특정 조건에 해당하는 데이터만 조회할 수 있다.
- **관계 연산자** / **논리 연산자** 사용 가능


_논리 연산자 **AND**, **OR** 사용 가능_

```sql
SELECT TRUE OR FALSE AND FALSE;   // 1
SELECT (TRUE OR FALSE) AND FALSE; // 0
```

- 여러 조건이 필요한 경우 논리 연산자를 사용하면 된다.
- _**AND**_가 _**OR**_보다 우선 순위를 가진다.
- MySQL에서는 **&&** 나 **||** 도 사용 가능하다.

#### _**BETWEEN**  -  범위 표현식_

```sql
-- member 테이블에서 height 컬럼 값이 160이상 165이하인 데이터 조회
SELECT * FROM member 
	WHERE height between 160 and 165;
```

- _**between**_ 연산자를 이용하여 특정 범위에 해당하는 데이터를 조회할 수 있다.
- 하지만 인덱스를 사용할 수 없으므로 주의


#### _**IN  ()**  - 여러 값 매칭_

```sql
-- addr 컬럼값이 경기, 전남, 경남인 데이터 조회
SELECT * FROM member 
	WHERE addr IN('경기', '전남', '경남');

SELECT * FROM member
	WHERE addr = '경기' AND addr = '전남' AND addr = '경남';
```

- **_IN()_** 연산자를 이용하여 특정 값이 포함된 데이터를 조회할 수 있다.
- IN 연산자는 동등비교 '=' 를 여러번 수행하는 효과를 가진다. 따라서 인덱스를 최적으로 활용할 수 있다.

#### _**LIKE** -  문자열의 일부 글자 검색_

```sql
-- mem_name 컬럼 값이 '블'로 시작하는 4글자 글자 데이터 조회
SELECT * FROM member WHERE mem_name LIKE '블___';

-- mem_name 컬럼 값이 '블'로 시작하는 모든 데이터 조회
SELECT * FROM member WHERE mem_name LIKE '블%';

-- mem_name 컬럼 값에 '블'이 들어가는 모든 데이터 조회
SELECT * FROM member WHERE mem_name LIKE '%블%';
```

- 문자열의 일부 글자 검색
    - **_** : 한 글자만 매치
    - **%** : 몇 글자든 매치

#### _**서브 쿼리**_

```sql
SELECT mem_name, height 
	FROM member 
	WHERE height > (select height from member where mem_name LIKE '에이핑크');
```

- 2개의 **SQL** 문을 하나로 만듦

###  ORDER BY  -  조회된 데이터를 정렬

```sql
-- debut_date 값을 기준으로 정렬 (기본 ASC)
SELECT * FROM member
	ORDER BY debut_date;
```

- _**ORDER BY**_ 절은 데이터를 정렬한다.
- WHERE 절 다음에 나와야 함
- _**ASC**_ (ascending order) : 오름차순 → **(생략시 기본값)**
- _**DESC**_ (descending order) : 내림차순

```sql
-- height 컬럼 값이 164 이상인 데이터를 조회하여 
-- height 값 기준 내림차순 정렬하고 동일한 값이라면 debut_date 값 기준 오름차순 정렬
SELECT * FROM member
  	WHERE height >= 164
	ORDER BY height DESC, debut_date;
```

- 콤마 _**,**_ 로  여러 정렬 조건 지정 가능

### LIMIT  -  출력 개수 제한

```sql
SELECT * FROM member
	LIMIT 3;    		-- 상위 3건만 조회

SELECT * FROM member
	LIMIT 3, 2; 		-- 3번째 데이터부터 2건만 조회
	LIMIT 2 OFFSET 3; 	-- 위와 동일
```

- **_LIMIT 시작, 개수_** 
- **_LIMIT_** 뒤에 하나의 숫자만 입력시 처음부터 N까지의 데이터만 가져옴
- LIMIT 과 OFFSET 조합으로도 출력 개수를 제한할 수 있다.

###  DISTINCT -  중복 데이터 제거

```sql
-- addr 의 모든 컬럼 값을 중복을 제거하여 조회
SELECT DISTINCT addr
	FROM member;
```

- **_DISTINCT_**를 열 이름 앞에 붙이면 중복된 값은 1개만 출력된다.

###  GROUP BY  -  그룹화

```sql
-- mem_id가 같은 데이터를 그룹으로 묶음
-- 그룹핑된 데이터에서 mem_id와 amount의 합계를 구함
SELECT mem_id, SUM(amount) AS "합계"
	FROM buy
  	GROUP BY mem_id
  	ORDER BY mem_id;
```

- 컬럼이 같은 데이터를 그룹화 해주는 기능
- 보통 **집계 함수**와 같이 쓰임

```sql
SELECT genre, AVG(price) AS "평균"
	FROM library
  	GROUP BY genre;
```

![](https://blog.kakaocdn.net/dn/wzcym/btrZbrdR6ge/tiZ73CoQja3KH9UvCTzk71/img.jpg)

GROUP BY(genre) 예시

- GROUP BY 로 지정한 컬럼이 같은 데이터끼리 그룹화
- 여러 컬럼 지정 가능

#### **집계 함수** (_**Aggregate Function**_)

- **SUM()** : 컬럼의 합계를 반환
- **AVG()** : 컬럼의 평균을 반환
- **MIN()** : 컬럼의 최소값을 반환
- **MAX()** : 컬럼의 최대값을 반환
- **COUNT()** : 행의 개수를 셈
- **COUNT(DISTINCT)** : 행의 개수를 셈

```sql
-- 집계 함수 안에서 연산도 가능
SELECT mem_id, SUM(amount*price) AS "총 금액"
	FROM buy
    GROUP BY mem_id
    ORDER BY mem_id;
```

- 집계 함수 내에서 사칙 연산도 가능하다.

#### **COUNT()**

```sql
-- member 테이블의 모든 데이터 개수를 셈
SELECT COUNT(*) 
	FROM member;
    
-- member 테이블의 phone1 컬럼이 NULL인 것을 제외한 모든 데이터 개수를 셈
SELECT COUNT(phone1) 
	FROM member;
```

- **COUNT(*)** 연산은 모든 row를 대상으로 이루어지기 때문에 NULL값이 포함되어있어도 카운트됨
- **COUNT(phone1)** 연산은 phone1 값에 NULL이 있을 경우 카운트하지 않음

### HAVING  -  그룹 조건

```sql
-- mem_id 를 기준으로 그룹화
-- 그룹화된 데이터를 기준으로 amount*price 합계가 1000 이상인 그룹만 남김
-- 조건에 걸러진 그룹에서 amount*price 의 합계를 조회
SELECT SUM(amount*price) AS "총 금액"
	FROM buy
    GROUP BY mem_id
    HAVING SUM(amount*price) >= 1000;
```

- 그룹화된 데이터에 대해서 조건을 제한함
- _**GROUP BY**_ 뒤에 와야함



## 다중 테이블 연산

### Join

- Join은 두 개의 table들을 연결(join)해서 두 table의 레코드를 읽어 들이고 싶을 때 사용합니다.

![](https://velog.velcdn.com/images%2Ftaeha7b%2Fpost%2F6c3d1714-c859-48ba-9714-941552206081%2FJoin.png)

- INNER JOIN: 기준이 되는 테이블 (left table)과 join이 걸리는 테이블(right table) 양쪽 모두에 matching되는 row만 select가 됨.
    
- LEFT JOIN: 기준이 되는 테이블 (left table)의 모든 row와 join이 걸리는 테이블(right table)중 left table과 matching되는 row만 select가 됨.
    
- RIGHT JOIN: join이 걸리는 테이블(right table)의 모든 row와 기준이 되는 테이블 (left table)에서 right table과 matching되는 row만 select가 됨.
    
- FULL (OUTER) JOIN: 기준이 되는 테이블 (left table)과 join이 걸리는 테이블(right table) 양쪽 모두의 모든 row를 select 한다.

### Join 기본 문법

**SELECT**  
테이블이름.조회할 테이블,  
테이블이름.조회할 테이블  
**FROM** 기준테이블 이름  
**(INNER, LEFT, RIGHT FULL) JOIN** 조인테이블 이름  
**ON** 기준테이블이름.기준키 = 조인테이블이름.기준키;

```sql
예시) 

SELECT users.id, users.name, users.age, users.gender, accounts.account_type 
FROM users JOIN accounts ON accounts.id = users.account_id;
```




COALESCE(A,'A의값이 없는 경우의 값')   >> * 모든 데이터베이스에서 사용
```sql
  SELECT COALESCE(MIN(SORT),0) FROM tb_comment

   WHERE  CGROUP = 2

   AND SORT > 0

   AND DEPTH <= 0;
```

IFNULL(A,'A의값이 없는 경우의 값')

```sql
SELECT IFNULL(MIN(SORT),0) FROM tb_comment

WHERE  CGROUP = 2

AND SORT > 0

AND DEPTH <= 0;


-- name을 조회했을 때 값이 null이면 '값이 없습니다'가 출력--
SELECT IFNULL(name, '값이없습니다')
  FROM TEST 


```









---
참조 - https://velog.io/@taeha7b/mysql-in-a-nutshell

https://rachel0115.tistory.com/entry/SQL-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC-SELECT-%EC%A0%88

https://velog.io/@ejayjeon/MYSQL-1.-%EA%B3%84%EC%A0%95-%EC%83%9D%EC%84%B1-%EA%B6%8C%ED%95%9C-%EB%B6%80%EC%97%AC