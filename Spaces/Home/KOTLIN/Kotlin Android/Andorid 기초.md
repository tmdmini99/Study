

```kotlin
package com.example.myapplication  
  
import android.os.Bundle  
import androidx.activity.ComponentActivity  
import androidx.activity.compose.setContent  
import androidx.compose.foundation.layout.fillMaxSize  
import androidx.compose.material3.MaterialTheme  
import androidx.compose.material3.Surface  
import androidx.compose.material3.Text  
import androidx.compose.runtime.Composable  
import androidx.compose.ui.Modifier  
import androidx.compose.ui.tooling.preview.Preview  
import com.example.myapplication.ui.theme.MyApplicationTheme  
  
class MainActivity : ComponentActivity() {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContent {  
            MyApplicationTheme {  
                // A surface container using the 'background' color from the theme  
                Surface(modifier = Modifier.fillMaxSize(), color = MaterialTheme.colorScheme.background) {  
                    Greeting("Android")  
                }  
            }        }    }  
}  
  
@Composable  
fun Greeting(name: String, modifier: Modifier = Modifier) {  
    Text(  
        text = "Hello $name!",  
        modifier = modifier  
    )  
}  
  
@Preview(showBackground = true)  
@Composable  
fun GreetingPreview() {  
    MyApplicationTheme {  
        Greeting("Android")  
    }  
}
```



### 1. `class MainActivity : ComponentActivity()`

- **`MainActivity`**: 안드로이드 애플리케이션의 진입점이 되는 액티비티 클래스입니다. 이 클래스는 `ComponentActivity`를 상속받아 생성됩니다.
- **`ComponentActivity()`**: Jetpack Compose와 함께 사용할 수 있는 기본 액티비티입니다. `Activity`는 안드로이드 애플리케이션에서 사용자 인터페이스(UI)를 제공하는 주요 구성 요소 중 하나입니다.

### 2. `override fun onCreate(savedInstanceState: Bundle?)`

- **`onCreate`**: 안드로이드 액티비티의 생명주기 메서드 중 하나로, 액티비티가 처음 생성될 때 호출됩니다. 여기서 초기화 작업을 수행합니다.
- **`savedInstanceState: Bundle?`**: 이전에 액티비티가 저장한 상태를 포함하는 번들 객체입니다. 액티비티가 새로 생성된 경우 `null`일 수 있습니다.

### 3. `setContent { ... }`

- 이 메서드는 Compose에서 UI를 정의하기 위해 사용됩니다. `setContent` 블록 안에서 Compose의 Composable 함수들을 호출하여 화면을 구성합니다.

### 4. `MyApplicationTheme { ... }`

- **`MyApplicationTheme`**: 애플리케이션의 테마를 설정하는 Composable 함수입니다. 이 블록 안에서 정의된 UI 요소들은 지정된 테마를 따르게 됩니다.

### 5. `Surface(modifier = Modifier.fillMaxSize(), color = MaterialTheme.colorScheme.background)`

- **`Surface`**: Compose에서 UI 요소들을 담는 컨테이너 역할을 합니다. 이 경우 화면을 꽉 채우고, 테마의 배경 색상으로 색칠된 Surface를 생성합니다.
- **`Modifier.fillMaxSize()`**: UI 요소가 부모 요소의 전체 크기를 채우도록 설정하는 Modifier입니다.
- **`MaterialTheme.colorScheme.background`**: 테마에서 정의된 배경 색상을 가져옵니다.

### 6. `Greeting("Android")`

- **`Greeting`**: "Hello, [name]!" 문자열을 출력하는 Composable 함수입니다. 이 경우 `name`이 "Android"로 전달됩니다.

### 7. `@Composable`

- **`@Composable`**: 이 어노테이션은 함수가 Compose UI의 구성 요소인 것을 나타냅니다. Composable 함수는 UI를 선언적으로 정의할 수 있게 해줍니다.

### 8. `fun Greeting(name: String, modifier: Modifier = Modifier)`

- **`Greeting` 함수**: 전달된 `name` 매개변수를 사용해 "Hello [name]!" 텍스트를 표시하는 Composable 함수입니다.
- **`modifier`**: UI 요소의 모양, 크기 등을 설정할 수 있는 매개변수입니다. 기본값은 `Modifier`로 설정되어 있습니다.

### 9. `Text(text = "Hello $name!", modifier = modifier)`

- **`Text`**: 화면에 텍스트를 표시하는 Composable 함수입니다. `"Hello $name!"` 문자열이 표시되며, `$name`은 함수의 인자로 전달된 값으로 대체됩니다.

### 10. `@Preview(showBackground = true)`

- **`@Preview`**: 이 어노테이션은 해당 Composable 함수의 미리보기를 안드로이드 스튜디오에서 확인할 수 있도록 합니다. `showBackground = true`는 배경을 함께 표시할지를 설정하는 옵션입니다.

### 11. `fun GreetingPreview()`

- **`GreetingPreview`**: `Greeting` 함수의 동작을 미리보기 위한 Composable 함수입니다. "Android"라는 문자열을 전달해 미리보기에서 해당 화면을 확인할 수 있습니다.



#  ToDoList
## Layout 만들기

### app > res 우클릭 > new >  XML > Layout XML File 선택


![[Pasted image 20240809144747.png]]


### Layout  code
```xml
<?xml version="1.0" encoding="utf-8"?>  
<TextView xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content"  
    android:padding="20dp"  
    android:textStyle="bold">  
</TextView>
```


### activity_main.xml code

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context=".MainActivity">  
  
    <LinearLayout  
        android:id="@+id/bottom_section"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:layout_alignParentBottom="true"  
        android:orientation="horizontal"  
        android:padding="10dp">  
  
        <EditText  
            android:id="@+id/todo_edit"  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_marginRight="10dp"  
            android:layout_weight="1" />  
  
        <Button  
            android:id="@+id/add_btn"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="추가"  
            android:textSize="25sp" />  
    </LinearLayout>  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:layout_above="@id/bottom_section"  
        android:background="#eee">  
  
        <ListView  
            android:id="@+id/list_view"  
            android:layout_width="match_parent"  
            android:layout_height="match_parent" />  
    </LinearLayout>  
</RelativeLayout>
```


### MainActivity.kt code

```kotlin
package com.example.myapplication  
  
import android.graphics.Paint  
import android.os.Bundle  
import android.widget.*  
import androidx.activity.ComponentActivity  
import androidx.activity.compose.setContent  
import androidx.compose.foundation.layout.fillMaxSize  
import androidx.compose.material3.MaterialTheme  
import androidx.compose.material3.Surface  
import androidx.compose.material3.Text  
import androidx.compose.runtime.Composable  
import androidx.compose.ui.Modifier  
import androidx.compose.ui.tooling.preview.Preview  
import com.example.myapplication.ui.theme.MyApplicationTheme  
import androidx.appcompat.app.AppCompatActivity  
class MainActivity : AppCompatActivity() {  
  
    private lateinit var todoList: ArrayList<String>  
    private lateinit var adapter: ArrayAdapter<String>  
    private lateinit var todoEdit: EditText  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        // Initialize ArrayList and ArrayAdapter  
        todoList = ArrayList()  
        adapter = ArrayAdapter(this, R.layout.layout, todoList)  
  
        // Initialize UI components  
        val listView: ListView = this.findViewById(R.id.list_view)  
        val addBtn: Button = findViewById(R.id.add_btn)  
        todoEdit = findViewById(R.id.todo_edit)  
  
        // Set adapter  
        listView.adapter = adapter  
  
        // Set button click listener  
        addBtn.setOnClickListener {  
            addItem()  
        }  
  
        // Set ListView item click listener  
        listView.setOnItemClickListener { adapterView, view, i, l ->  
            val textView: TextView = view as TextView  
            // Strike through text  
            textView.paintFlags = textView.paintFlags or Paint.STRIKE_THRU_TEXT_FLAG  
        }  
  
        // Set content with Jetpack Compose  
//        setContent {  
//            MyApplicationTheme {  
//                Surface(modifier = Modifier.fillMaxSize(), color = MaterialTheme.colors.background) {  
//                    Greeting("Android 승민")  
//                }  
//            }  
//        }  
    }  
  
    private fun addItem() {  
        val todo: String = todoEdit.text.toString()  
        if (todo.isNotEmpty()) {  
            todoList.add(todo)  
            adapter.notifyDataSetChanged()  
        } else {  
            Toast.makeText(this, "할 일을 적어주세요", Toast.LENGTH_SHORT).show()  
        }  
    }  
}  
  
@Composable  
fun Greeting(name: String, modifier: Modifier = Modifier) {  
    Text(  
        text = "Hello $name!",  
        modifier = modifier  
    )  
}  
  
@Preview(showBackground = true)  
@Composable  
fun GreetingPreview() {  
    MyApplicationTheme {  
        Greeting("Android")  
    }  
}
```



---
https://github.com/code-with-joyce/must_have_android/blob/main/10_TodoList/gradle.properties 보고 따라하기


https://velog.io/@kevinkim2586/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-%EA%B0%80%EC%A7%80%EA%B3%A0-%EB%86%80%EA%B8%B0-4
