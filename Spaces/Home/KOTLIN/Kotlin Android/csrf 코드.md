

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


처리방법 

```kotlin
import android.content.Context
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.withContext
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL

// API 호출을 처리하는 클래스
class ApiService(private val context: Context) {

    // TokenManager 인스턴스 생성
    private val tokenManager = TokenManager(context)

    // 사용자 로그인 메소드
    suspend fun loginUser(username: String, password: String): String? {
        return withContext(Dispatchers.IO) {
            var responseMessage: String? = null
            try {
                val url = URL("http://10.0.2.2:8080/login")
                val connection = url.openConnection() as HttpURLConnection
                connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")
                connection.requestMethod = "POST"
                connection.doOutput = true

                // 로그인 요청 데이터 작성
                val postData = "username=$username&password=$password"
                connection.outputStream.use { outputStream ->
                    OutputStreamWriter(outputStream, "UTF-8").use { writer ->
                        writer.write(postData)
                        writer.flush()
                    }
                }

                // 응답 코드가 HTTP_OK일 경우
                if (connection.responseCode == HttpURLConnection.HTTP_OK) {
                    // 응답 헤더에서 CSRF 토큰 추출
                    val csrfToken = connection.getHeaderField("X-CSRF-Token")
                    csrfToken?.let {
                        // CSRF 토큰 저장
                        tokenManager.saveCsrfToken(it)
                    }

                    // 응답 바디 읽기
                    connection.inputStream.use { inputStream ->
                        BufferedReader(InputStreamReader(inputStream, "UTF-8")).use { reader ->
                            responseMessage = reader.readText()
                        }
                    }
                } else {
                    responseMessage = "Error: ${connection.responseCode}"
                }
            } catch (e: Exception) {
                e.printStackTrace()
                responseMessage = "Error: ${e.message}"
            } finally {
                connection.disconnect()
            }
            responseMessage
        }
    }

    // CSRF 토큰을 포함한 요청 메소드
    suspend fun makeRequestWithCsrfToken(): String? {
        val csrfToken = tokenManager.getCsrfToken()
        return withContext(Dispatchers.IO) {
            var responseMessage: String? = null
            try {
                val url = URL("http://10.0.2.2:8080/some_endpoint")
                val connection = url.openConnection() as HttpURLConnection
                connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded")
                // 저장된 CSRF 토큰을 요청 헤더에 추가
                csrfToken?.let {
                    connection.setRequestProperty("X-CSRF-Token", it)
                }
                connection.requestMethod = "POST"
                connection.doOutput = true

                // 데이터 전송
                connection.outputStream.use { outputStream ->
                    OutputStreamWriter(outputStream, "UTF-8").use { writer ->
                        writer.write("data=example")
                        writer.flush()
                    }
                }

                // 응답 코드가 HTTP_OK일 경우
                if (connection.responseCode == HttpURLConnection.HTTP_OK) {
                    connection.inputStream.use { inputStream ->
                        BufferedReader(InputStreamReader(inputStream, "UTF-8")).use { reader ->
                            responseMessage = reader.readText()
                        }
                    }
                } else {
                    responseMessage = "Error: ${connection.responseCode}"
                }
            } catch (e: Exception) {
                e.printStackTrace()
                responseMessage = "Error: ${e.message}"
            } finally {
                connection.disconnect()
            }
            responseMessage
        }
    }
}

```


```kotlin

```