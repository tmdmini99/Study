v

```kotlin
import android.content.Context
import android.content.SharedPreferences

// CSRF 토큰을 저장하고 관리하는 클래스
class TokenManager(context: Context) {
    // SharedPreferences 인스턴스 생성
    private val sharedPreferences: SharedPreferences = context.getSharedPreferences("app_prefs", Context.MODE_PRIVATE)

    // CSRF 토큰 저장
    fun saveCsrfToken(token: String) {
        sharedPreferences.edit().putString("csrf_token", token).apply()
    }

    // 저장된 CSRF 토큰 가져오기
    fun getCsrfToken(): String? {
        return sharedPreferences.getString("csrf_token", null)
    }
}


```


로그인시  csrf 토큰 뽑아서  TokenManager에 저장

```kotlin
package com.example.myapplicationproject

import android.app.Activity
import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL

class LoginActivity : AppCompatActivity() {

    // TokenManager 인스턴스
    private lateinit var tokenManager: TokenManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        tokenManager = TokenManager(this)

        val usernameEditText = findViewById<EditText>(R.id.username)
        val passwordEditText = findViewById<EditText>(R.id.password)
        val loginButton = findViewById<Button>(R.id.login_button)
        val signUpButton = findViewById<Button>(R.id.signup_button)

        loginButton.setOnClickListener {
            val username = usernameEditText.text.toString()
            val password = passwordEditText.text.toString()

            if (username.isNotEmpty() && password.isNotEmpty()) {
                // 로그인 비동기 처리
                lifecycleScope.launch {
                    val result = loginUser(username, password)
                    if (result == "success") {
                        setResult(Activity.RESULT_OK)
                        finish()
                    } else {
                        Toast.makeText(this@LoginActivity, "로그인 실패", Toast.LENGTH_SHORT).show()
                    }
                }
            } else {
                Toast.makeText(this, "모든 필드를 입력해 주세요.", Toast.LENGTH_SHORT).show()
            }
        }

        signUpButton.setOnClickListener {
            val signUpIntent = Intent(this, SignUpActivity::class.java)
            startActivity(signUpIntent)
        }
    }

    private suspend fun loginUser(id: String, pw: String): String? {
        val sendMsg = "id=$id&pw=$pw"
        var receiveMsg: String? = null

        return withContext(Dispatchers.IO) {
            try {
                val url = URL("http://10.0.2.2:8080/android/login")
                val conn = url.openConnection() as HttpURLConnection
                conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")
                conn.requestMethod = "POST"

                OutputStreamWriter(conn.outputStream, "UTF-8").use { osw ->
                    osw.write(sendMsg)
                    osw.flush()
                }

                if (conn.responseCode == HttpURLConnection.HTTP_OK) {
                    // 응답 헤더에서 CSRF 토큰을 추출하여 저장
                    val csrfToken = conn.getHeaderField("X-CSRF-Token")
                    csrfToken?.let {
                        tokenManager.saveCsrfToken(it)
                    }

                    InputStreamReader(conn.inputStream, "UTF-8").use { tmp ->
                        BufferedReader(tmp).use { reader ->
                            val buffer = StringBuilder()
                            var line: String?
                            while (reader.readLine().also { line = it } != null) {
                                buffer.append(line)
                            }
                            receiveMsg = buffer.toString()
                        }
                    }
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
            receiveMsg
        }
    }
}


```




저장한 값을 로그인후 페이지 불러올때 사용
```kotlin
package com.example.myapplicationproject

import android.os.Bundle
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import java.io.BufferedReader
import java.io.InputStreamReader
import java.net.HttpURLConnection
import java.net.URL

class NoticeActivity : AppCompatActivity() {

    private lateinit var noticeTextView: TextView
    private lateinit var tokenManager: TokenManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_notice)

        noticeTextView = findViewById(R.id.notice_text)
        tokenManager = TokenManager(this)

        // 공지사항을 가져와서 표시
        lifecycleScope.launch {
            val notice = fetchNotice()
            noticeTextView.text = notice ?: "공지사항을 불러올 수 없습니다."
        }
    }

    private suspend fun fetchNotice(): String? {
        var receiveMsg: String? = null

        return withContext(Dispatchers.IO) {
            try {
                val url = URL("http://10.0.2.2:8080/android/notice")
                val conn = url.openConnection() as HttpURLConnection
                conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")
                conn.requestMethod = "GET"

                // CSRF 토큰을 요청 헤더에 추가
                val csrfToken = tokenManager.getCsrfToken()
                csrfToken?.let {
                    conn.setRequestProperty("X-CSRF-Token", it)
                }

                if (conn.responseCode == HttpURLConnection.HTTP_OK) {
                    InputStreamReader(conn.inputStream, "UTF-8").use { tmp ->
                        BufferedReader(tmp).use { reader ->
                            val buffer = StringBuilder()
                            var line: String?
                            while (reader.readLine().also { line = it } != null) {
                                buffer.append(line)
                            }
                            receiveMsg = buffer.toString()
                        }
                    }
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
            receiveMsg
        }
    }
}

```








---

앱 실행 시킬때 csrf 가져와서 로그인시에도 csrf 적용 시키기


MainActivity

```kotlin
package com.example.myapplicationproject

import android.app.Activity
import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.result.ActivityResultLauncher
import androidx.activity.result.contract.ActivityResultContracts
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {

    private lateinit var loginResultLauncher: ActivityResultLauncher<Intent>
    private lateinit var tokenManager: TokenManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        tokenManager = TokenManager(this)

        // ActivityResultLauncher 초기화
        loginResultLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
            if (result.resultCode == Activity.RESULT_OK) {
                // 로그인 성공 시 공지사항을 가져옴
                val noticeIntent = Intent(this, NoticeActivity::class.java)
                startActivity(noticeIntent)
            } else {
                Toast.makeText(this, "로그인 실패", Toast.LENGTH_SHORT).show()
            }
        }

        lifecycleScope.launch {
            // 메인 페이지 로드 시 CSRF 토큰을 서버에서 가져오기
            val csrfToken = fetchCsrfToken()
            csrfToken?.let {
                tokenManager.saveCsrfToken(it)
            }
        }

        // 로그인 Activity를 실행
        val intent = Intent(this, LoginActivity::class.java)
        loginResultLauncher.launch(intent)
    }

    private suspend fun fetchCsrfToken(): String? {
        return withContext(Dispatchers.IO) {
            var token: String? = null
            try {
                val url = URL("http://10.0.2.2:8080/get-csrf-token") // CSRF 토큰을 제공하는 엔드포인트
                val conn = url.openConnection() as HttpURLConnection
                conn.requestMethod = "GET"

                if (conn.responseCode == HttpURLConnection.HTTP_OK) {
                    InputStreamReader(conn.inputStream, "UTF-8").use { tmp ->
                        BufferedReader(tmp).use { reader ->
                            val buffer = StringBuilder()
                            var line: String?
                            while (reader.readLine().also { line = it } != null) {
                                buffer.append(line)
                            }
                            token = buffer.toString()
                        }
                    }
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
            token
        }
    }
}

```


LoginActivity

```kotlin
package com.example.myapplicationproject

import android.app.Activity
import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL

class LoginActivity : AppCompatActivity() {

    private lateinit var tokenManager: TokenManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        tokenManager = TokenManager(this)

        val usernameEditText = findViewById<EditText>(R.id.username)
        val passwordEditText = findViewById<EditText>(R.id.password)
        val loginButton = findViewById<Button>(R.id.login_button)
        val signUpButton = findViewById<Button>(R.id.signup_button)

        loginButton.setOnClickListener {
            val username = usernameEditText.text.toString()
            val password = passwordEditText.text.toString()

            if (username.isNotEmpty() && password.isNotEmpty()) {
                lifecycleScope.launch {
                    val csrfToken = tokenManager.getCsrfToken()
                    val result = loginUser(username, password, csrfToken)
                    if (result == "success") {
                        setResult(Activity.RESULT_OK)
                        finish()
                    } else {
                        Toast.makeText(this@LoginActivity, "로그인 실패", Toast.LENGTH_SHORT).show()
                    }
                }
            } else {
                Toast.makeText(this, "모든 필드를 입력해 주세요.", Toast.LENGTH_SHORT).show()
            }
        }

        signUpButton.setOnClickListener {
            val signUpIntent = Intent(this, SignUpActivity::class.java)
            startActivity(signUpIntent)
        }
    }

    private suspend fun loginUser(id: String, pw: String, csrfToken: String?): String? {
        val sendMsg = "id=$id&pw=$pw"
        var receiveMsg: String? = null

        return withContext(Dispatchers.IO) {
            try {
                val url = URL("http://10.0.2.2:8080/android/login")
                val conn = url.openConnection() as HttpURLConnection
                conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")
                conn.setRequestProperty("X-CSRF-Token", csrfToken ?: "")
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
                            }
                            receiveMsg = buffer.toString()
                        }
                    }
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
            receiveMsg
        }
    }
}

```

TokenManager

```kotlin
package com.example.myapplicationproject

import android.content.Context
import android.content.SharedPreferences

class TokenManager(context: Context) {
    private val prefs: SharedPreferences = context.getSharedPreferences("TokenPrefs", Context.MODE_PRIVATE)

    fun saveCsrfToken(token: String) {
        prefs.edit().putString("csrf_token", token).apply()
    }

    fun getCsrfToken(): String? {
        return prefs.getString("csrf_token", null)
    }
}

```