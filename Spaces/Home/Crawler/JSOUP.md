
# JSOUP
Java html parser 로 html 형식의 string 을 java 에 넘겨주면 java에서 사용할 수있는 DOM 객체로 만들어 주는 parser 이지, 웹페이지를 읽어들이는 기능을 하는 lib 는 아니다

**자바로 만들어진 HTML 파서 이며 쉽고 강력한 기능을 제공한다.**

- URL, 파일, 문자열을 소스로 하여 html을 파싱 가능
- DOM 구조를 추적하거나 익숙한 CSS선택자를 사용하여 데이터를 찾아 추출 가능
- 문서내의 HTML요소, 속성, 텍스트 조작 가능
- 사용자가 입력한 데이터로부터 XSS(Cross-Site Script) 공격을 방지하기 위해서 안전한 화이트 리스트 방식으로 지정된 태그만 남기고 제거 가능


**1. 문서의 파싱**

문서는 URL, 파일, 문자열로 부터 파싱할 수 있습니다.

**1.1. 문서전체를 가지고 있는 문자열로부터 파싱하는 예입니다.**

```java
import org.jsoup.Jsoup; import org.jsoup.nodes.Document;
... 
String html = "<title>First parse</title>"
            + "<p>Parsed HTML into a doc.</p>";
Document doc = Jsoup.parse(html);
```

문서로부터 html  tag 를 모두 제거 하고 순수 문자열만 얻고자 할 때는 **String text = doc.text();** 를 사용합니다.

**1.2. 문서의 body 일부분을 가지고 있는 문자열로부터 파싱합니다.**

```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
...
String html = "<div><p>Lorem ipsum.</p>";
Document doc = Jsoup.parseBodyFragment(html);
Element body = doc.body();
```

**doc.body()** 메소드는 문서의 body 요소를 추출합니다. **doc.getElementByTag("body")** 와 동일합니다.

사용자가 웹페이지의 폼으로 부터 입력한 html 태그를 포함한 입력 내용에서 **cross-site scripting** 공격을 피하기 위해서 화이트 리스트 기반의 tag 제거 기능을 사용할 수 있습니다.

**Jsop.clean(String bodyHtml, Whitelist whitelist);**

허용하는 tag 를 지정하는 **Whitelist** 클래스에 대해서는 API 문서를 참조 하세요.

**1.3.  URL로부터 문서를 파싱하는 방법 입니다. GET 방식의 호출을 합니다.**

```java
Document doc = Jsoup.connect("http://example.com/").get();
String title = doc.title();
```

POST 방식으로 사용할  수도 있습니다.

```java
Document doc = Jsoup.connect("http://example.com")
                    .data("query", "Java")
                    .userAgent("Mozilla")
                    .cookie("auth", "token")
                    .timeout(3000)
                    .post();
```

**jsoup는 http 와 https** 만을 지원합니다.

**1.4. 파일로부터 파싱하는 방법 입니다.**

```java
File input = new File("/tmp/input.html");
Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");
```

**parse** 메소드의 첫 번째 인자는 파싱할 파일의 **File 객체**입니다. 두 번째 인자는 파일의 **캐릭터셋**입니다. 세 번째 인자는 파일내의 a, img 태그 등의 **base url** 입니다. base url 이 없는 오버로드 된 메소드도 있습니다.(**Jsoup.parse(input, "UTF-8");**)

**2. 문서의 내부 돌아다니기**

파싱을 해서 Document 객체가 만들어지면 Document내에서 원하는 데이터를 추출하거나 조작을 할 수 있습니다.

**2.1. 먼저 필요한 요소를 찾습니다.**

* **getElementById(String id)** : Element 객체를 반환합니다. 하나를 반환합니다. 없으면 null 을 반환합니다.

* **getElementsByTag(String tag)** : Elements 객체를 반환합니다. 없으면 size() 가 0 입니다.

* **getElementsByClass(String className)** : Elements 객체를 반환합니다. 없으면 size() 가 0 입니다.

이 이외에도 많은 메소드들이 있습니다. API를 참조 하세요.

Elements 객체로 반환하는 것은 선택이 되었는지 확인하기 위해서 size() 를 체크합니다. Element 객체를 반환하는것은 선택인 되지 않으면 null 을 반환하므로, null 체크를 해야 NullpointerException 을 예방할 수 있습니다.

**2.2. Element 객체가 할 수 있는 작업 입니다.**

* **attr(String key)** 로 속성의 값을 얻습니다. **attr(String key, String value)**로 속성의 각을 설정할 수 있습니다.

* **id(), className()** 은 id와 class속성의 값을 가져옵니다. class는 여러개 지정되면 하나의 문자열로 반환됩니다.  예로 요소가 **\<div class="center red">** 라면 **className()** 은 **"center red"** 를 반환합니다. 하나씩 구하기 위해서는 **classNames()** 메소드를 사용합니다. **Set\<String>** 타입으로 반환합니다.

* **text()** 로 순수 텍스트만 구합니다. **text(String value)** 로 요소의 텍스트를 설정할 수 있습니다.

* **html()** 로 html 문자열을 구합니다.  **html(String value)**  메소드로 **inner HTML** 을 설정합니다.

* **outerHtml()** 요소의 outer html을 반환합니다.

**inner HTML** 은 요소가 포함하는 html을 나타내고, **outer html** 은 요소 자체 태그까지 포함하는 것입니다.

**※ 문서내의 모든 <img> 태그들중 첫 번째  img 요소의 src 속성 값을 구하려면 다음과 같이 할 수 있습니다 .**

```java
Elements imgs = doc.getElementByTag("img");
if(imgs.size() > 0) {
    String src = imgs.get(0).attr("src');
}
```

Elements 객체는 ArrayList를 상속해서 만들어졌습니다. 그러므로 내부의 Element 들을 다루기 위해서 ArrayList와 동일한 방법을 사용할 수 있습니다. 

다음 처럼 사용할 수도 있습니다.

```java
Element img = doc.getElementByTag("img").first();
if(img != null) {
    String src = img.attr("src");
}
```

**2.3. HTML  과 text 조작하기**

*** append(String html), prepend(String html)** : 선택된 요소의 뒤(append)와 앞(prepend)에 html 을 추가합니다.

*** appendText(String text), prependText(String text)** : 선택된 요소의 뒤(append)와 앞(prepend)에 text를 추가합니다.

*** appendElement(String tagName), prependElement(String tagName)** : 선택된 요소의 뒤(append)와 앞(prepend)에 Element를 추가합니다.

*** html(String value)** : 선택된 요소에 inner html 을 설정 합니다.

*** remove()** : 선택된 요소를 삭제 합니다.

- 모든 \<table> 요소를 삭제하는 예제

```java
Elements tables = doc.select("table");
for(Element table : tables) {
    table.remove(); 
}
```

**3. CSS 스타일로 요소를 선택하기**

jsoup은 CSS 스타일의 선택 기능을 제공합니다. 이 기능을 Document, Elements, Element가 가지고 있는 **select**메소드를 수행합니다.

*  **doc.select("a")** : \<a> 요소를 모두 선택합니다.

* **doc.select("#logo")** : id="logo" 인 요소를 선택합니다.

* **doc.select(".head")** : class="head"인 요소들을 선택합니다.

* **doc.select("[href]")** : href 속성을 가진 요소들을 선택합니다.

* **doc.select("[width=500]")** : width 속성의 값이 500인 모든 요소들을 선택합니다.

다음 처럼도 사용할 수 있습니다.

**doc.select("div").select(".head").select("[width=500]");**

단일 Element가 반환되는 경우에 이런 표현식을 쓰면 안됩니다. NullpointerException 이 발생할 수 있습니다.

다음은 div 요소들중에서 class가 "logo" 가 아닌것들 을 선택합니다.

**Elements divs = doc.select("div").not(".logo");**

더 많은 기능들은 API 문서를 참조하세요.




---
참조 -  https://pso62.tistory.com/19
https://offbyone.tistory.com/116


