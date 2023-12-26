
```jsx
1. DBMS(DataBase Management System)
		- DataBase : Data의 저장소
		- DBMS는 DB에 데이터들을 관리하는 프로그램
		- 통상 DBMS도 DB에 포함해서 부른다

2. RDBMS(Relational DataBase Management System)
		- 관계형 DB
		- oracle, mysql, mssql, maria, ...
```

### SQL(Structured Query Language)

```jsx
- 구조화된 질의 언어
- SQL은 DB를 관리하는 DBMS에서 사용하는 언어
- DB별로 언어가 있지만 표준화가 되어 있음
- 예약어(키워드)는 대소문자를 구별하지 않음, 대문자 사용을 권장
- Data는 대소문자 구별
- 여러줄에 걸쳐서 작성이 가능, 마지막에는 ;  종료
- 문자열과 날짜 형식의 데이터는 앞 뒤로 ' ' 사용

ex) 테이블명 : 대소문자 구별 X (Oracle)
	  컬럼명   : 대소문자 구별 X (Oracle)
```

### Table, Row, Column, Field

```jsx
- DB는 Excel과 비슷한 형태
1. Table  -> sheet
2. Row    -> 가로 한줄(행)
3. Column -> 세로 (열)
4. Field  -> 한칸
```

### SQL문 종류

```java
1. DML(Data Manipulation Language)
	- DB에 Data를 생성, 수정, 삭제, 조회
	- CRUD 작업
	1) INSERT : 생성 (Create)
	2) UPDATE : 수정 (Update)
	3) DELETE : 삭제 (Delete)
	4) SELECT : 조회 (Read)

2. DDL(Data Definition Language)
	- DB 객체(table, user...)를 생성 수정 삭제
	1) CREATE     : 생성
	2) ALTER      : 수정
	3) DROP       : 삭제
	4) TRUNCATE   : 삭제
	5) RENAME     : 이름 변경

3. DCL (Data Control Language)
	- 객체의 권한
	1) GRANT     : 권한 지정(적용)
	2) REVOKE    : 권한 취소

4. TCL(Transaction Control Language)
	- 데이터의 무결성, 트랜잭션 제어
	1) COMMIT     : 저장
	2) ROLLBACK   : 취소
```

### Data Type

```java
1. Number
	- 숫자, 정수, 실수 다 포함
	- Number(전체자릿수, 소숫점자리수)
	- 최대 38자리
	ex) Number(3, 1) : 12.1, 3.2, 12.34(X), 3.14(X)
	ex) Number(3)    : 123, 999, 12.3(x)

2. varchar
	- 고정길이 문자열
	- varchar(바이트수)
	- 최대 2000 byte
	ex) varchar(3)
		- 3byte를 확보
		- 영어 1글자를 입력하면 나머지 공간은 공백으로 채워서 3 byte로 만듬

3. varchar2
	- 가변길이 문자열
	- varchar2(바이트수)
	- 최대 4000 byte
	ex) varchar2(3)
		- 영어 1글자를 입력하면 1byte 만 차지

4. CLOB
	- 대용량의 Text, e-pub
	- 최대 4GB

5. BLOB
	- 대용량의 이진데이터(Binary)
	- 최대 2GB

6. Date
	- 날짜와 시간
	- 세기, 년, 월, 일, 시, 분, 초, 오전/오후, 요일
	- 한글 - yy/mm/dd
	- 영문 - dd/MON/yy

7. TimeStamp
	- 날짜와 시간
	- 밀리세컨즈

* 영어 - 1byte
* 한글 - 2byte or 3byte
```