![[Kotlin 개념1.png]]



![[Kotlin개념3.png]]




Activity  
- 앱의 한 화면이다  
  
- Life Cycle(수명 주기)  
- onCreate  
    -> activity가 만들어질 때 단 한번만 호출 된다.  
    -> activity를 만들 때 단 한번만 하면 되는 작업들은 여기에서 해준다.  
```kotlin
private lateinit var binding: ActivityMainBinding
private lateinit var textView: TextView
// onCreate(): activity를 생성할 때 수행된다(필수로 있어야 함).
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    textView = binding.statusText
    textView.text = "activity created."
}
```

- setContentView 함수는 다른 lifeCycle 함수 안에서 사용할 수 없으므로 무조건 onCreate() 안에서 사용해야 한다.

onCreate() 함수 호출 후 activity는 "시작" 상태가 되는데, 이때 onStart()와 onResume()를 호출한다.

- onStart  
	- onStart() 함수는 activity가 "시작" 상태일 때 호출된다.
	- 이때 "시작" 상태는 activity가 사용자에게 보여지는 시점을 뜻한다.

```kotlin
override fun onStart() {
    super.onStart()

    Log.d("TAG", "onStart: activity started.")
}
```
onStart()가 수행되고 난 후, activity는 "Resume" 상태로 진입하면서 onResume()가 수행된다


- onResume  
    -> 다시 앱으로 돌아올 때 무조건 호출된다.  
	 onResume() 함수는 activity가 "Resume" 상태일 때 호출된다.
	"Resume" 상태는 사용자가 앱과 상호작용 할 수 있는 상태를 뜻한다.
```kotlin
override fun onResume() {
    super.onResume()

    Log.d("TAG", "onStart: activity resumed.")
}
```
intent를 통해 다른 액티비티로 넘어가는 경우에는 액티비티가 "Paused" 상태에 들어가 onPause()를 호출한다.

onCreate() 에서 onResume()까지의 앱의 모습

![[Kotlin개념4.gif]]

- onPause  
    -> 화면의 일부가 가려졌을 때  
    onPause는 activity가 "Paused" 상태일 때 호출된다.
   "Paused" 상태는 acivity가 일시 정지 상태라는 것으로 이해하면 될 것 같다.
```kotlin
override fun onPause() {
    super.onPause()

    Log.d("TAG", "onPause activity pause")
}
```
- onStop  
    -> 화면 전부가 보이지 않을 때  
    activity가 화면에서 완전히 안보이게되었을 때 호출된다.
```kotlin
override fun onStop() {
    super.onStop()

    Log.d("TAG", "onPause activity stopped")
}
```
사용자가 다시 돌아오게 되면, "Stopped" 상태에서 다시 시작해서 onRestart(), onStart(), onResume() 순서대로 다시 호출된다.
- onDestroy
activity가 완전히 죽기(?) 전에 수행되는 메서드이다.
```kotlin
override fun onDestroy() {
    super.onDestroy()

    Log.d("TAG", "onPause activity died.")
}
```

onPause()부터 onDestroy()까지의 모습

![[Kotlin개념5.gif]]


- intent
	- intent란 messaging object(메세지 객체) 이다. 이 객체를 통해 다른 컴포넌트 간에 정보를 주고 받을 수 있다.
	-  '컴포넌트를 실행하려고 시스템에 전달하는 메시지'
	- MainActivity 실행 후 DetailActivity로 화면 전환을 하려고 할 때
	    - 보통의 경우에는 DetailActivity 클래스의 객체를 생성해서 실행하면 되지만,
	    - 만약 DetailActivity가 컴포넌트 클래스라면 개발자가 코드에서 직접 생성해서 실행할 수 없음 → 시스템에 인텐트를 전달해 주어야 함  
        
	- 인텐트의 중재 역할은 같은 앱의 컴포넌트뿐 아니라 외부 앱의 컴포넌트와 연동할 때에도 마찬가지
    - A앱의 컴포넌트에서 인텐트를 시스템에 전달하고 그 정보를 분석하여 B앱의 컴포넌트 실행 가능
- 컴포넌트는 매니페스트 파일에 등록하여 사용


```xml
// Activity 등록
    <activity android:name=".DetailActivity"></activity>
    <activity android:name=".MainActivity">. .. 생략 ... </activity>
```

```kotlin
    val intent: Intent = Intent(this, DetailActivity::class.java)
    startActivity(intent)
```
```kotlin
btnStart.setOnClickListener { val intent = Intent(this, QuizQuestionsActivity::class.java) startActivity(intent) }
```

this 가 현재 액티비티이며, QuizQuestionsActivity::class.java가 이동할 액티비티이다.
startActivity(intent) 를 해주므로 화면 전환 코드는 끝이 난다. 꼭 startActivity를 해줘야한다.\

![[kotlin개념2.gif]]


### **인텐트 엑스트라 데이터**

- 컴포넌트 객체는 시스템이 생성하므로 개발자 코드로는 직접 접근 불가 → 컴포넌트 객체로 어떤 함수를 호출할 수는 없음  
    
- 따라서 인텐트에 컴포넌트 실행을 요청할 때 데이터를 함께 전달하려면 **엑스트라 데이터**를 이용
- **putExtra()** 함수 이용해 인텐트에 엑스트라 데이터를 추가  
    

```kotlin
    val intent = Intent(this, DetailActivity::class.java)
    intent.putExtra("data1", "hello")
    intent.putExtra("data2",10)
    startActivity(intent)
```

- 실행된 컴포넌트에서 엑스트라 데이터를 가져오려면
    - **getIntent()** 함수로 자신을 실행한 인텐트 객체를 얻어온 후
    - 그 인텐트 객체를 **get IntExtra()** 형태의 함수로 데이터를 가져오면 됨(함수는 타입별로 제공)

```kotlin
    val intent = Intent
    val data1 = intent.getStringExtra("data1")
    val data2 = intent.getIntExtra("data2", 0)
```

### **액티비티 화면 되돌리기**

- 액티비티는 화면을 구성하는 컴포넌트이므로, 다른 액티비티를 인텐트로 실행하면 화면이 전환됨
- 액티비티가 다른 액티비티를 실행해 화면이 전환되었을 때, 화면을 되돌리거나 되돌리지 않을 수 있음 → 이 옵션을 선택적으로 사용하기 위해 아래 함수 사용  
    

`startActivity()` : 화면을 되돌릴 필요 없을 때사용  
`registerForActivityResult()` : 결과를 포함해 화면을 되돌릴 때 사용 (실행 시 `launch()`사용)

```null
기존에 사용하던 startActivityForResult()는 deprecated 된 이유

- 프로젝트가 커지면그에 따라 onActivityResult 메서드 안에 들어가는 코드도 굉장히 많아지기 때문
    
    → 유지보수성이 떨어짐
    
    → requestCode를 지정하는 데에도 명확하지 않다는 단점이 있음
    
- 퍼미션 요청 시 불편하기 때문
    
   → requestCode에 따라 분기를 나누고 해당 권한을 가지고 있는지 없는지에 따라 또 분기를 나누게 되므로 퍼미션 요청을 여러 개 해야 하는 경우에는 코드가 복잡해짐
    

👉  이러한 문제를 해결하기 위해 **[ActivityResultContracts](https://developer.android.com/training/basics/intents/result#custom) 도입**
```

```kotlin
//결과를 포함해 화면을 되돌리는 경우
val intent = Intent(this, DetailActivity<::class.java)
	//startActivityForResult(intent, 10)      // deprecated
	registerActivity.launch(intent)

private val registerActivity: ActivityResultLauncher<Intent> = registerForActivityResult(
	ActivityResultContracts.StartActivityForResult() // ◀ StartActivityForResult 처리를 담당
	) { result ->
		if (result.resultCode == RESULT_OK) {
			val value = result.data?.getStringExtra("resultData")
			... 생략 ...
	}
}
```

```kotlin
//DetailActivity에서 화면 되돌리면서 데이터 전달
intent.putExtra("resultData","world")
setResult(RESULT_OK,intent)
finish()
```

- `setResult()` 함수로 결과를 지정할 때 **RESULT_OK**, **RESULT_CANCELED** 등의 상수 지정 가능
- 결과가 되돌아와서 다시 자신이 화면에 출력되면 `StartActivityForResult()`가 자동으로 호출됨
- 이 함수의 매개변수
    - `resultCode` : 인텐트로 실행된 곳에서 돌려받은 결과 코드
    - `data` : 결과 데이터가 들어있는 인텐트 객체

### **인텐트 필터**

- 아래처럼 인텐트에서 클래스 타입 레퍼런스 로 설정하는 경우, 같은 앱이면 가능하지만 외부 앱의 컴포넌트는 실행 불가

```kotlin
val intent: Intent = Intent(this, DetailActivity::class.java)
```

- 인텐트는 실행할 컴포넌트 정보를 어떻게 설정하는지에 따라 2가지로 나뉨
    1. **명시적 인텐트** : 클래스 타입 레퍼런스 정보를 활용한 인텐트(내부 앱의 컴포넌트 요청하는 객체 생성)
    2. **암시적 인텐트** : 인텐트 필터 정보를 활용한 인텐트(외부 앱 컴포넌트 요청 시 이용)

```kotlin
    //암시적 인텐트는 intent-filter 이용
    //name만 지정한 OneActivity는 명시적 인텐트로만 실행 가능
    <activity android:name=".OneActivity"/>
    <activity android:name=".TwoActivity">
    	<intent-filter>
    		<action android:name="ACTION_EDIT"/>
    	</intent-filter>
    </activity>
```

- 암시적 인텐트의 경우 클래스의 이름만 지정해줘도 등록된 정보와 비교해서 실행할 수 있지만, 어떤 컴포넌트를 외부에서도 인텐트로 실행할 수 있어야 한다면 해당 컴포넌트가 있는 앱의 매니페스트 파일에 암시적 인텐트로 실행할 수 있게 \<intent-filter>를 설정해줘야 함
- **\<intent-filter>**는 **\<activity>, \<service>,\<receiver>** 등 컴포넌트 등록 태그 하위에 작성 가능
- **\<intent-filter>**의 하위에는 다음 태그들을 이용해 정보 설정
    - **\<action>** : 컴포넌트의 기능을 나타내는 문자열 - 개발자 임의 지정
    - **\<category>** : 컴포넌트가 포함되는 범주를 나타내는 문자열 - 플랫폼 API에서 제공하는 문자열 이용
    - **\<data>** : ****컴포넌트에서 처리할 데이터의 성격을 나타내는 문자열 - URL형식으로 표현하며, 이나 처럼 문자열 하나로 선언하지 않고** android:scheme**,** android:host** 등의 속성을 필요한 만큼 선언하여 사용
- 암시적 인텐트 대표적 예 → 자동으로 만들어지는 메인 액티비티
    - 사용자가 아이콘 터치 시 외부 앱(런처)에서 실행하는 것이기 때문에 앱의 첫 화면인 MainActivity에 인텐트 필터를 설정해야 함

```kotlin
    <activity android:name=".MainActivity">
    	<intent-filter>
    		<action android:name="android.intent.action.MAIN" />
    		<category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
```

- 다른 컴포넌트도 외부 앱과 연동하려면 인텐트 필터 등록
    - TwoActivity를 다음처럼 등록하면 인텐트 필터에 선언된 정보에 맞춰 액티비티 시작해야 함

```kotlin
    <activity android:name=".TwoActivity">
    	<intent-filter>
    		<action android:name="ACTION_EDIT"/>
    		<category android:name="android.intent.category.DEFAULT"/>
    		<data android:scheme="http"/>
    		//<data android:mimeType="image/*"/>
    	</intent-filter>   
    </activity>
```

- 액티비티에 action과 data 프로퍼티에 실행 대상인 컴포넌트 정보 지정
    - 인텐트 정보에 **\<category>**를 설정하지 않으면 **DEFAULT**값으로 지정됨
    - 만약 매니페스트 파일에 **DEFAULT**가 아닌 다른 문자열을 지정했다면, 인텐트에도 해당 문자열을 그대로 지정해야 함

```kotlin
    //action, data 정보를 인텐트의 각 프로퍼티에 설정하는 방법
    val intent = Intent()
    intent.action = "ACTION_EDIT"
    intent.data = Uri.parse("http://www.google.com")
    //intent.type = "image/*"
    startActivity(intent)
    
    //생성자의 매개변수로 지정하는 방법
    val intent = Intent("ACTION_EDIT", Uri.parse("http://www.google.com"))
    startActivity(intent)
```

### **액티비티 인텐트 동작 방식**

- 명시적 인텐트는 클래스 타입 레퍼런스 정보를 이용하므로 액티비티가 없거나, 여러 개일 수 **없음**
- 암시적 인텐트는 실행할 액티비티를 \<action>, \<category> 등의 문자열 정보로 나타내므로 없거나 여러 개일 수 있음
    - 없을 때 : 인텐트를 시작한 곳에 오류가 발생
    - 1개일 때 : 문제없이 실행
    - n개일 때 : 사용자 선택으로 하나만 실행
- 실행할 액티비티 없는 경우, 오류가 발생하기 때문에 예외 처리를 해야함  
    

```kotlin
    val intent = Intent("ACTION_HELLO")
    try {
    	startActivity(intent)
    } catch (e: Exception){
    	Toast.makeText(this,"no app...",Toast.LENGTH_SHORT).show()
    }
```

- 사용자의 폰에 실행할 액티비티가 여러 개이면, 다이얼로그를 띄워서 선택한 액티비티를 실행  
    

```kotlin
    //지도 앱 액티비티 실행 예시
    val intent = Intent(Intent.ACTION_VIEW, Uri.parse("geo:37.7749,129.4194"))
    startActivity(intent)
```

→ 지도를 실행할 액티비티가 여러 개(카카오맵, 네이버 지도, T map 등)인 경우, 사용자에게 연결 프로그램 목록을 띄워 선택하게 함

- 이처럼 액티비티가 여러 개일 때 특정 앱의 액티비티를 실행하기 위해서는 해당 앱의 패키지명을 지정  
    

```kotlin
    val intent = Intent(Intent.ACTION_VIEW, Uri.parse("geo:37.7749,129.4194"))
    intent.setPackage("com.google.android.apps.maps")
    startActivity(intent)
```

### **패키지 공개 상태**

- 안드로이드 11(API 레벨 30) 버전부터는 앱의 패키지 공개 상태를 지정하지 않으면 외부 앱의 패키지 정보에 접근할 수 없음
- 인텐트로 외부 앱에 연동하는 부분에는 영향이 없고, 외부 앱을 연동하더라도 패키지 정보를 활용하지 않는다면 문제는 없음
- 하지만, 다음과 같은 함수 사용 시, 패키지 공개 상태에 따라 영향을 받음
    - PackageManager.getPackageInfo()
    - PackageManager.queryIntentActivities()
    - Intent.resolveActivity()
    - PackageManager.getInstalledPakages()
    - PackageManager.getInstalledApplications()
    - bindService()
- 외부 앱 정보 가져오기  
    

```kotlin
    val packageInfo = packageManager.getPackageInfo("com.example.text_outter",0)
    val versionName = packageInfo.versionName
```

- 안드로이드 11버전부터는 적용된 패키지 공개 상태 필터링 때문에 다음과 같은 오류 발생
    - Caused by: android.content.pm.PackageManager$NameNotFoundException: com.example.text_outter
- 매니페스트 파일에 접근하고자 하는 앱의 패키지명 선언하여 오류 해결 !

```kotlin
    <manifest ... 생략 ...>
    	<queries>
    		<package android:name="com.e.ch7_layout" />
    	</queries>
    <manifest>
```

- **\<uses-permission>** 태그로 허용해 달라고 요청할 수는 있지만, 되도록이면 **\<queries>**태그를 사용할 것

# 1. 레이아웃 (Layout)

레이아웃은 여러 요소들을 배치할 수 있는 캔버스라고 생각할 수 있을 것 같다. 레이아웃 위에 내가 넣고 싶은 요소들을 원하는 대로 배치하고 머릿속에 있는 어플리케이션의 화면을 레이아웃을 통해 구성해낼 수 있는 것이다.  
  

오늘 살펴볼 레이아웃은 3가지가 있다.

- 컨스트레인트 레이아웃(제약, ConstraintLayout) (기본 레이아웃)
- 리니어 레이아웃(선형, LinearLayout)
- 프레임 레이아웃(FrameLayout)

  

화면 구성은 **"activity_main.xml"** 이라는 이름의 레이아웃 파일에서 할 수 있고, 이 파일은 자동으로 생성된다.

  

##### * 앱은 이미지, MP3, DB와 같은 많은 종류의 파일로 구성되는데, 코틀린으로 작성되는 소스 코드 파일을 제외한 모든 파일을 리소스(Resource)라고 한다. xml파일도 리소스에 해당하며 리소스 파일의 이름은 모두 소문자로 작성한다.

  
  

# 2. 컨스트레인트 레이아웃 (ConstraintLayout)

컨스트레인트 레이아웃은 안드로이드 기본(Default) 레이아웃으로 화면에 배치되는 위젯들 사이에 간단한 제약조건 설정만으로 전체 화면을 쉽게 구성할 수 있다.  
  

### 2.1. 핸들러 (Handler)

![[Pasted image 20240816112247.png]]


레이아웃에 객체를 클릭하면 위 사진과 같이 상하좌우로 4개의 동그라미가 생기는데 이것이 핸들러(Handler)이다.



![[Pasted image 20240816112257.png]]


- 핸들러의 상태  
    . 연결 : 파란색  
    . 연결 안 됨 : 흰색
    
- 컨스트레인트 : 주름무늬선
    
- 앵커 포인트 : 컨스트레인트가 연결될 수 있는 부위  
      
    

### 2.2. 컨스트레인트 편집기

- #### 연결
    
    파란색 "+" 버튼은 연결이 해제되어 있는 상태로 "+"를 클릭하면 가장 가까이에 있는 다른 위젯 또는 레이아웃의 앵커 포인트에 컨스트레인트가 생성된다. 연결된 앵커 포인트와의 마진(거리값)은 자동으로 설정된다.
    
      
    
- #### 크기 조절 핸들러
    
    주로 상하 또는 좌우 양쪽에 컨스트레인트가 연결되었을 때 사용한다.  
    핸들러 가운데에 보이는 사각 박스 안의 ">> <<"를 클릭하면 세 가지 모드로 변경이 가능하다.  
      
    **-픽스드 (fixed) : 입력된 크기로 고정  
    -매치 컨스트레인트 (match constraint) : 앵커 포인트에 맞춰서 크기와 가로세로비 조절 가능**
    

  

- #### 바이어스
    
    상하 또는 좌우 양쪽이 같이 연결되어 있을 때 위치 조절 버튼 바이어스(Bias)가 활성화된다.  
      
    **- 0~100 사이의 값으로 변경 가능  
    -중앙 : 50**  
    

  

- #### 체이닝(Chaining)
    
    컨스트레인트로 연결된 위젯끼리 서로의 위칫값을 공유해서 상대적인 값으로 크기와 위치를 결정한다. 화면을 가로세로로 전환했을 때 위젯의 상대 비율을 유지해준다.

  

- #### 가이드라인(GuideLine)
    
    컨스트레인트 레이아웃에만 사용할 수 있는 보조 도구이다. 가이드라인을 드래그해서 임의의 위치에 가져다 놓으면 레이아웃 안에 배치되는 위젯에 가상의 앵커 포인트를 제공한다.  
      
      
      
    

# 3. 리니어 레이아웃 (LinearLayout)

위젯을 가로(horizontal)/세로(vertical) 한 줄로 배치하기 위한 레이아웃이다.

### 3.1. 레이아웃 설정

XML 코드를 직접 수정하거나 [Design] 버튼을 클릭해서 컴포넌트 트리의 최상위 레이아웃을 리니어 레이아웃으로 설정하면 리니어 레이아웃을 기본 레이아웃으로 설정할 수 있다.


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

</LinearLayout>
```

  

### 3.2. orientation 속성

하위 버전에서는 필수 속성으로 horizontal/vertical을 설정해줘야 했지만, 3.1부터는 입력하지 않으면 가로(horizontal)이 적용된다.  
  

### 3.3. layout_weight 속성

레이아웃 안에 배치되는 위젯의 크기를 비율로 나타낼 수 있다.  
**-기본 설정값 : 1  
-정확한 비율 설정 : layout_weight 속성을 정확하게 주기 위해서는 layout_width 또는 layout_height 속성값을 '0dp'로 설정해줘야 한다.**  
  

### 3.4. gravity 속성

위젯에 원하는 정렬을 설정할 수 있는 속성이다. 동시에 2개 이상의 방향을 선택할 수 있다.  
  

### 3.5. layout_gravity 속성

부모 레이아웃을 기준으로 위젯의 위치가 정렬된다.  
**-layout_gravity : right**

![[Pasted image 20240816112352.png]]

 부모 레이아웃이 horizontal일 경우에는 layout_gravity : right가 적용되지 않는 것 같음..  
  
**cf) gravity : right**

![[Pasted image 20240816112401.png]]


### 3.6 스크롤뷰

스크롤뷰를 추가해주면 위젯이 화면을 넘어갈 때 스크롤이 가능하다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

</ScrollView>
```

  

- #### Space 도구
    
    빈 여백을 만들 수 있는 레이아웃 보조 도구이다. 위젯 사이에 일정한 간격을 두고 싶을 때 사용한다.  
      
      
      
    

# 4. 프레임 레이아웃 (FrameLayout)

위젯을 중첩해서 사용하기 위한 레이아웃이다. 주로 게임 화면처럼 배경과 플레이어가 서로 다른 레이어에서 겹쳐 움직일 때 사용할 수 있다.

레이아웃 중에서 처리 속도가 가장 빨라서 1개의 이미지만 화면에 보여준다든지 하는 단순한 형태로 사용할 경우에 성능이 가장 좋다.



---
출처 - https://comain.tistory.com/339

https://velog.io/@ywown/kotlin-%EC%9D%B8%ED%85%90%ED%8A%B8-%EB%B3%B5%EC%8A%B5


https://rkdrkd-history.tistory.com/47