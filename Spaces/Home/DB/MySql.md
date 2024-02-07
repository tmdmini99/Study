
### Data Type



자동 번호 매기기
```sql
`ROW_NUMBER() OVER (ORDER BY SIGUN_NM ASC)`는 SQL에서 사용되는 윈도우 함수입니다. 이 함수는 결과 집합의 각 행에 순차적인 번호를 할당합니다.

`ROW_NUMBER()` 함수는 `OVER` 절과 함께 사용되며, `OVER` 절 내부에 정렬 조건을 지정할 수 있습니다. 위의 쿼리에서는 `SIGUN_NM`을 오름차순으로 정렬하기 위해 `ORDER BY SIGUN_NM ASC`를 사용했습니다.

따라서 `ROW_NUMBER() OVER (ORDER BY SIGUN_NM ASC)`는 `SIGUN_NM`을 기준으로 정렬한 후, 그 결과에 대해 각 행에 순차적인 번호를 할당하는 것을 의미합니다.

이렇게 번호가 할당된 결과를 사용하면 각 행에 순차적인 식별자를 부여할 수 있습니다. 예를 들어, 결과 집합의 첫 번째 행은 `NUM` 값이 1이 되고, 두 번째 행은 2가 되는 식으로 번호가 할당됩니다.
```


```sql
SELECT ROW_NUMBER() OVER (ORDER BY SIGUN_NM ASC) AS NUM, op.*
```


```SQL
INSERT IGNORE INTO [TABLE] (COLUMN1, COLUMN2, ...)  
VALUES (VALUE1, VALUE2, ...)

```
이터가 이미 있으면 따로 후속처리없이

  

그냥 아무행위도 안하고 나머지 데이터들만 INSERT를 진행하고자할 때

  

INSERT INTO 구문에 IGNORE을 추가해주면 된다.


hash 코드 사용해서 값 랜덤으로 넣을수 있음
```

```

https://namkisec.tistory.com/entry/mysql-Hash-%EC%82%AC%EC%9A%A9%EA%B8%B0sha


## 문자형 데이터타입 

|**데이터 유형**|**정의**|
|---|---|
|CHAR(n)|고정 길이 데이터 타입(최대 255byte)- 지정된 길이보다 짦은 데이터 입력될 시 나머지 공간 공백으로 채워진다.|
|VARCHAR(n)|가변 길이 데이터 타입(최대 65535byte)- 지정된 길이보다 짦은 데이터 입력될 시 나머지 공간은 채우지 않는다.|
|TINYTEXT(n)|문자열 데이터 타입(최대 255byte)|
|TEXT(n)|문자열 데이터 타입(최대 65535byte)|
|MEDIUMTEXT(n)|문자열 데이터 타입(최대 16777215byte)|
|LONGTEXT(n)|문자열 데이터 타입(최대 4294967295byte)|
|JSON|JSON 문자열 데이터 타입 - JSON 형태의 포맷을 꼭 준수해야 한다.|


## 숫자형 데이터 타입 

|**데이터 유형**|**정의**|
|---|---|
|TINYINT(n)|정수형 데이터 타입(1byte) -128 ~ +127 또는 0 ~ 255수 표현할 수 있다.|
|SMALLINT(n)|정수형 데이터 타입(2byte) -32768 ~ 32767 또는 0 ~ 65536수 표현할 수 있다.|
|MEDIUMINT(n)|정수형 데이터 타입(3byte) -8388608 ~ +8388607 또는 0 ~ 16777215수 표현할 수 있다.|
|INT(n)|정수형 데이터 타입(4byte) -2147483648 ~ +2147483647 또는 0 ~ 4294967295수 표현할 수 있다.|
|BIGINT(n)|정수형 데이터 타입(8byte) - 무제한 수 표현할 수 있다.|
|FLOAT(길이, 소수)|부동 소수형 데이터 타입(4byte) -고정 소수점을 사용 형태이다.|
|DECIMAL(길이, 소수)|고정 소수형 데이터 타입고정(길이+1byte) -소수점을 사용 형태이다.|
|DOUBLE(길이, 소수)|부동 소수형 데이터 타입(8byte) -DOUBLE을 문자열로 저장한다.|

## 날짜형 데이터 타입

|**데이터 유형**|**정의**|
|---|---|
|DATE|날짜(년도, 월, 일) 형태의 기간 표현 데이터 타입(3byte)|
|TIME|시간(시, 분, 초) 형태의 기간 표현 데이터 타입(3byte)|
|DATETIME|날짜와 시간 형태의 기간 표현 데이터 타입(8byte)|
|TIMESTAMP|날짜와 시간 형태의 기간 표현 데이터 타입(4byte) -시스템 변경 시 자동으로 그 날짜와 시간이 저장된다.|
|YEAR|년도 표현 데이터 타입(1byte)|



## 이진 데이터 타입 

|**데이터 유형**|**정의**|
|---|---|
|BINARY(n) & BYTE(n)|CHAR의 형태의 이진 데이터 타입 (최대 255byte)|
|VARBINARY(n)|VARCHAR의 형태의 이진 데이터 타입 (최대 65535byte)|
|TINYBLOB(n)|이진 데이터 타입 (최대 255byte)|
|BLOB(n)|이진 데이터 타입 (최대 65535byte)|
|MEDIUMBLOB(n)|이진 데이터 타입 (최대 16777215byte)|
|LONGBLOB(n)|이진 데이터 타입 (최대 4294967295byte)|

### 숫자 자동 증가
```sql
AUTO_INCREMENT PRIMARY KEY

ex)
CREATE TABLE 'test'( 
	'num' int(10) NOT NULL AUTO_INCREMENT PRIMARY KEY, 
	'name' varchar(10) NOR NULL 
);
```



```sql
show databases; -- database 조회--
select user, host from user; -- 사용자 조회--
show grants for test@host; -- 사용자 권한 조회--
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


```mysql
'GRANT USAGE ON *.* TO `test`@`localhost`'

```

`GRANT USAGE`는 사용자가 MySQL 서버에 접속할 수 있는 권한을 주는 동시에, 다른 권한(예: SELECT, INSERT 등)은 부여하지 않습니다. 따라서 사용자는 접속은 가능하지만, 특정 데이터베이스나 테이블에 대한 작업은 할 수 없습니다.
`'GRANT USAGE ON *.* TO 'test'@'localhost'`는 `'test'@'localhost'` 사용자에게 MySQL 서버에 접속할 수 있는 권한을 부여하고, 다른 권한은 부여하지 않는 것을 의미합니다.


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

![[MySql1.jpg]]


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

![[MySql2.jpg]]


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

![[MySql3.png]]


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

https://www.incodom.kr/DB_-_%EB%8D%B0%EC%9D%B4%ED%84%B0_%ED%83%80%EC%9E%85/MYSQL#h_732744493db972e54a38219a89782ad6

https://prostudy.tistory.com/2215 가상 컬