
### 1. 안드로이드 데이터베이스 연동
안드로이드와 데이터베이스연동은 내장DB를 사용해도 되지만

서버연동이 되지 않기 때문에 외부디비(Oracle,MySql,몽고DB)을 사용한다.

외부디비를 사용할경우 안드로이드 보안 정책상 직접연동이 불가능하다.

그래서 중간매체인 JSP혹은PHP를 사용한다.

오늘 포스팅은 출처에 있는 글을 보고 디비연동을 해볼것이다.

초보자(필자)의 기준으로 작성되서 따라하기에는 무리가 없을것이다.

### 2. 이클립스에서 JSP 만들기

초보자분들이 가장 헷갈려하는 부분은 패키지의 구조와 파일의 위치일 것이다.


![[Kotlin DB1.png]]

**Java Resources**의 **src** 아래에 **com.db** 라는 패키지를 만들어 주었고

**WebContent**아래에 **Android**라는 폴더를 만들어주었다.

제일먼저 **ConnectDB.java**파일을 만들어주겠다.



```java
package com.db;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ConnectDB {
    private static ConnectDB instance = new ConnectDB();

    public static ConnectDB getInstance() {
        return instance;
    }
    public ConnectDB() {  }

    // oracle 계정
    String jdbcUrl = "jdbc:oracle:thin:@IPv4주소:1521:testdb";
    String userId = "test";
    String userPw = "test";

    Connection conn = null;
    PreparedStatement pstmt = null;
    PreparedStatement pstmt2 = null;
    ResultSet rs = null;

    String sql = "";
    String sql2 = "";
    String returns = "a";

    public String connectionDB(String id, String pwd) {
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            conn = DriverManager.getConnection(jdbcUrl, userId, userPw);

            sql = "SELECT id FROM userTBL WHERE id = ?";
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, id);

            rs = pstmt.executeQuery();
            if (rs.next()) {
                returns = "이미 존재하는 아이디 입니다.";
            } else {
                sql2 = "INSERT INTO userTBL VALUES(?,?)";
                pstmt2 = conn.prepareStatement(sql2);
                pstmt2.setString(1, id);
                pstmt2.setString(2, pwd);
                pstmt2.executeUpdate();
                returns = "회원 가입 성공 !";
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (pstmt2 != null)try {pstmt2.close();    } catch (SQLException ex) {}
            if (pstmt != null)try {pstmt.close();} catch (SQLException ex) {}
            if (conn != null)try {conn.close();    } catch (SQLException ex) {    }
        }
        return returns;
    }
}
```

여러분들이 수정해야할부분은 oracle 계정인데

**jdbcUrl**에서는 **IPv4**주소와 마지막 **testdb**부분이다.

IPv4주소는 cmd에서 **ipconfig**를 입력하여 IPv4주소를 알수있다.

testdb는 Oracle를 설치할때 **SID**를 입력해줬을텐데 그것에 해당한다


```jsp
<%@page import="com.db.ConnectDB"%>
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<%
   ConnectDB connectDB = ConnectDB.getInstance();

//한글 인코딩 부분
	request.setCharacterEncoding("utf-8");
   String id = request.getParameter("id");
   String pw = request.getParameter("pw");
	
   String returns = connectDB.connectionDB(id, pw);

   System.out.println(returns);

   // 안드로이드로 전송
   out.println(returns);
%>
```

상단의 <%@page improt는 반드시 필요한 코드이다.

여기서는 딱히 수정할만한 코드는 없다

**request.setCharacterEncoding**은 입력받은 한글을 데이터베이스에 저장할때 인코딩을 제대로 하기 위한 코드이다.

없을시 외계문자 i？？e¸？i？´ 와 같은 값이 보이게 된다.


### 3. 안드로이드 스튜디오에서 XML와 JAVA 만들기
먼저 **XML**을 만들어 주겠다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <TableLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="ID"
                    android:textAlignment="center"
                    android:textAppearance="@style/TextAppearance.AppCompat.Headline" />

                <EditText
                    android:id="@+id/register_id"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="8" />

            </TableRow>

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="PW"
                    android:textAlignment="center"
                    android:textAppearance="@style/TextAppearance.AppCompat.Headline" />

                <EditText
                    android:id="@+id/register_pw"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="8" />

            </TableRow>

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center_horizontal">

                <Button
                    android:id="@+id/register_btn"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_weight="10"
                    android:text="회원가입" />
            </TableRow>

        </TableLayout>

    </LinearLayout>

</LinearLayout>
```

화면은 아래와 같이 나온다.


![[Kotlin DB2.png]]


다음은 Java코드이다. Main 코드와 클래스파일 하나를 만들어 줘야한다.

![[Kotlin DB3.png]]


우선 **RegisterActivity** 클래스 부터 작성해보자.


```java
import android.os.AsyncTask;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;

public class RegisterActivity extends AsyncTask<String, Void, String> {
    String sendMsg, receiveMsg;

    @Override
    protected String doInBackground(String... strings) {
        try {
            String str;

            // 접속할 서버 주소 (이클립스에서 android.jsp 실행시 웹브라우저 주소)
            URL url = new URL("http://IP주소:포트번호/DbConn/Android/androidDB.jsp");
            // http://ip주소:포트번호/이클립스프로젝트명/WebContent아래폴더/androidDB.jsp

            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            conn.setRequestMethod("POST");
            OutputStreamWriter osw = new OutputStreamWriter(conn.getOutputStream(),"UTF-8");

            // 전송할 데이터. GET 방식으로 작성
            sendMsg = "id=" + strings[0] + "&pw=" + strings[1];

            osw.write(sendMsg);
            osw.flush();

            //jsp와 통신 성공 시 수행
            if (conn.getResponseCode() == conn.HTTP_OK) {
                InputStreamReader tmp = new InputStreamReader(conn.getInputStream(), "UTF-8");
                BufferedReader reader = new BufferedReader(tmp);
                StringBuffer buffer = new StringBuffer();

                // jsp에서 보낸 값을 받는 부분
                while ((str = reader.readLine()) != null) {
                    buffer.append(str);
                }
                receiveMsg = buffer.toString();
            } else {
                // 통신 실패
            }
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        //jsp로부터 받은 리턴 값
        return receiveMsg;
    }

}
```

수정해야 될 부분으로는 IP주소부분이다 주석에 설명된것을 참고하면 된다.

다음으로는 **MainActivity** 파일이다.

```java
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;

public class MainActivity extends AppCompatActivity {

    Button registerBtn;
    EditText idet, pwet;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        setTitle("ORACLE");

        registerBtn = (Button)findViewById(R.id.register_btn);
        idet = (EditText)findViewById(R.id.register_id);
        pwet = (EditText)findViewById(R.id.register_pw);

        registerBtn.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
                try {
                    Toast.makeText(MainActivity.this, "버튼눌림", Toast.LENGTH_SHORT).show();
                    String result;
                    String id = idet.getText().toString();
                    String pw = pwet.getText().toString();

                    RegisterActivity task = new RegisterActivity();
                    result = task.execute(id, pw).get();
                } catch (Exception e) {
                    Log.i("DBtest", ".....ERROR.....!");
                }
            }
        });

    }
}
```

여기서도 마찬가지로 크게 바꿀건 없다.

이제 값을 저장할 테이블을 만들어주자.

```sql
CREATE TABLE userTBL (	
    ID VARCHAR2(20), 
	PWD VARCHAR2(20)
);
```

JSP웹을 통해 디비로 들어가기 때문에 **매니페스트**에 퍼미션을 줘야한다.


![[Kotlin DB4.png]]


**<uses-permission android:name="android.permission.INTERNET"/>** 인터넷 접속을 허용하는 퍼미션이다.

세팅은 모두 완료되었다.

이제 이클립스에서 서버를 실행시켜주고 안드로이드 에뮬레이터 혹은 안드로이드 기기로 테스트를 해보자.

![[Kotlin DB5.png]]

회원가입 성공이라고 콘솔에 출력되는것을 확인할수있다. 데이터베이스도 확인해보자.

![[Kotlin DB6.png]]

**select * from userTBL;** 을 통해 확인해보니 정상적으로 들어가는것을 확인할수 있다.


## DB 시큐리티 설정


### 안드로이드 네트워크 보안 설정 변경

프로젝트에 **res/xml/network_security_config.xml** 파일을 생성하고 다음과 같이 설정해주세요.

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```

이렇게하면 앱은 Cleartext 통신을 허용합니다. 다만, 이 방법은 보안 측면에서는 안좋다고 합니다...

 하지만!! 저는 이 방법이 통하지 않았어요... 바로 다음 방법을 시도해봤습니다.

## AndroidManifest.xml에서 Cleartext 허용 설정 변경

**AndroidManifest.xml** 파일에 다음과 같이 설정을 추가했습니다.

```xml
<application
    ...
    android:usesCleartextTraffic="true"
    ...
</application>
```


이 방법을 시도했더니 드디어! get 요청을 성공적으로 보냈습니다...!

하지만 이 역시 보안상 좋지 않으니 되도록이면 https를 사용하시길!


# localhost, 10.0.2.2

위 방법을 다 시도했는데 안되시는 분들은

base url이 localhost인지 확인하셔야 합니다.

ip주소가 localhost 또는 127.0.0.1이신 분들은 아마 다음과 같은 에러 메세지를 마주하셨을겁니다

**java.net.ConnectException: Failed to connect to localhost/127.0.0.1:8080**

보통 로컬 서버는 localhost나 127.0.0.1 주소로 접근합니다.

하지만 안드로이드 스튜디오에서의 localhost는 **애뮬레이터**의 로컬이기 때문에 

localhost:포트번호가 아닌 **\http://10.0.2.2:포트번호** 로 base url을 설정해야합니다!

![[Pasted image 20240812134302.png]]


## 1단계

\<AndroidManifest.xml>

```xml
<?xml version="1.0" encoding="utf-8"?>
    <manifest ... >
        <application android:networkSecurityConfig="@xml/network_security_config"
                        ... >
            ...
        </application>
    </manifest>
```

## 2단계

방법 1) 믿을 수 있는 도메인만 추가하기

\<res/xml/network_security_config.xml>

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">test.com</domain>
    </domain-config>
</network-security-config>
    
```

방법 2) 모든 http연결을 허가하기

\<res/xml/network_security_config.xml>

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```


---
출처 - https://coinco.tistory.com/68 - DB 연동


https://hulrud.tistory.com/35 - DB 시큐리티 설정


https://sskey.tistory.com/53