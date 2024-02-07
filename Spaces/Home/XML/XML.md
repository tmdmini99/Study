### XML 개요

---

#### XML

XML은 데이터를 저장하고 전달할 목적으로 만들어졌으며, 저장되는 데이터의 구조를 기술하기 위한 언어입니다.

XML은 eXtensible Markup Language의 약자로, 수많은 응용 분야에서 데이터를 저장하고 전달하는 중요한 역할을 맡고 있습니다.

##### 예제

```xml
<?xml version="1.0" encoding="UTF-8"?>

<programming_languages>

    <language>

        <name>HTML</name>

        <category>web</category>

        <developer>W3C</developer>

        <version status="working draft">5.1</version>

        <priority rating="1">high</priority>

    </language>

    <language>

        <name>CSS</name>

        <category>web</category>

        <developer>W3C</developer>

        <version status="stable">3.0</version>

        <priority rating="3">middle</priority>

    </language>

    <language>

        <name korean="자바">Java</name>

        <category>application</category>

        <developer>Oracle</developer>

        <version status="stable">8.91</version>

        <priority rating="2">high</priority>

    </language>

    <language>

        <name korean="파이썬">Python</name>

        <category>application</category>

        <developer>Python</developer>

        <version status="stable">3.52</version>

        <priority rating="4">middle</priority>

    </language>

</programming_languages>

```


### XML 기초

---

#### XML를 배우기 위한 사전지식

XML은 수많은 응용 분야에서 데이터를 저장하고 전달하는 중요한 역할을 맡고 있습니다.

XML를 배우기 전에 여러분은 다음과 같은 기초 지식이 필요합니다.

- HTML

- 자바스크립트


---

#### XML이란?

XML은 EXtensible Markup Language의 약자이며, 1998년에 W3C 표준 권고안에 포함되었습니다.

XML은 HTML과 매우 비슷한 문자 기반의 마크업 언어(text-based markup language)입니다.

이 언어는 사람과 기계가 동시에 읽기 편한 구조로 되어 있습니다.

그러나 XML은 HTML처럼 데이터를 보여주는 목적이 아닌, 데이터를 저장하고 전달할 목적으로만 만들어졌습니다.

또한, XML 태그는 HTML 태그처럼 미리 정의되어 있지 않고, 사용자가 직접 정의할 수 있습니다.

---

#### XML의 특징

XML의 중요한 특징은 다음과 같습니다.

1. XML은 다른 목적의 마크업 언어를 만드는 데 사용되는 다목적 마크업 언어입니다.

2. XML은 다른 시스템끼리 다양한 종류의 데이터를 손쉽게 교환할 수 있도록 해줍니다.

3. XML은 새로운 태그를 만들어 추가해도 계속해서 동작하므로, 확장성이 좋습니다.

4. XML은 데이터를 보여주지 않고, 데이터를 전달하고 저장하는 것만을 목적으로 합니다.

5. XML은 텍스트 데이터 형식의 언어로 모든 XML 문서는 유니코드 문자로만 이루어집니다.

XML은 문서용 마크업 언어를 정의하기 위한 메타언어인 SGML(Standard Generalized Markup Language)을 기반으로 만들어졌습니다.

---

#### XML 기반의 언어

XML을 기반으로 하는 대표적인 언어는 다음과 같습니다.

1. XHTML

2. SVG

3. RDF

4. RSS

5. Atom

6. MathML

---

#### XML 표준

XML 표준화 작업은 1996년 W3C에서 지원하는 XML 워킹 그룹에 의해 시작됩니다.

그 후 1997년에 XML 1.0 초안이 완성되고, 1998년에 마침내 XML 1.0 표준 권고안이 공표됩니다.

XML 1.0의 오류를 수정하고 발전을 거듭하여 2008년에는 XML 1.0의 다섯 번째 버전이 발표되기에 이릅니다.

XML의 최신 표준에 대한 더 자세한 사항은 다음 링크를 참고하면 됩니다.

[https://www.w3.org/TR/xml =>](https://www.w3.org/TR/xml/)


# 레이아웃 xml 파일에서 xmlns 코드의 의미는? ( xml에서의 import 기능 )

아래와 같이 레이아웃을 나타내는 xml 파일이 있다고 할때,

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/senderTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:text="NICKNAME" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

이 부분은 무엇을 나타낼까?

```xml
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
                         ......
>
                         ......
```

이 본론부터 말하자면 이 부분은 xml에서의 import 기능이라고 보면 된다.

즉, 해당 위치에 있는 라이브러리를 각각 임의의 변수에 담아서 사용하겠다는 의미이다.

- 예를 들어,
    
    ```xml
    <androidx.constraintlayout.widget.ConstraintLayout 
       xmlns:android="http://schemas.android.com/apk/res/android"
                     ......
               >
     
       <TextView
           android:id="@+id/senderTextView"
    />
    ```
    
    - 우리가 위와 같이 TextView의 id 속성을 지정할 수 있는 것도, 뜯어보면  
        "android"라는 변수에 "[http://schemas.android.com/apk/res/android](http://schemas.android.com/apk/res/android)" 위치에 있는  
        android 라이브러리를 지정하였고,  
        id 속성을 가져와서 지정해주었기 때문이다.
        
    - 즉, 여기서 "android"라는 것은 라이브러리에 임시로 붙여준 변수명일 뿐 아무 의미 없다.
        
- 단적으로 말해서 아래와 같이 해도 코드에는 아무 문제가 없다.
    
    ```xml
    <androidx.constraintlayout.widget.ConstraintLayout 
       xmlns:ThisIsAndroHaHa="http://schemas.android.com/apk/res/android"
                     ......
               >
     
       <TextView
           ThisIsAndroHaHa:id="@+id/senderTextView"
    />
    ```
    
    - 하지만 개발자간의 정해진 약속이 있기 때문에 위와 같이  
        변수명을 변경하는 일은 사실상 없다고 보는 것이 좋다.

### 결론

1. 우리가 레이아웃을 나타내는 xml파일에서 흔히 사용하는  
    android, app, tools 등은  
    문법이 아니라 변수명일 뿐이다.
    
    --> xml에서 tools를 써도 자동 import가 안되는 이유임
    
2. xmlns는 import의 성격을 띄고 있으므로,  
    xmlns에도 주목하여 각각의 라이브러리들의 성격을 인식하면  
    좀 더 쉽게 코드를 파악할 수 있을 것이다.




---
출처 - https://tcpschool.com/xml/intro