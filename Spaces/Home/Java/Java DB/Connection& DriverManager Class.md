### Connection 생성

- Connection - 데이터베이스와 연결하는 객체입니다.
- **DriverManager.getConnection(연결문자열, DB_ID, DB_PW)** 으로 Connection 객체를 생성합니다.
- 연결문자열(Connection String) - **“jdbc:Driver 종류://IP:포트번호/DB명”** 
- ex) jdbc:mysql://localhost:3306/test_db
- DB_ID : MySQL 아이디
- DB_PW : MySQL 비밀번호


#### 2.1 DriverManager 클래스

- DriverManager 클래스는 JDBC 드라이버를 통하여 Connection을 만드는 역할을 합니다.
- DriverManager는 Class.forName( ) 메소드를 통해서 생성됩니다.




---
참조 -  https://allg.tistory.com/20