# HTML 이란 무엇인가?

HTML 은 Hyper Text Markup Language 약어로 ==HyperText(웹 페이지에서 다른 페이지로 이동할 수 있도록 하는 것)== 기능을 가진 문서를 만드는 언어입니다. 다시 말해,  구조를 설계할 때 사용되는 언어로  hyper link 시스템을 가지고 있으며, 흔히 말하는 웹 페이지를 위한 마크업 언어라고 할 수 있습니다.


웹 페이지(web page)는 월드 와이드 웹 상에 있는 개개의 문서를 가리킵니다.   
        .gif, jpg, .Ai, .pdf, .doc, .hwp 이와 같은 확장자 포맷이 있듯이 HTML 은 `.htm`, `.html` 확장자 포맷을 가지고 있습니다.   
        이 html 문서는 단순히 텍스트 파일에 불과하고 웹 브라우저가 해석을 해서 구조를 통해 화면에 렌더링 해주게 되고 사용자는 View 라고 하는 스크린을 통해 접하게 되는 것입니다.


시멘틱 마크업(Semantic Markup) 은 웹 사이트(페이지)의 콘텐츠를 설명하는데 사용되는 마크업 언어이고,   
        HTML 은 콘텐츠의 의미를 설명하는데 유일한 목적을 가집니다.   
        앞으로 공부할 CSS 가 비주얼 디자인(Visual Design) 이라면 HTML 은 구조적 설계(Structure Design) 이라 할 수 있습니다.


## 시멘틱 마크업(Semantic Markup)

 Semantic Markup은 종종 POSH(Plain Old Semantic HTML)라고도 불리우는데 말 그대로 평범하고오래된 의미론적인   HTML이라는 뜻입니다.    
        HTML 은 웹사이트 콘텐츠를 설명하는데 사용되는 마크업 언어이므로 콘텐츠의 의미를 설명하는데 유일한 목적을 가지고 있습니다.   
        앞으로 공부할 CSS 가 비주얼 디자인(Visual Design) 이라면 ==HTML 은 구조적 설계(Structure Design)== 이라 할 수 있습니다.


## HTML 문서 작성을 위한 기본 문법

HTML 문서인 웹 페이지는 ==head 영역과 body 영역==으로 구성됩니다.  문서의 타이틀(title) 은 웹 페이지의 제목으로 브라우저 탭에 표시됩니다.

### HTML 용어  

 - 엘리먼트(element) - 요소
 콘텐츠(요소포함)를 감싸는 태그(tag)
 - open tag - 여는 태그
- close tag - 닫는 태그    
![](https://t1.daumcdn.net/cfile/tistory/99C713425C1B1CC714)
- 여는 태그와 닫는 태그가 있는 이유는 콘텐츠를 감싸기 위함입니다.(또는 요소가 다른 요소를 감싸기 위해서)
- 닫는 태그(close tag)가 없는 HTML 요소 -  콘텐츠(contents)를 감싸지 않아 비어있다는 의미   
ex) `<meta charset="utf-8">`   
헤더는 브라우저가 인식할 수 있는 정보를 제공해 주는 영역으로 메타 태그의 문자셋은 utf-8 로 되어있음을 나타내는 정보만 제공하고 있기 때문에 내용(콘텐츠)을 감싸지 않았다라고 볼 수 있습니다.

- 애트리뷰트(attribute) - 속성
- 벨류(value) - 값  
`<tagname attribute="value"> 콘텐츠 </tagname>`  
![](https://t1.daumcdn.net/cfile/tistory/99A1A1495C1B1D2F18)

## HTML 문서 작성을 위한 DTD



HTML 을 작성하려면 문서타입이라는 것이 반드시 필요합니다.


문서 타입을 DTD 라고 하며,
==DTD(DOCTYPE or Document type Definition)는 DTD는 HTML 문서의 반드시 최상 위에 위치==해야 합니다.

이러한 문서형 정의로 HTML5, HTML4, XHTML 세가지 문서 유형이 존재하며 기술한 유형에 따라 마크업 문서의 요소와 속성(attribute) 등을 처리하는 기준이 되고 이것은 또한 유효성 검사에 이용된다.

문서형(DTD) 정의를 생략하는 경우 웹 브라우저가 표준모드가 아니라 비표준모드(Quirks mode)로 렌더링되어 크로스 브라우징에 어려움을 겪을 수 있습니다.


DOCTYPE 의 버전별선언(HTML5, HTML4, XHTML)에 따라서 HTML 은 지원하는 태그가 조금씩 다릅니다.


그리고 DOCTYPE 태그가 아니라 선언문으로서의 역할이기 때문에 HTML문서 최상위에 위치하는 것입니다.


또한 DOCTYPE 종료하는 태그가 없는 것이 특징이며 `<!DOCTYPE>`은 HTML 문서의 구성 요소는 아닙니다.


아래 문서가 기본 HTML5 문서타입입니다.


```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta http-equiv="Content-Script-Type" content="text/javascript">
    <meta http-equiv="Content-Style-Type" content="text/css">
    <title>HTML 4.01 문서타입</title>
    <link rel="stylesheet" type="text/css" href="css/service_name.css">
</head>
<body>

</body>
</html>
```

위와 같이 최상단에 문서 타입 지정을 지정하고, 여기서 PUBLIC 은 DTD가 공공(모든 사용자)에게 공개되어 있으며 문서타입에 따른 W3C 표준안의 URI 을 작성하게 됩니다.
그리고 URI 를 생략할 경우에도 페이지는 쿼크 모드로 렌더링됩니다.

HTML이 어떤 버전으로 작성되었는지 미리 선언하여, 웹 브라우저가 내용을 올바르게 표시할 수 있도록 해주는 것이 독타입입니다.

그리고 바로 다음에 html 태그가 오고 html 태그 안에는 `head` 와 `body` 로 크게 나뉘어져 있습니다

태그는 일반적으로 시작태그와 종료(끝)태그로 이루어져 있으며 예외적인 태그들도 있습니다.

또한 `html` 태그의 속성으로 문서에서 다룰 언어를 지정해야합니다.

언어 설정은 필수 속성으로 이 속성이 생략되어 있으면 안됩니다.

`head` 안에는 콘텐츠를 표현하는 내용은 없지만 콘텐츠를 표현하기 위한 내용들을 포함하게 됩니다.


위의 `meta`(메타 태그)는(문서자체를 설명하는정보)는 문서의 정보(웹페이지의 요약)를 브라우저와 검색엔진에게 이 문서가 어떤 정보를 가지고 있는지 알려주는 것을 명시합니다.
==즉, 문서 자체를 설명하는 정보를 담고 있는 것으로 그 문서의 핵심키워드, 누가 만들었는지, 문자셋(언어설정) 등은 어떤 것을 사용하고 있는지 등의 정보를 담고 있는 태그입니다.==

위 코드의 메타 정보는 실제 문서가 다루고 있는 언어들의 셋(문자셋)을 정의하고 있습니다.

`title` 은 문서의 정보를 브라우저에 표시하는 역할을 합니다.

`link` 는 외부자원(external file)이라고 합니다.

마지막으로 문서의 본문영역 즉, 콘텐츠 영역을 의미하는 `body` 태그에 웹 페이지에 표현되는 콘텐츠를 작성하게 됩니다.

## DOCTYPE 버전 정보

 
 ==DTD = Document Type Definition = 문서 형식 선언 = HTML 버전 정보==

유효한(valid) HTML 문서를 만들기 위해서는 HTML 버전 정보를 명시해야 한다.

ML 4.01 문서 유형

The HTML 4.01 Transitional DTD


 - Strict DTD 에서 deprecated 요소와 속성을 포함합니다.
 - 권장되지 않는 요소나 속성을 문서에 포함할 필요가 있을 때, 하위 호환성을 위해 이 선언문을 사용하면 됩니다.



```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```



The HTML 4.01 Strict DTD


 - Traditional DTD에서 deprecated된 요소와 \<frameset> 관련 요소 및 속성을 제외한 가장 엄격한 DTD입니다.
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```



The HTML 4.01 Frameset DTD

- Traditionl DTD에 frameset을 포함합니다. Frameset을 적용한 문서에서는 이 선언문을 사용해야 합니다.
- 가장 느슨한 문서 형식입니다.



```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```


### The XHTML 1.0 문서 유형



The XHTML 1.0



  - XHTML 1.0에서는 HTML 4.01의 DTD 와 유사하게 3가지 DTD 중 하나를 사용할 수 있습니다.
  - HTML 과 유사하지만 문법이 좀 더 엄격한 특징이 있습니다.
 - XML 의 응용 



```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```



The XHTML 1.1

   

- HTML 1.1에서는 하나의 DTD만 정의할 수 있습니다.
 - 이것은 기존의 XHTML 1.0 Strict DTD를 기본으로 합니다.



```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```





---
참조 -  https://webclub.tistory.com/608