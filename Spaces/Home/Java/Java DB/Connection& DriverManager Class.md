




![[JDBC.png]]






### Connection 생성

- Connection - 데이터베이스와 연결하는 객체입니다.
- **DriverManager.getConnection(연결문자열, DB_ID, DB_PW)** 으로 Connection 객체를 생성합니다.
- 연결문자열(Connection String) - **“jdbc:Driver 종류://IP:포트번호/DB명”** 
- ex) jdbc:mysql://localhost:3306/test_db
- DB_ID : MySQL 아이디
- DB_PW : MySQL 비밀번호
- 특정 데이터 원본에 대한 커넥션은 Connection 인터페이스가 구현된 클래스의 객체로 표현된다.
- **어떤 SQL 문장을 실행시키기 전에 우선 Connection 객체가 있어야 한다.**
- Connection 객체는 특정 데이터 원본과 연결된 커넥션을 나타내고, 
- 특정한 SQL 문장을 정의하고 실행시킬 수 있는 Statement 객체를 생성할 때 Connection 객체를 사용한다.
- Connection 객체는 데이터베이스에 대한 데이터인 메타데이터(Meta Data)에 관한 정보를 데이터 원본에 질의하는데 사용한다. 이 때, 사용 가능한 테이블의 이름, 특정 테이블의 열에 정보 등이 포함된다.


### 2.1 DriverManager 클래스

- DriverManager 클래스는 JDBC 드라이버를 통하여 Connection을 만드는 역할을 합니다.
- DriverManager는 Class.forName( ) 메소드를 통해서 생성됩니다.
- 
- DriverManager 클래스는 데이터 원본에 JDBC 드라이버를 통하여 **커넥션을 만드는 역할**을 한다.
- Class.forName() 메소드를 통해서 생성되는데, 이 메소드는 인터페이스 드라이버를 구현하는 작업이다.
- **Class.forName("oracle.jdbc.driver.OracleDriver")** 처럼 특정 클래스를 로딩하면 자동으로 객체가 생성되고 DriverManager에 등록된다.
- 드라이버 클래스를 찾지 못할 경우 forName() 메소드는 **ClassNotFoundException 예외를 발생**시키므로, 반드시 예외처리를 해야 한다.
- 드라이버 클래스들은 로드될 때 자신의 인스턴스를 생성하고, 자동적으로 DriverManager 클래스 메소드를 호출하여 인스턴스를 등록한다.
- DriverManager 클래스의 모든 메소드는 **static** 이기 때문에 반드시 객체를 생성시킬 필요가 없다.
- DriverManager 클래스는 Connection 인터페이스의 구현 객체를 생성하는데 getConnection() 메소드를 사용한다.



### 3. Statement 인터페이스

  

- Statement 인터페이스는 Connection 객체를 통해 프로그램에 리턴되는 객체에 의해 구현되는 일종의 메소드 집합을 정의한다. Statement 객체는 Statement 인터페이스를 구현한 객체로, 
- 항상 인수가 없는 Connection 클래스의 CreateStatement() 메소드를 호출함으로써 얻어진다.

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5|`try{`<br><br>        `Statement stmt = connection.createStatment();`<br><br>`}catch(SQLException e){`<br><br>    `e.printStackTrace();`           <br><br>`}`|

- Statement 객체를 생성하면 Statment 객체의 **exequteQuery()** 메소드를 호출하여 SQL 질의를 실행시킬 수 있다.
- 메소드의 인수로는 SQL 질의 문장을 담은 String 객체를 전달한다.
- **Statment 객체는 단순한 질의문을 사용할 경우에 좋다.**

  

  

  

### 4. PreparedStatement 인터페이스

- PreparedStatement 인터페이스는 Connectio 객체의 prepareStatement() 메소드를 사용해서 객체를 생성한다.

- PreparedStatement 객체는 SQL 문장이 미리 컴파일되고, 실행시간동안 인수 값을 위한 공간을 확보할 수 있다는 점에서 Statement 객체와 다르다.
- **PreparedStatement 객체는 동일한 질의문을 특정 값만 바꾸어서 여러 번 실행해야 할 때, 많은 데이터를 다루기 때문에 질의문을 정리해야 할 필요가 있을 때, 인수가 많아서 질의문을 정리해야 할 필요가 있을 때 사용하면 유용하다.**
- 또한, Statement 객체의 SQL은 실행될 때 매번 서버에서 분석되어야 하는 반면, **PreparedStatement 객체는 한번 분석되면 재사용이 용이하다는 장점을 가지고 있다.**

- PreparedStatement 인터페이스는 각각의 인수에 대해 위치홀더 (placeholder)를 사용하여 SQL 문장을 정의할 수 있다. **위치홀더는 물음표 (' ? ' )로 표현된다.** 위치홀더는 **SQL 문장에 나타나는 토큰(Token)**인데, **이것은 SQL 문장이 실행되기 전에 실제 값으로 대체된다.** 이러한 방법을 이용하면 특정 값으로 문자열을 연결하는 방법보다 훨씬 쉽게 SQL 문장을 만들 수 있다.

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8|`try{`<br><br>        `String sql =``"INSERT INTO tb_user VALUES(? , ?)"``;`<br><br>        `pstmt = conn.prepareStatement(sql);`<br><br>        `pstmt.setString(``1``,id);`<br><br>        `pstmt.setString(``2``,pw)`<br><br>`}catch(SQLException e){`<br><br>        `e.printStackTrace();`   <br><br>`}`|

- PreparedStatement 객체는 각각의 SQL 타입을 처리할 수 있는 setXxxx() 메소드를 제공한다. 여기서 Xxxx는 해당 테이블의 해당 필드의 데이터 타입과 관련이 있다. 해당 필드의 데이터 타입이 문자열이면 setString()이 되고, 해당 필드의 데이터 타입이 int 이면 setInt()가 된다.
- setXxxx(num, var) 메소드는 두 개의 매개 변수를 가지고 있다. num은 파라미터 인덱스로서 위치홀더와 대응된다. 첫번째 위치홀더에 대응되면 1이고, 다음 위치홀더에 대응부터 1씩 값을 증가시키면 된다. var은 해당 필드에 저장할 데이터 값이다.
- PreparedStatement 객체가 제공하는 setXxxx(num, var) 메소드

|   |   |
|---|---|
|메소드 명|매개변수|
|setString|(int parameterIndex, String x)|
|setInt|(int parameterIndex, int x)|
|setLong|(int parameterIndex, Long x)|
|setObject|(int parameterIndex, Object x)|
|setDate|(int parameterIndex, Date x)|
|setTimestamp|(int parameterIndex, Timestamp x)|
|setDouble|(int parameterIndex, Double x)|
|setFloat|(int parameterIndex, Float x)|


- **PreparedStatement 객체를 사용하는 것이 Statement 객체를 사용하는 것 보다 효율적이다.**

- PreparedStatement 객체의 장점

1. 동일한 질의문을 특정 값만 바꿔서 여러번 실행해야 할 때, 많은 데이터를 다루기 때문에 질의문을 정리해야할  필요가 있을때, 인수가 많아서 질의문을 정리해야 할 필요가 있을때 좋다.

  

2. 미리 컴파일되기 때문에 쿼리의 수행 속도가 Statement 객체에 비해 빠르다.

  

3. Statement 객체는 쿼리 실행시 작은 따옴표( ' )가 들어 있으면 작은 따옴표를 두개 ( '' )로 표시해야 한다.

	 그러나 PreparedStatement 객체는 작은 따옴표의 문제를 쿼리 실행시 자동으로 처리하므로 신경 쓸 필요가  없다.

### 5. CallableStatement 인터페이스

  

- CallableStatement 인터페이스는 Connection 객체의 prepareCall() 메소드를 사용해서 객체를 생성한다.

CallableStatement 객체는 주로 스토어드 프로시저(Stored Procedure)를 사용하기 위해 사용한다.

  

- Stored Procedure란? 해당 데이터베이스 SQL 문을 저장한 것을 말한다.

미리 저장된 쿼리를 사용하기 때문에 수행 속도가 빠르다.

  



|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5|`try{`<br><br>        `CallableStatement cstmt = conn.prepareCall();`<br><br>`}catch(SQLException e){`<br><br>        `e.printStackTrace();`<br><br>`}`|

  

- CallableStatement는 데이터베이스에 저장된 Stored Procedure를 단지 호출하는 것만으로 처리가 가능하다.

  

conn.prepareCall("{call query1}")

|   |   |
|---|---|
|종류|설명|
|{call procedure_name[(?, ?, ...)]}|단순히 호출만 한다.|
|{ ? = call procedure_name[(?, ?, ...)]}|호출을 한 후 결과 값을 가지고 온다.|
|{call procedure_name}|파라미터가 없는 stored procedure|

  
  

  

  

### 6. ResultSet 인터페이스

  

- SQL 문에서 SELECT 문을 사용한 질의의 경우 성공 시 결과물로 ResultSet을 반환한다.

- ResultSet 은 SQL 질의에 의해 생성된 테이블을 담고 있다. 또한 ResultSet 객체는 '커서(cursor)' 라고 불리는 것을 가지고 있는데, 그것으로 ResultSet에서 특정 행에 대한 참조를 조작할 수있다.
- 커서는 초기에 첫번째 행의 직전을 가리키도록 되어 있는데, ResultSet 객체의 next() 메소드를 사용하면 다음 위치로 커서를 옮길 수 있다.

|   |   |
|---|---|
|메소드|설명|
|first()|커서를 첫번째 행으로 옮긴다.|
|last()|커서를 마지막 행으로 옮긴다.|
|beforeFirst()|커서를 첫번째 행 이전으로 옮긴다.|
|afterLast()|커서를 마지막 행 다음으로 옮긴다.|
|next()|커서를 다음 행으로 옮긴다.|
|previous()|커서를 이전 행으로 옮긴다.|
- ResultSet에서 행을 처리하는데 반복문을 사용하며 next() 메소드가 유효한 행이 있으면 true, 없으면 false를 리턴하는 것을 이용하여 while 으로 제어할 수 있다.


|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4|`while(rs.next()){`<br><br>        `String name = getString(``"해당 컬럼명"``);`<br><br>        `String id = getString(``"해당 컬럼명"``);`<br><br>`}`|
- ResultSet 객체에서 현재 행에서 필드명 혹은 레코드셋에서의 위치를 통해서 어떤 필드의 값을 가져올 수 있는데, 때 getXxxx() 메소드를 제공한다.
- 해당 필드의 데이터 타입이 문자열이면 getString()이 되고, 해당 필드의 데이터 타입이 int이면 getInt()가 된다.

|   |   |   |   |
|---|---|---|---|
|메소드|   |   |   |
|getString("해당 컬럼명")|getDate("해당 컬럼명")|getBytes("해당 컬럼명")|getDouble("해당 컬럼명")|
|getInt("해당 컬럼명")|getTimestamp("해당 컬럼명")|getObject("해당 컬럼명")|getLong("해당 컬럼명")|


---
참조 -  https://allg.tistory.com/20

https://programmingyoon.tistory.com/53