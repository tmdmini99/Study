
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





---
참조 - https://velog.io/@taeha7b/mysql-in-a-nutshell