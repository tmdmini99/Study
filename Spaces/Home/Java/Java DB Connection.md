
```java
- JDBC (Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다
```

### 1. 준비사항

```java
1. 사용하려는 DB 의 라이브러리 다운
- ojdbc8.jar
<https://www.oracle.com/database/technologies/jdbc-ucp-122-downloads.html>

2. DB 연동할 Java Project에 ojdbc.jar 추가
	- 프로젝트별로 추가
	1) 프로젝트명 우클릭 -> Build Path -> Configure Build Path
	2) Libraries Tap -> ModulePath Click -> Add External Jars CLick
	3) Ojdbc8.jar 찾아서 클릭하고 열기
	4) ApplyAndClose 
```

### 2. DAO

```java
1. DB 연결 정보
	String username = ""   : DB의 ID
	String password = ""   : DB의 PW
	String url = ""        : DB의 IP:PORT:SID
	String driver = ""     : 사용하려는 DB의 Library (생략가능) 	

2. driver를 메모리에 로딩
	- 1번에서 driver를 생략 했으면 생략 가능
	Class.forName(driver);

3. DB 연결
	Connection con = DriverManger.getConnection(url, username, password)

4. SQL Query문 작성
	- 한개의 문자열이 나오도록 작성
	- 끝에 ; 는 작성하지 않음
	- 변수값은 ? 처리
	- 컬럼명이나 테이블명은 ? 로 처리 할 수 없음
	- 문자열 이나 날짜 Data 는 '' 생략
	String sql = "SELECT * FROM DEPARTMENTS WHERE DEPARTMENT_ID = ?"

5. 작성한 Query문을 DB로 미리 전송
	- 미완성된 Query문을 DB로 보내서 실행 준비 시키기
	PreparedStatement  st = con.prepareStatement(sql);

6. ? 값 적용
	- DB에게 ? 값 전송 준비
	- set데이터타입(인덱스번호, 값)
	- 인덱스 번호는 ? 의 순서대로 자동 지정, SQL문을 읽는 순서와 상관 없음
	- 인덱스 번호는 1번 부터 시작
	st.setInt(1, 10);

7. 최종 전송 후 결과처리

	- executeXXX(SQL문을 넣으면 에러)

	a. SELECT
	ResultSet rs = st.executeQuery();
		- Java에서 DB의 ResultSet을 처리하기 위한 ResultSet 제공
		- 주요메서드
			1) next()
					- 결과물에서 한줄의 ROW를 읽고 데이터가 있으면 true, 없으면 false를 리턴
					- 결과물에서 Data를 꺼내 오려면 무조건 실행

			2) getXXX()
					- next() 메서드가 호출된 후 사용 가능
					- get데이터타입("조회결과의 컬럼명");
					- get데이터타입(컬럼의 인덱스 번호)  - SELECT 결과의 왼쪽부터 1이 자동 지정

	b. INSERT, UPDATE, DELETE
	int result = st.executeUpdate();

8. 연결 해제
	- 연결된 역순으로 해제
	rs.close(); //SELECT 만, INSERT UPDATE DELETE는 ResultSet이 없음
	st.close();
	con.close();

```





















---
참조- https://itstudy402.tistory.com/79

[12. JDBC (notion.site)](https://marble-plastic-56b.notion.site/12-JDBC-cc30aa2d5ecd4d0094149b31e71af38f)


https://fora.tistory.com/71

https://brilliantdevelop.tistory.com/54 - classforName


https://smile-place.tistory.com/entry/JAVA-%EC%9E%90%EB%B0%94%EC%97%90%EC%84%9C-OPEN-API-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0




https://programmingbeginner.tistory.com/124