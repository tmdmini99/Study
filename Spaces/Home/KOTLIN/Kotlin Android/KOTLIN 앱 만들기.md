
signup.kt

실제 환경에서 사용

```
package com.example.myapplicationproject  
  
import android.content.Intent  
import android.os.Bundle  
import android.util.Log  
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
import java.net.URL  
import javax.net.ssl.HttpsURLConnection  
import javax.net.ssl.SSLContext  
  
class SignUpActivity : AppCompatActivity() {  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_signup)  
  
        val usernameEditText = findViewById<EditText>(R.id.username)  
        val passwordEditText = findViewById<EditText>(R.id.password)  
        val signUpButton = findViewById<Button>(R.id.signup_button)  
        val checkUsernameButton = findViewById<Button>(R.id.check_username_button)  
  
        var isUsernameChecked = false  
  
        checkUsernameButton.setOnClickListener {  
            val username = usernameEditText.text.toString()  
            if (username.isNotEmpty()) {  
                lifecycleScope.launch {  
                    val result = checkUsername(username)  
                    if (result) {  
                        isUsernameChecked = true  
                        Toast.makeText(this@SignUpActivity, "사용 가능한 아이디입니다.", Toast.LENGTH_SHORT).show()  
                    } else {  
                        isUsernameChecked = false  
                        Toast.makeText(this@SignUpActivity, "이미 사용 중인 아이디입니다.", Toast.LENGTH_SHORT).show()  
                    }  
                }  
            } else {  
                Toast.makeText(this, "아이디를 입력해 주세요.", Toast.LENGTH_SHORT).show()  
            }  
        }  
  
        signUpButton.setOnClickListener {  
            val username = usernameEditText.text.toString()  
            val password = passwordEditText.text.toString()  
  
            if (username.isNotEmpty() && password.isNotEmpty()) {  
                if (isUsernameChecked) {  
                    lifecycleScope.launch {  
                        val result = signUpUser(username, password)  
                        Log.d("SignUpActivity", "SignUp result: $result")  
                        if (result == "success") {  
                            Toast.makeText(this@SignUpActivity, "회원가입 성공", Toast.LENGTH_SHORT).show()  
                            // 로그인 화면으로 이동  
                            val intent = Intent(this@SignUpActivity, LoginActivity::class.java)  
                            startActivity(intent)  
                            finish()  // 현재 액티비티 종료  
                        } else {  
                            Toast.makeText(this@SignUpActivity, "회원가입 실패", Toast.LENGTH_SHORT).show()  
                        }  
                    }  
                } else {  
                    Toast.makeText(this, "아이디 중복 체크를 완료해 주세요.", Toast.LENGTH_SHORT).show()  
                }  
            } else {  
                Toast.makeText(this, "모든 필드를 입력해 주세요.", Toast.LENGTH_SHORT).show()  
            }  
        }  
    }  
  
    private suspend fun checkUsername(username: String): Boolean {  
        return withContext(Dispatchers.IO) {  
            var isAvailable = false  
            try {  
                val url = URL("https://10.0.2.2:8443/android/check")  
                val connection = (url.openConnection() as HttpsURLConnection).apply {  
                    val sslContext = SSLContext.getInstance("TLSv1.2")  
                    sslContext.init(null, null, java.security.SecureRandom())  
                    sslSocketFactory = sslContext.socketFactory  
                    setRequestProperty("Content-Type", "application/x-www-form-urlencoded")  
                    requestMethod = "POST"  
                    doOutput = true  
                }  
  
                connection.outputStream.use { outputStream ->  
                    OutputStreamWriter(outputStream, "UTF-8").use { writer ->  
                        writer.write("id=$username")  
                        writer.flush()  
                    }  
                }  
                if (connection.responseCode == HttpsURLConnection.HTTP_OK) {  
                    connection.inputStream.use { inputStream ->  
                        InputStreamReader(inputStream, "UTF-8").use { reader ->  
                            BufferedReader(reader).use { bufferedReader ->  
                                val result = bufferedReader.readLine()  
                                isAvailable = result.toBoolean()  // 서버 응답을 Boolean으로 변환  
                            }  
                        }                    }                } else {  
                    Log.e("SignUpActivity", "Check Username Error: ${connection.responseCode} ${connection.responseMessage}")  
                }  
            } catch (e: Exception) {  
                Log.e("SignUpActivity", "Check Username Exception: ${e.localizedMessage}", e)  
            }  
            isAvailable  
        }  
    }  
  
    private suspend fun signUpUser(id: String, pw: String): String? {  
        return withContext(Dispatchers.IO) {  
            var result: String? = null  
            try {  
                val url = URL("https://10.0.2.2:8443/android")  
                val connection = (url.openConnection() as HttpsURLConnection).apply {  
                    val sslContext = SSLContext.getInstance("TLSv1.2")  
                    sslContext.init(null, null, java.security.SecureRandom())  
                    sslSocketFactory = sslContext.socketFactory  
                    setRequestProperty("Content-Type", "application/x-www-form-urlencoded")  
                    requestMethod = "POST"  
                    doOutput = true  
                }  
  
                connection.outputStream.use { outputStream ->  
                    OutputStreamWriter(outputStream, "UTF-8").use { writer ->  
                        writer.write("id=$id&pw=$pw")  
                        writer.flush()  
                    }  
                }  
                if (connection.responseCode == HttpsURLConnection.HTTP_OK) {  
                    connection.inputStream.use { inputStream ->  
                        InputStreamReader(inputStream, "UTF-8").use { reader ->  
                            BufferedReader(reader).use { bufferedReader ->  
                                result = bufferedReader.readLine()  
                            }  
                        }                    }                } else {  
                    Log.e("SignUpActivity", "SignUp Error: ${connection.responseCode} ${connection.responseMessage}")  
                }  
            } catch (e: Exception) {  
                Log.e("SignUpActivity", "SignUp Exception: ${e.localizedMessage}", e)  
            }  
            result  
        }  
    }  
}
```




---
출처 - https://velog.io/@jihyeon9975/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9C%BC%EB%A1%9C-%ED%95%98%EB%8A%94-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%B1-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-1-


