#### XML이란?

XML은 EXtensible Markup Language의 약자이며, 1998년에 W3C 표준 권고안에 포함되었습니다.

XML은 HTML과 매우 비슷한 문자 기반의 마크업 언어(text-based markup language)입니다.

이 언어는 사람과 기계가 동시에 읽기 편한 구조로 되어 있습니다.

그러나 XML은 HTML처럼 데이터를 보여주는 목적이 아닌, 데이터를 저장하고 전달할 목적으로만 만들어졌습니다.

또한, XML 태그는 HTML 태그처럼 미리 정의되어 있지 않고, 사용자가 직접 정의할 수 있습니다.


#### XML의 특징

XML의 중요한 특징은 다음과 같습니다.

1. XML은 다른 목적의 마크업 언어를 만드는 데 사용되는 다목적 마크업 언어입니다.

2. XML은 다른 시스템끼리 다양한 종류의 데이터를 손쉽게 교환할 수 있도록 해줍니다.

3. XML은 새로운 태그를 만들어 추가해도 계속해서 동작하므로, 확장성이 좋습니다.

4. XML은 데이터를 보여주지 않고, 데이터를 전달하고 저장하는 것만을 목적으로 합니다.

5. XML은 텍스트 데이터 형식의 언어로 모든 XML 문서는 유니코드 문자로만 이루어집니다.



#### XML 문법

- XML문서는 매우 규칙적, 예측이 가능한 구조이다.
- 모든 XML 요소는 종료 태그를 가져야 한다. 생략되면 오류가 발생.
- 대소문자를 구분하여 대소문자가 다르면 다른 요소로 인식한다.
- 시작태그와 종료태그의 문자가 동일해야한다. 앞에는 소문자 뒤에는 대문자 불가
- XML은 띄어쓰기를 인식한다.


#### XML 데이터 예시

- 간단한 예시로 형태를 확인해본다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<note>
  <date>
    <day>07</day>
    <month>01</month>
    <year>2021</year>
  </date>
  <tistory>
    <title>XML은 무엇인가</title>
    <writer>annajang</writer>
  </tistory>
  <remark>
    <![CDATA[  CDATA부분에는 < > & " 등과 같은 문자를 그대로 표현할 수 있다  ]]>
  </remark>
 </note>
```


#### XML 주석

- ```<!-- 으로 시작하여 --> ```으로 끝난다.
- 시작태그에는 느낌표가 있지만 종료 태그에는 느낌표가 없다.

  
#### CDATA

- Character DATA의 약자이다. 문자 데이터를 XML 데이터로 해석하지 않고 그대로 표현하는 것을 의미한다.

```xml
<![CDATA[ 특수문자 혹은 노출하고 싶은 문자열을 적어준다 ]]>
<![CDATA[ 와 ]]> 에는 공백을 포함하면 안된다.
```

  
####  XML 요소

- 아래처럼 시작과 종료 태그로 한 쌍이 되어야 한다.
- <시작태그명> 요소내용 </종료태그명>


#### XML 기반의 언어

XML을 기반으로 하는 대표적인 언어는 다음과 같습니다.

1. XHTML

2. SVG

3. RDF

4. RSS

5. Atom

6. MathML












---
참조 - https://mommoo.tistory.com/17
https://tcpschool.com/xml/xml_intro_basic

https://annajang.tistory.com/43

