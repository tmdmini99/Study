### **뷰(View)**



![[Kotlin View1.png]]


- 안드로이드 앱의 **UI를 구성하는 기본 단위 = 위젯 + 레이아웃**
- 구성  
    **1. 위젯(Widget)**  
        - View의 서브 클래스로서, 앱 화면을 구성하는 시각적인 모양을 지닌 UI요소  
        - 예) 버튼, 메뉴, 리스트, 이미지뷰 등  
    **2. 레이아웃(Layout)**  
        - ViewGroup의 서브 클래스로서, 다른 뷰(위젯 혹은 레이아웃)를 포함하면서  
        이들을 정렬하는 기능을 지닌 UI요소

### **UI 설계 방법**

#### **01. Layout Editor의 XML을 사용하여 UI설계**

- AndroidStudio의 Layout Editor이용
- 드래그 앤 드롭 방식의 WYSIWYG(what you see is what you get) 에디터
- 다양한 디바이스 / 안드로이드 버전에 대한 Preview
- XML코드 자동 변환 및 동기화

**💡 Layout Editor 구성**

![[Kotlin View2.png]]


- **1) Palette**  
    - 레이아웃으로 드래그 할 수 있는 다양한 뷰 및 뷰 그룹을 포함
- **2) Component Tree**  
    - 레이아웃에서 구성요소의 계층 구조를 표시
- **3) 툴바**  
    - 편집기에서 레이아웃 모양을 구성하고 레이아웃 속성을 변경 (언어, 크기, API, 블루프린트 등)
- **4) 디자인 편집기**  
    - 디자인 뷰나 청사진(blueprint)뷰 또는 두 뷰 모두에서 레이아웃을 수정
- **5) Attributes**  
    - 선택한 뷰의 속성을 제어할 수 있는 영역
- **6) 뷰 모드**  
    - 레이아웃을 코드, 디자인, 분할(split) 모드 중 하나로 표시  
    - 분할 모드는 코드 창과 디자인 창을 동시에 표시
- **7) 확대/축소 및 화면 이동 컨트롤**  
    - 편집기 내에서 미리보기의 크기와 위치를 제어

#### **02. XML file을 직접 편집**

- 필요한 XML 태그나 속성을 잘 모를 경우 불편
- Copy & paste를 이용한 편집이 효율적인 경우가 많음




## **2. 위젯(Widget)**

### **위젯(Widget)**



![[Kotlin View3.png]]




- View의 서브 클래스 중 위젯
- 대표적인 위젯은 TextView, EditText, Button 등

### **뷰(View)**

![[Kotlin View4.png]]


- View클래스는 **모든 UI 컴포넌트들의 부모** 클래스
- View클래스의 속성은 모든 UI컴포넌트들에서 공통적으로 사용 할 수 있음

#### **layout_width, layout_height**

![[Kotlin View5.png]]

- **match_parent**  
      - **부모** UI컴포넌트의 크기에 맞춤   
      - 원래는 fill_parent 였으나 SDK2.2(프로요)부터는 match_parent로 변경. 둘 다 사용 가능
- **wrap_content**  
      - UI 컴포넌트의 내용물 크기에 맞춤


#### **다양한 스마트폰 해상도 대응**

- **1) px (pixels)**  
    - 스크린의 실제 단위  
    - 화면의 전체 화면 크기와 상관 없이 지정한 수치만큼 표시되는 **절대적 표시 단위  
    - 따라서 디스플레이의 해상도에 따라 달라보일 수 있음 (저해상도에서 더 크게 보임)**
- **2) dp (Density-independentPixel)**   
    - 밀도에 독립적인 단위  
    - **디스플레이의 해상도(밀도)와 상관 없이** 다룰 수 있는 단위  
      텍스트의 경우 해상도가 달라지면 해당 해상도에 맞게 크기가 곱해짐 (x2, x3...)  
      이미지의 경우 폴더를 따로 해상도에 맞게 만들어서 대응  
    - 160dpi(320x480) 장비에서의 100dp(100px)와 320dpi(720x1280)의 100dp(200px)는 같음
- **3) dpi (Dots per Inch)**   
    - 1인치에 들어가는 픽셀을 나타내는 단위  
    - 100DPI는 1인치 당 픽셀이 100개 포함 된다는 것으로, 개수가 많을수록 고밀도  
    - 안드로이드의 기준 DPI는 160DPI (160DPI인 경우 1dp = 1px)
- 4) in (inches)
- 5) mm (millimeters)
- 6) sp (텍스트 지정 단위))

![[Kotlin View6.png]]


![[Kotlin View7.jpg]]

![[Kotlin View8.png]]



- 여러 개의 크기별 파일을 만들어 다양한 스마트폰의 해상도에 대처한다.

#### **TextView (텍스트 표시)**

- View 속성 상속: id, layout_width, layout_height, background, etc
- text:"" 출력할 문자열 지정
- textSize: 폰트 크기
- textStyle: 텍스트 스타일 (normal, bold, italic)
- typeface: 텍스트 폰트 (normal, sans, serif, monspace)
- textColor: 문자열 색상
- **singleLine:** 속성값이 'true'면, 텍스트가 위젯의 폭보다 길어도 **강제로 한 줄에 출력**

💡 **background**

- 뷰의 배경을 지정하며, 색상 및 이미지 등의 여러가지 객체로 지정 가능
- #RGB, #ARGB, #RRGGBB, #AARRGGBB 네 가지 형식 적용 가능
- 참고로 색상은 RGB와 같은 컬러코드나, res/values/colors.xml 등록, android.graphics.Color 클래스로 설정 가능

#### **EditText (텍스트 입력)**

- TextView의 모든 속성 상속 (EditText는 TextView의 서브클래스)
- **inputType:** 입력 시 허용되는 키보드 타입 설정 및 키보드 행위를 설정
- 키보드 타입 설정  
        - “**text**”: 일반적인 텍스트 키보드   
        - “**phone**”: 전화번호 입력 키보드   
        - “**textEmailAddress**”: @문자를 가진 텍스트 키보드
- 키보드 행위 설정  
        - “**textCapWords**” : 문장의 시작을 대문자로 변환   
        - “**textAutoCorrect**” : 입력된 단어와 유사한 단어를 제시하고 제시된 단어 선택 시, 입력된 단어를 대치   
        - “**textMultiLine**”:여러 줄을 입력 받을 수 있음

#### **Button (버튼)**

- 일반적으로 많이 사용되는 푸시 버튼으로 사용자가 버튼을 클릭하였을 때, 어떤 행동을 수행 하고자 할 때 사용
- Button클래스는 TextView의 서브클래스이므로, TextView의 모든 속성을 사용 할 수 있음  
    - singleLine 사용 가능  
    - 버튼 내에 텍스트, 아이콘을 표시할 수 있음  
    - 버튼 전체를 이미지로 그리기 위해서는 ImageButton 사용

#### **💡 버튼 클릭 이벤트 처리**

- 사용자가 버튼 위젯을 클릭 할 때, 지정된 행동을 수행하기 위해 둘 중 하나 설정
- **(1) 버튼 위젯의 onClick속성 활용**  
      - 버튼 위젯을 정의한 화면을 contentView로 설정한 액티비티 클래스에 새로운 메소드를 추가

```
MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?){
    	super.onCreate(savedInstanceState)
        setContentView(R.layout.text_views) //이 부분이 변경
    }
    
    fun doAction(v:View) {
    Toast.makeText(getApplicationContext(), "Submitted Successfully",
    	Toast.LENGTH_SHORT).show(); // 버튼을 눌렀을 때 작동할 toast 메시지 출력 함수 생성
    }
}


text_views.xml

<Button
	android:onClick="doAction"/> // 버튼을 눌렀을 때 doAction을 실행할 onClick 설정
```

- **(2) 이벤트 처리 객체를 이용**  
    - 이벤트를 처리하는 객체를 생성하여 해당 이벤트를 발생시키는 위젯에 등록  
    - 위젯에서 이벤트가 발생하면 등록된 이벤트 처리 객체가 정의된 일을 수행

```
MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?){
    	super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    
    val btn = findViewById<Button>(R.id.submit_button) // findViewbyID()로 객체를 가져옴
    
    btn.setOnClickListener{ // 버튼을 눌렀을 때 작동할 toast 메시지 출력 SetonClickListener
    Toast.makeText(this, "Submitted Successfully",Toast.LENGTH_SHORT).show();	
    	}
    }
}


text_views.xml

<Button
	android:onClick="doAction"/> // 버튼을 눌렀을 때 doAction을 실행할 onClick 설정
```

**💡findViewByID()**

- Activity 클래스에 정의된 메소드로, Activity 하위 클래스에서 사용 가능
- 해당 액티비티와 연결된 XML layout 리소스 요소(위젯) 중에서 id속성을 바탕으로 해당 Java 객체를 가져옴
- onCreate() 메소드 내의 setContentView()를 통해서 연결된 XML 리소스 요소 중에서만 검색이 가능함
- 따라서 다른 액티비티와 연결된 XML layout 리소스에 정의된 위젯을 findViewByID() 메소드로 가져올 수는 없음


## **3.  실무에서의 버튼 디자인**

#### **이미지 버튼**

![[Kotlin View9.png]]

![[Kotlin View10.png]]



- 대부분의 버튼은 ImageView를 사용
- ImageView 속성에 Clickable 옵션을 true로 설정
- 이미지는 normal, pressed, disable 3가지로 준비
- 해상도 처리를 위해 dpi별 5개로 이미지 준비 (hdpi, mdpi, xhdpi, xxhdpi, xxxhdpi)
- 1개의 버튼에 총 15개의 이미지 필요

#### **9-patch (나인패치 이미지)**


![[Kotlin View11.png]]


- ImageView에서의 배경 이미지는 가로/세로모드, 해상도에 따라 각각의 이미지가 필요함  
    (예) 가로모드 변경 시 배경 이미지가 깨질 수 있음
- 기기별 해상도 및 화면 모드에 따라 각각의 이미지 파일을 모두 만든다면 좋지만, 관리가 쉽지 않음
- 대신, 이미지의 특정 영역(주로 가장자리)는 원본 이미지 그대로 표시되도록 만들고, 그 외 이미지의 내용이 표시되는 영역을 버튼의 크기에 따라 늘어나도록 만들면 깔끔한 형태의 이미지 버튼을 표시할 수 있음
- 나인패치(9-patch) 이미지: **이미지에서 늘어날 수 있는(stretchable) 영역과 원본 크기대로 표시되어야 할 영역을 구분**하여, **이미지가 그려질 영역의 크기가 늘어나거나 줄어들더라도 원본이미지 형태를 유지하도록** 만들어진 이미지

#### **ImageView**

- 앱 화면에 이미지를 표시하는 용도로 사용
- **사용법**  
    - Layout 리소스 XML파일에 ImageView를 추가  
    - 화면에 표시 할 이미지 파일을 Drawable 리소스 폴더(/res/drawable)에 추가  
    - 화면에 표시 할 이미지(Drawable) 리소스 ID를 ImageView의 "src" 속성에 지정  
    (예) android:src="@drawable/android_iconpic"
- **참고사항**  
    - 이미지 파일의 형식은 .jpg,.png가 가능하나 대부분.png를 사용함 (투명도 때문)  
    - 해상도에 따른 다른 크기의 이미지는 별도의 폴더를 생성하고 복사 (drawable-xhdpi 등)  
    - 파일명 한글, 영어 대문자 불가, 숫자로 파일명 시작 불가
- **xml이 아닌 Kotlin 소스에서 직접 이미지뷰의 이미지 변경**  
    var imageView: ImageView = findViewById(R.id.imageView1)   
    imageView.**setImageResource**(R.drawable.android_iconpic)

**💡 ImageView의 영역에 맞게 이미지 크기 조절**

![[Kotlin View12.png]]


- scaleType 속성 >  **android:scaleType** 이용
- 1) android:scaleType = **"Center"**  
    - 이미지의 본래 크기와 비율을 유지하며 이미지의 중앙을 ImageView의 중심에 맞춤   
    - ImageView보다 이미지가 클 경우 이미지가 잘릴 수 있음
- 2) android:scaleType = **"centerCrop"**  
    - 이미지의 비율을 유지하며 가로,세로 중 짧은 쪽을 ImageView에 꽉 차게 출력  
    - ImageView를 벗어나는 부분은 잘릴 수 있음
- 3) android:scaleType = **"centerInside"**       
    - 이미지의 가로, 세로 중 긴 쪽을 ImageView의 레이아웃에 맞춰 출력  
    - 이미지의 비율은 유지되며 남는 공간은 background의 색으로 채워짐  
    - fitCenter와 달리 이미지가 ImageView보다 작을 경우 크기가 유지
- 4) android:scaleType = **"fitCenter"**  
    - 이미지의 가로, 세로 중 긴 쪽을 ImageView의 레이아웃에 맞춰 출력 (centerInside와 매우 유사)  
    - 이미지의 크기가 ImageView보다 작다면 ImageView의 크기에 따라 달라짐
- 5) android:scaleType = **"fitStart"**  
    - 가로, 세로 비율을 유지하며 fit하게 출력  
    - fitCenter와 다르게 좌측 상단을 기준으로 정렬
- 6) android:scaleType = **"fitEnd"**  
    - 가로, 세로 비율을 유지하며 fit하게 출력  
    - fitCenter와 다르게 우측 상단을 기준으로 정렬
- 7) android:scaleType = **"fitXY"**  
    - 가로, 세로 비율에 상관 없이 ImageView에 꽉 차게 출력  
    - 이미지가 찌그러진 상태로 보일 수 있음
- 8) android:scaleType = **"matrix"**  
    - 이미지의 크기, 비율을 유지하며 ImageView의 좌측 상단을 기준으로 정렬  
    - 이미지가 크다면 ImageView 외의 부분은 잘림



---
출처 - https://limheejin.tistory.com/149


