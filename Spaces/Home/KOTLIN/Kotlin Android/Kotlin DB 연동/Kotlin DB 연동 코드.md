
MainActivity
```kotlin
package com.example.myapplication  
import RegisterActivity  
import android.os.Bundle  
import android.util.Log  
import android.view.View  
import android.widget.Button  
import android.widget.EditText  
import android.widget.Toast  
import androidx.appcompat.app.AppCompatActivity  
import com.example.myapplication.R  
import kotlinx.coroutines.CoroutineScope  
import kotlinx.coroutines.Dispatchers  
import kotlinx.coroutines.launch  
  
class MainActivity : AppCompatActivity() {  
  
    private lateinit var registerBtn: Button  
    private lateinit var idet: EditText  
    private lateinit var pwet: EditText  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        title = "ORACLE"  
  
        registerBtn = findViewById(R.id.register_btn)  
        idet = findViewById(R.id.register_id)  
        pwet = findViewById(R.id.register_pw)  
  
        registerBtn.setOnClickListener {  
            val id = idet.text.toString()  
            val pw = pwet.text.toString()  
  
            CoroutineScope(Dispatchers.Main).launch {  
                try {  
                    Toast.makeText(this@MainActivity, "버튼눌림", Toast.LENGTH_SHORT).show()  
                    val result = RegisterActivity().registerUser(id, pw)  
                    Log.d("DBtest", "결과: $result")  
  
                } catch (e: Exception) {  
                    Log.i("DBtest", ".....ERROR.....!", e)  
                }  
            }  
        }    }  
}
```

RegisterActivity

```kotlin
import kotlinx.coroutines.Dispatchers  
import kotlinx.coroutines.withContext  
import java.io.BufferedReader  
import java.io.InputStreamReader  
import java.io.OutputStreamWriter  
import java.net.HttpURLConnection  
import java.net.URL  
  
class RegisterActivity {  
  
    suspend fun registerUser(id: String, pw: String): String? {  
        var sendMsg = "id=$id&pw=$pw"  
        var receiveMsg: String? = null  
  
        return withContext(Dispatchers.IO) {  
            try {  
                val url = URL("http://10.0.2.2:8080/android")  
                val conn = url.openConnection() as HttpURLConnection  
                conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")  
                conn.requestMethod = "POST"  
  
                OutputStreamWriter(conn.outputStream, "UTF-8").use { osw ->  
                    osw.write(sendMsg)  
                    osw.flush()  
                }  
  
                if (conn.responseCode == HttpURLConnection.HTTP_OK) {  
                    InputStreamReader(conn.inputStream, "UTF-8").use { tmp ->  
                        BufferedReader(tmp).use { reader ->  
                            val buffer = StringBuilder()  
                            var line: String?  
                            while (reader.readLine().also { line = it } != null) {  
                                buffer.append(line)  
                            }                            receiveMsg = buffer.toString()  
                        }  
                    }                }  
            } catch (e: Exception) {  
                e.printStackTrace()  
            }  
            receiveMsg  
        }  
    }  
}
```
### `10.0.2.2`의 용도

- **호스트 컴퓨터 접근**: Android Emulator는 가상 네트워크를 사용하여 실행됩니다. `10.0.2.2`는 이 가상 네트워크에서 호스트 컴퓨터의 `localhost`(127.0.0.1) 또는 `loopback` 주소를 참조합니다. 즉, Android Emulator 내에서 `10.0.2.2`를 사용하면 호스트 컴퓨터에서 실행 중인 서버나 서비스에 접근할 수 있습니다.




layout > activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    tools:context=".MainActivity">  
  
    <TableLayout  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:stretchColumns="1">  
  
        <TableRow  
            android:layout_width="match_parent"  
            android:layout_height="wrap_content">  
  
            <TextView  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:layout_weight="1"  
                android:text="ID"  
                android:textAlignment="center"  
                android:textAppearance="@style/TextAppearance.AppCompat.Headline" />  
  
            <EditText  
                android:id="@+id/register_id"  
                android:layout_width="0dp"  
                android:layout_height="wrap_content"  
                android:layout_weight="8" />  
  
        </TableRow>  
  
        <TableRow  
            android:layout_width="match_parent"  
            android:layout_height="wrap_content">  
  
            <TextView  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:layout_weight="1"  
                android:text="PW"  
                android:textAlignment="center"  
                android:textAppearance="@style/TextAppearance.AppCompat.Headline" />  
  
            <EditText  
                android:id="@+id/register_pw"  
                android:layout_width="0dp"  
                android:layout_height="wrap_content"  
                android:layout_weight="8" />  
  
        </TableRow>  
  
        <TableRow  
            android:layout_width="match_parent"  
            android:layout_height="wrap_content"  
            android:gravity="center_horizontal">  
  
            <Button  
                android:id="@+id/register_btn"  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="회원가입" />  
        </TableRow>  
  
    </TableLayout>  
  
</LinearLayout>
```




layout > layout.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content"  
    android:orientation="horizontal"  
    android:padding="10dp">  
<TextView xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content"  
    android:padding="20dp"  
    android:textStyle="bold">  
</TextView>  
  
<Button  
        android:id="@+id/delete_btn"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="삭제"  
        android:textSize="16sp"/>  
  
    </LinearLayout>
```



AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
          xmlns:tools="http://schemas.android.com/tools">  
  
    <uses-permission android:name="android.permission.INTERNET" />  
    <application  
            android:allowBackup="true"  
            android:dataExtractionRules="@xml/data_extraction_rules"  
            android:fullBackupContent="@xml/backup_rules"  
            android:icon="@mipmap/ic_launcher"  
            android:label="@string/app_name"  
            android:roundIcon="@mipmap/ic_launcher_round"  
            android:supportsRtl="true"  
            android:theme="@style/Theme.MyApplication"  
            android:networkSecurityConfig="@xml/network_security_config"  
            tools:targetApi="31">  
        <activity  
                android:name=".MainActivity"  
                android:exported="true"  
                android:label="@string/app_name"  
                android:theme="@style/Theme.MyApplication">  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN"/>  
  
                <category android:name="android.intent.category.LAUNCHER"/>  
            </intent-filter>  
        </activity>  
    </application>  
  
</manifest>
```


xml > network_security_config.xml
network_security_config.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<network-security-config xmlns:android="http://schemas.android.com/apk/res/android">  
    <domain-config cleartextTrafficPermitted="true">  
        <domain includeSubdomains="true">10.0.2.2</domain>  
        <domain includeSubdomains="true">localhost</domain>  
    </domain-config>  
</network-security-config>

```


values > styles.xml

```xml
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <!-- Base application theme -->  
    <style name="Theme.MyApplication" parent="Theme.MaterialComponents.DayNight.NoActionBar">  
        <!-- Primary brand color -->  
        <item name="colorPrimary">@color/purple_500</item>  
        <!-- Darker variant of the primary color -->  
        <item name="colorPrimaryVariant">@color/purple_700</item>  
        <!-- Color for text/icons on top of primary color -->  
        <item name="colorOnPrimary">@color/white</item>  
        <!-- Secondary brand color -->  
        <item name="colorSecondary">@color/teal_200</item>  
        <!-- Darker variant of the secondary color -->  
        <item name="colorSecondaryVariant">@color/teal_700</item>  
        <!-- Color for text/icons on top of secondary color -->  
        <item name="colorOnSecondary">@color/black</item>  
        <!-- Color for text/icons on top of background color -->  
        <item name="colorOnBackground">@color/black</item>  
        <!-- Surface color (e.g., cards, dialogs) -->  
        <item name="colorSurface">@color/white</item>  
        <!-- Color for text/icons on top of surface color -->  
        <item name="colorOnSurface">@color/black</item>  
    </style>  
</resources>
```
