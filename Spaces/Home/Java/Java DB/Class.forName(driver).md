**JDBC 4.0 이상부터는 자동적으로 classpath에 있는 클래스를 로드해서 `Class.forName()`을 작성할 필요가 없다고 한다.**


자바 파일은 확장자가 .java 인 파일로서 자바 언어로 소스 코드를 작성할 때

그 내용을 적는 파일을 뜻합니다.

그리고 이 자바 파일을 자바 컴파일러로 컴파일한 파일이

바로 .class 확장자를 가진 클래스 파일입니다.

우리가 흔히 이클립스와 같은 IDE 혹은 커맨드 라인에서

javac 명령어를 통해 컴파일했을 때 나오는 파일이죠.

이 클래스 파일을 가지고 자바의 클래스 로더(Class Loader)가

JVM으로 클래스 파일을 로딩합니다.

Java에서 클래스의 로딩 과정은 클래스 로더(Class Loader)가 확장자가

.class 클래스 파일의 위치를 찾아

그것을 JVM위에 올려놓는 과정을 뜻합니다. 

### 동적 로딩

Java는 동적 로딩을 지원하기 때문에 실행 시에 모든 클래스가 로딩되지 않고 필요한

시점에 클래스를 로딩하여 사용할 수 있습니다. 

- 로드 타임 동적 로딩 : 하나의 클래스를 로딩하는 과정에서 필요한 다른 클래스를 동적으로 로딩하는 것
- 런타임 동적 로딩 : 코드를 실행하는 순간(런타임)에 필요한 클래스를 로딩하는 것   
      
    

### 로드 타임 동적 로딩

로드 타임 동적로딩은  하나의 클래스를 로딩하는 과정에서

필요한 다른 클래스를 동적으로 로딩하는 것입니다.

예를 들어 우리가 처음에 시작하는 Hello.java에서 

저희는 System.out.prinln("Hello World");와 같은 코드를 작성하고  
이를 Hello.class로 컴파일합니다. 

이 때 Hello.class만 JVM에 로딩하는게 아니라 Hello.class를 로딩하는데

필요한  System, String 관련 .class파일들도 미리 JVM에 로딩된다는 것입니다.   

여기서 System,String 관련 .class파일을 로딩하지못하면

역시 Hello.class를 로딩하지 못합니다.

### 런타임 동적 로딩

런타임 동적 로딩은코드를 실행하는 순간에 필요한 클래스를 로딩하는 것입니다.

즉  JVM이 실제로 .class파일을 실행할  때 코드 속에서

'아,  어떤 클래스.class를 로딩하자' 라는 코드를 발견하고

관련 .class파일을 로딩을하는 걸 말합니다.

로딩을 하고나서는 다음 내용을 실행합니다.   

바로 Class.forName("로드할 클래스 이름")이 바로 런타임동적로딩에 해당합니다.

![[ClassForName1.jpg]]


위 그림에서 컴파일러가  javac.exe로 .class를 만들면서 로드타임동적로딩이 되고 

JVM이 실제로 java.exe로 실행할 때 런타임 동적 로딩이 됩니다.

### 자바에서 DB연결까지

일반적으로 자바에서 DB(oracle)까지 연결하는 코드는 다음과 같습니다.

(OJDBC는 라이브러리는 미리 다운받았습니다.)

```java
	try{
		Class.forName("oracle.jdbc.driver.OracleDriver"); 
	}catch (ClassNotFoundException e){
		//에러처리하는 코드 
	}
	Connection conn=null;
	Statement stmt=null;
	ResultSet rs=null;
	
	try{
		conn=DriverManager.getConnection("jdbc:oracle:thin:@127.0.0.1:1521:xe","jsp","oracle");
		stmt=conn.createStatement(); //쿼리 수행
		rs=stmt.executeQuery("SELECT 1 FROM DUAL");
		if(rs.next())  System.out.print(rs.getInt(1));
		
	}catch (SQLException e){
		//에러처리하는 코드 
	}finally {
		if(rs!=null){try{rs.close();} catch(Exception e){} }
		if(stmt!=null){try{stmt.close();} catch(Exception e){} }
		if(conn!=null){try{conn.close();} catch(Exception e){} }
	}
```

※실행도중 Class.forName()에 있는 클래스를 로더하려고했는데

문자열을 잘못 입력했다면 해당 클래스를 찾을 수 없기때문에

ClassNotFoundException이 발생한다.

또 연결,쿼리 수행 도중에 나는에러는 전부 SQLException이다.

> JAVA에서 JDBC API를 이용해 jdbc Driver(나는 오라클드라이버)를 이용하려면
> 
> 해당 드라이버가 로드되어야합니다.

Class.forName("oracle.jdbc.driver.OracleDriver")은

> '어이 JVM, 실행하다가 이 코드만나면 OracleDriver로드하고 실행해'라는 의미가 될겁니다. 

자 그럼 Class.forName()을 통해 oracle드라이버는 실행도중 로드가 됐습니다.

근데 로드만 했을 뿐 변수를 선언해  oracle드라이버객체를 받지도 않았는데 어떻게 바로 conn=DriverManger.getConnection()을 써서 DB에 연결을 할 수 있었을 까요?

정답은 바로 Driver클래스의 static{}구문에 있습니다.

다음은 Driver클래스의 구조인데요. (OracleDriver도 Driver의 자식)

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    static {
        try {
           java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }
    
    그외 기타 기능
}
```

다음은 OracleDriver static구문 

```java
static 
    {
        defaultDriver = null;
        Timestamp timestamp = Timestamp.valueOf("2000-01-01 00:00:00.0");
        try
        {
            if(defaultDriver == null)
            {
                defaultDriver = new OracleDriver();
                DriverManager.registerDriver(defaultDriver);
            }
        }
        catch(RuntimeException runtimeexception) { }
        catch(SQLException sqlexception) { }
    }
```

static{}구문은 클래스가 로드될 때 실행되는 구문입니다.

즉 로드될 때  Driver를 DriverManger라는 곳에 등록했기 때문에

바로 DB에 연결하는 Connection conn=DriverManger.getConnection("url","id","pw")구문을

사용해  DB에 연결할 수 있게 된겁니다. 

 이와 같이 등록한 JDBC Driver는 데이터베이스 Connection을 생성하는 시점에 사용되게 됩니다.

※getConnection메소드는 DriverManger의 메소드다.

url,id,pw를 매개변수로 받고  DriverManger가 가지고 있는 드라이버를 통해 DB에 연결한다.

지금까지과정을 정리하면 다음과 같습니다.

- 1. Class.forName()을 통해 실행도중 오라클 드라이버로드
- 2. 로드되면서 OracleDriver의 static{}구문 -> DriverManger에  OracleDriver 등록
- 3. DriverManger.getConnection()을 통해 Oracle에 연결
- 4. 이후는 쿼리 수행 

### Class.forName사용 이유

static{}구문이 로딩될 때 사용되는 거라면 OracleDriver oracle=new OracleDriver()

로드타임 동적 로딩으로 해도 DirverManger에 등록되서 

DriverManger.getConnection()하면 되지 않냐?? 

```java
	OracleDriver oracleDriver=new OracleDriver();
	Connection conn=null;
	Statement stmt=null;
	ResultSet rs=null;
	
	try{
		conn=DriverManager.getConnection("jdbc:oracle:thin:@127.0.0.1:1521:xe","jsp","oracle");
		stmt=conn.createStatement(); //쿼리 수행
		rs=stmt.executeQuery("SELECT 1 FROM DUAL");
		if(rs.next())  System.out.print(rs.getInt(1));
		
	}catch (SQLException e){
		//에러처리하는 코드 
	}finally {
		if(rs!=null){try{rs.close();} catch(Exception e){} }
		if(stmt!=null){try{stmt.close();} catch(Exception e){} }
		if(conn!=null){try{conn.close();} catch(Exception e){} }
    }
```

맞습니다. 위처럼 코딩해도 문제없이 실행됩니다. 

근데 위의 코드에서 oracleDriver객체는 사용되지않습니다.

즉 쓸데없이 객체1개를 생성해서 메모리 낭비를 하고 있습니다.

또  Class.forName()의 매개변수 문자열 값을 직접 입력하는게 아니라

조건에 따라 oracle이냐 mysql이냐 하는 경우겠죠.

(물론 이때는 getConnection()의  매개변수 문자열 값도 달라집니다)

JSP를 예로 들면  파라미터값에 따라

oracle에 연결해라, mysql에 연결해라 라는 코드가 있다고 합시다.

```java
String dirverName=request.getParameter("driverName");
	String connectionInfo="";
	if(dirverName.equals("oracle.jdbc.driver.OracleDriver")){
		connectionInfo="jdbc:oracle:thin:@127.0.0.1:1521:xe";
	}else{
		//오라클 말고 기타 DB Driver
	}
	
	
	try{
		Class.forName(dirverName);
	}catch (ClassNotFoundException e){
		//에러처리하는 코드 
	} 
	Connection conn=null;
	Statement stmt=null;
	ResultSet rs=null;
	
	try{
		conn=DriverManager.getConnection(connectionInfo,"jsp","oracle");
		stmt=conn.createStatement(); //쿼리 수행
		rs=stmt.executeQuery("SELECT 1 FROM DUAL");
		if(rs.next())  System.out.print(rs.getInt(1));
		
	}catch (SQLException e){
		//에러처리하는 코드 
	}finally {
		if(rs!=null){try{rs.close();} catch(Exception e){} }
		if(stmt!=null){try{stmt.close();} catch(Exception e){} }
		if(conn!=null){try{conn.close();} catch(Exception e){} }
	}
```

Class.forName()을 안 쓸 경우  컴파일 하는 과정에서

OracleDriver 및 기타 DBDriver를 전부 로드해야합니다.

(로드타임으로 한다면 어떤 driver가 사용될지모르기때문에 모든 Driver를 로드합니다.)

하지만 Class.forName()을 사용한다면  실행도중 한가지의 Driver만 로드하게 됩니다.

즉  Class.forName()을 쓰면 

1.Driver의 경우 로드만하면되지 객체 생성 필요없습니다.

2. 특정조건에따라 Driver로드 다르게 할 때 쓸데없이 모든 Driver를 로드 할 필요 없습니다.

3. 로드가 되는 시점이 컴파일이 아닌 런타임이기 때문에 static{} 구문의 실행시점을
     조절할 수  있습니다.




---
참조 - https://brilliantdevelop.tistory.com/54 - classforName