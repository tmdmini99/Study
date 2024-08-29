
AlertDialog는 사용자의 전체 화면을 가리지 않으면서 사용자의 응답이나 추가 정보를 입력하도록 하는 작은 창을 의미합니다.

### **1. AlertDialog 생성하기**

AlertDialog.Builder 객체를 생성하여 AlertDialog의 다양한 디자인을 구축할 수 있습니다. 먼저 AlertDialog를 생성하는 가장 기본적인 형태를 살펴보겠습니다. 먼저 예제의 메인 화면이 되는 Activity의 XML 레이아웃 리소스입니다.
리소스입니다.

```xml
    <LinearLayout
        android:id="@+id/layout"
        android:layout_width="368dp"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="8dp">

        <Button
            android:id="@+id/button3"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:onClick="OnClickHandler"
            android:text="다이얼로그 창 띄우기" />
    </LinearLayout>
```


▼ Button 한 개를 배치한 간단한 UI입니다. Button을 클릭하였을 때 다이얼로그 창이 뜨도록 구현할 것이며 아래에 나오는 예제들도 특수한 경우가 아닌 이상 해당 XML 레이아웃 리소스를 사용할 것입니다.

java
```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setTitle("인사말").setMessage("반갑습니다");

        AlertDialog alertDialog = builder.create();

        alertDialog.show();
    }
}
```


```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onClickHandler(view: View) {
        val builder = AlertDialog.Builder(this)
        builder.setTitle("인사말").setMessage("반갑습니다")
        val alertDialog = builder.create()
        alertDialog.show()
    }
}
```




▼ AlertDialog에 여러정보를 Setting을 하려면 먼저 AlertDialog.Buider 객체를 생성합니다. Builder 클래스에서 제공하는 다양한 함수들을 사용하여 AlertDialog에 여러 정보를 Setting 하는데 위 예제에서는 setTitle() 함수를 사용하여 다이얼로그 창의 제목을 설정하고 setMessage() 함수를 통해 다이얼로그 창을 통해 사용자에게 보여주고 싶은 Message를 지정합니다. Setting이 끝나면 create() 함수를 통해 AlertDialog 객체를 생성하고 show() 함수를 통해 다이얼로그 창을 화면에 보여주게 됩니다.


![[Dialog1.png]]



### **2. AlertDialog에 버튼 추가하기**

AlertDialog는 3개의 버튼을 추가하여 사용자가 특정한 상황에 대하여 선택적으로 앱의 기능을 수행할수 있도록 하는 게 가능합니다. 

java
```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setTitle("버튼 추가 예제").setMessage("선택하세요.");

        builder.setPositiveButton("OK", new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int id)
            {
                Toast.makeText(getApplicationContext(), "OK Click", Toast.LENGTH_SHORT).show();
            }
        });

        builder.setNegativeButton("Cancel", new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int id)
            {
                Toast.makeText(getApplicationContext(), "Cancel Click", Toast.LENGTH_SHORT).show();
            }
        });

        builder.setNeutralButton("Neutral", new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int id)
            {
                Toast.makeText(getApplicationContext(), "Neutral Click", Toast.LENGTH_SHORT).show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```

kotlin
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onClickHandler(view: View) {
        val builder = AlertDialog.Builder(this)
        builder.setTitle("버튼 추가 예제").setMessage("선택하세요.")

        builder.setPositiveButton("OK") { _, _ ->
            Toast.makeText(applicationContext, "OK Click", Toast.LENGTH_SHORT).show()
        }

        builder.setNegativeButton("Cancel") { _, _ ->
            Toast.makeText(applicationContext, "Cancel Click", Toast.LENGTH_SHORT).show()
        }

        builder.setNeutralButton("Neutral") { _, _ ->
            Toast.makeText(applicationContext, "Neutral Click", Toast.LENGTH_SHORT).show()
        }

        val alertDialog = builder.create()
        alertDialog.show()
    }
}

```


▼ 각 버튼을 추가를 할 때는 setPositiveButton(), setNegativeButton(), setNeutralButton() 함수를 통해서 추가를 할 수 있습니다. 인자 정보는 첫 번째 인자로 Button의 Text 속성으로 지정할 문자열 값 또는 문자열 리소스 ID 값을 넘겨줍니다.

두 번째 인자는 해당 버튼을 클릭하였을 때 이벤트 처리를 위한 DialogInterface.OnClickListener 구현체를 넘겨줍니다. 위 예제에서는 익명 클래스 생성 방식으로 구현하였으며 onClick() 함수를 오버 라이딩하여 그 안에 해당 버튼을 클릭하였을 때 처리할 동작을 정의하면 됩니다. 예제에서는 각 버튼을 때 Toast 메시지를 통해 어떤 버튼이 클릭되었는지를 알려주는 구문을 작성하였습니다.


![[Dialog2.png]]




### **3. 단일 선택 목록 (List) 추가하기**

먼저 다이얼로그 창에 Setting 할 문자열 배열 리소스를 /res/values/strings.xml 경로 밑에 추가합니다. 

```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="title_activity_sub">SubActivity</string>

    <string-array name = "LAN">
        <item>"Android"</item>
        <item>"Java"</item>
        <item>"C/C++"</item>
    </string-array>
</resources>
```

▼ "LAN" 이라는 문자열 배열 리소스를 추가하였습니다.\

java

```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setTitle("리스트 추가 예제");

        builder.setItems(R.array.LAN, new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int pos)
            {
                String[] items = getResources().getStringArray(R.array.LAN);
                Toast.makeText(getApplicationContext(),items[pos],Toast.LENGTH_LONG).show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```

kotlin
```kotlin
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("리스트 추가 예제");

        builder.setItems(R.array.LAN, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int pos) {
                String[] items = getResources().getStringArray(R.array.LAN);
                Toast.makeText(getApplicationContext(), items[pos], Toast.LENGTH_LONG).show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}

```



▼ 리스트(List)를 추가하기 위해서는 **setItems()** 함수를 사용합니다. 첫 번째 인자로 다이얼로그 창에 지정할 List입니다. 해당 예제에서는 앞서 정의한 문자열 배열 리소스 ID 값을 넘겨줍니다. 두 번째 인자는 항목을 클릭하였을 때 이벤트 처리를 위한 **DialogInterface.OnClickListener()** 구현체입니다. onClick() 함수의 두 번째 매개변수로 선택 된 항목의 Index 값이 넘어오기 때문에 **getResources(). getStringArray()를** 통해 문자열 배열을 받아와 선택된 항목을 Toast 메시지로 출력하도록 하였습니다.


![[Dialog3.png]]



### **4. 다중 선택 목록 추가 - Checkbox**


java
```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        final ArrayList<String> selectedItems = new ArrayList<String>();
        final String[] items = getResources().getStringArray(R.array.LAN);

        builder.setTitle("리스트 추가 예제");

        builder.setMultiChoiceItems(R.array.LAN, null, new DialogInterface.OnMultiChoiceClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int pos, boolean isChecked)
            {
                if(isChecked == true) // Checked 상태일 때 추가
                {
                    selectedItems.add(items[pos]);
                }
                else				  // Check 해제 되었을 때 제거
                {
                    selectedItems.remove(pos);
                }
            }
        });

        builder.setPositiveButton("OK", new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int pos)
            {
                String SeletedItemsString = "";

                for(int i =0; i<selectedItems.size();i++)
                {
                    SeletedItemsString =  SeletedItemsString + "," + selectedItems.get(i);
                }

                Toast toast = Toast.makeText(getApplicationContext(), "선택 된 항목은 :" + SeletedItemsString,Toast.LENGTH_LONG);
                toast.setGravity(Gravity.CENTER, 0, 0);
                toast.show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```


kotlin
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onClickHandler(view: View) {
        val builder = AlertDialog.Builder(this)
        val selectedItems = ArrayList<String>()
        val items = resources.getStringArray(R.array.LAN)

        builder.setTitle("리스트 추가 예제")

        builder.setMultiChoiceItems(R.array.LAN, null) { _, pos, isChecked ->
            if (isChecked) {
                selectedItems.add(items[pos])
            } else {
                selectedItems.remove(items[pos])
            }
        }

        builder.setPositiveButton("OK") { _, _ ->
            val selectedItemsString = selectedItems.joinToString(", ")
            Toast.makeText(applicationContext, "선택 된 항목은 : $selectedItemsString", Toast.LENGTH_LONG).show()
        }

        val alertDialog = builder.create()
        alertDialog.show()
    }
}

```





▼ 다중 선택 리스트를 적용하기 위해서는 **setMultiChoiceItems()** 함수를 통해 지정합니다. 세 번째 인자로 리스너 구현체를 넘겨주는데 **onClick()** 함수를 오버라이딩 하여 각 항목이 선택되었을 때 이벤트를 처리합니다. onClick() 세 번째 인자로 넘어오는 Checked 여부를 사용하여 선택된 항목을 ArrayList에 추가하거나 제거하는 구문이 들어갑니다. 

▼ **setPositiveButton()** 함수를 통해 버튼을 추가합니다. 버튼을 클릭하였을 때는 **selectedItems**로부터 선택 된 항목들을 하나의 문자열 데이터로 변경하여 Toast 메시지를 통해 화면에 보여줍니다.


![[Dialog4.png]]

### **5. 단일 선택 목록 추가 - 라디오 버튼(Radio Button)**

java
```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        final String[] items = getResources().getStringArray(R.array.LAN);
        final ArrayList<String> selectedItem  = new ArrayList<String>();
        selectedItem.add(items[0]);

        builder.setTitle("리스트 추가 예제");

        builder.setSingleChoiceItems(R.array.LAN, 0, new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int pos)
            {
                selectedItem.clear();
                selectedItem.add(items[pos]);
            }
        });

        builder.setPositiveButton("OK", new DialogInterface.OnClickListener(){
            @Override
            public void onClick(DialogInterface dialog, int pos)
            {
                Toast toast = Toast.makeText(getApplicationContext(), "선택된 항목 : " + selectedItem.get(0), Toast.LENGTH_LONG);
                toast.setGravity(Gravity.CENTER, 0, 0);
                toast.show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```

kotlin
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onClickHandler(view: View) {
        val builder = AlertDialog.Builder(this)
        val items = resources.getStringArray(R.array.LAN)
        val selectedItem = arrayListOf(items[0])

        builder.setTitle("리스트 추가 예제")

        builder.setSingleChoiceItems(items, 0) { _, pos ->
            selectedItem.clear()
            selectedItem.add(items[pos])
        }

        builder.setPositiveButton("OK") { _, _ ->
            val toast = Toast.makeText(applicationContext, "선택된 항목 : ${selectedItem[0]}", Toast.LENGTH_LONG)
            toast.setGravity(Gravity.CENTER, 0, 0)
            toast.show()
        }

        val alertDialog = builder.create()
        alertDialog.show()
    }
}

```


▼ 단일 선택 목록을 지정하기 위해서는 **setSingleChoiceItems()** 함수를 사용합니다. 두 번째 인자로 다이얼로그 창이 열렸을 때 default로 선택된 항목의 index 값을 지정하고 마찬가지로 항목이 선택되었을 때 이벤트 처리를 위해 리스너 구현체를 넘겨줍니다. 단일 항목 선택이므로 항목이 클릭되었을 때 ArrayList를 **clear()** 함수를 통해 초기화하고 새로 선택 된 항목을 추가합니다. 

---

### **6. 커스텀 다이얼로그(Custom Dialog) 창 만들기**

다이얼로그에 사용자 지정 레이아웃을 지정할 수 있습니다. 먼저 다이얼로그에 지정할 레이아웃 리소스를 정의합니다.

```xml
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/textView4"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="이름" />

            <EditText
                android:id="@+id/name"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:ems="10"
                android:inputType="textPersonName"
                android:text="Name" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/textView5"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="별명" />

            <EditText
                android:id="@+id/nickname"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:ems="10"
                android:inputType="textPersonName"
                android:text="Name" />
        </LinearLayout>
    </LinearLayout>
```

java
```java
public class MainActivity extends AppCompatActivity{

    private CharSequence list;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void OnClickHandler(View view)
    {
        View dialogView = getLayoutInflater().inflate(R.layout.activity_sub, null);
        final EditText nameEditText = (EditText)dialogView.findViewById(R.id.name);
        final EditText NicknameEditText = (EditText)dialogView.findViewById(R.id.nickname);

        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setView(dialogView);

        builder.setPositiveButton("OK", new DialogInterface.OnClickListener(){
            public void onClick(DialogInterface dialog, int pos)
            {
                String name = "이름은 : " + nameEditText.getText().toString();
                String nickname = "별명은 : " + NicknameEditText.getText().toString();

                Toast.makeText(getApplicationContext(),name + "\n" + nickname, Toast.LENGTH_LONG).show();
            }
        });

        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```

kotlin
```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onClickHandler(view: View) {
        val dialogView = layoutInflater.inflate(R.layout.activity_sub, null)
        val nameEditText = dialogView.findViewById<EditText>(R.id.name)
        val nicknameEditText = dialogView.findViewById<EditText>(R.id.nickname)

        val builder = AlertDialog.Builder(this)
        builder.setView(dialogView)

        builder.setPositiveButton("OK") { _, _ ->
            val name = "이름은 : ${nameEditText.text.toString()}"
            val nickname = "별명은 : ${nicknameEditText.text.toString()}"

            Toast.makeText(applicationContext, "$name\n$nickname", Toast.LENGTH_LONG).show()
        }

        val alertDialog = builder.create()
        alertDialog.show()
    }
}

```

▼ 먼저 Inflater를 통해 앞서 정의한 레이아웃을 View 객체로 받아와 dialogView가 참조하도록 합니다. 다이얼로그 창에 사용자 지정 레이아웃을 적용하기 위해서는 setView() 함수를 사용합니다. 인자는 dialogView를 넘겨줍니다. 그다음으로 버튼을 추가하여 해당 버튼을 클릭하였을 때 다이얼로그의 EditText의 내용을 Toast로 보여주도록 하였습니다.




















---
출처 - https://lktprogrammer.tistory.com/155