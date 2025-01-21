### JSTL의 주요 기능

- **조건문과 반복문**: `<c:if>`, `<c:forEach>` 등의 태그를 사용하여 조건문과 반복문을 간결하게 표현할 수 있습니다.
- **데이터베이스 연동**: `<sql:query>`, `<sql:update>` 등의 태그를 통해 데이터베이스와의 연동을 쉽게 구현할 수 있습니다.
- **국제화 및 형식화**: `<fmt:formatNumber>`, `<fmt:formatDate>` 등의 태그를 사용하여 숫자, 날짜, 시간 등을 다양한 형식으로 표현할 수 있습니다.
- **XML 처리**: `<x:parse>`, `<x:forEach>` 등의 태그를 사용하여 XML 문서를 처리할 수 있습니다.

### JSTL 사용 방법

1. **라이브러리 다운로드**: JSTL 라이브러리(`javax.servlet.jsp.jstl-api` 및 `javax.servlet.jsp.jstl-implementation`)를 다운로드하고, 프로젝트의 `WEB-INF/lib` 디렉토리에 추가합니다.
2. **태그 라이브러리 선언**: JSTL 태그를 사용하고자 하는 JSP 파일 상단에 태그 라이브러리를 선언합니다.
    
    jsp
    
    ```jsp
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```
    

JSTL은 개발자가 JSP 페이지를 더 효율적으로 개발할 수 있도록 도와주며, 다양한 웹 애플리케이션 프레임워크와 함께 사용될 수 있습니다. 예를 들어, Spring Framework와 함께 사용하면 Spring MVC에서 JSP를 개발할 때 JSTL을 쉽게 활용할 수 있습니다




## escape




### 사용 예시

- **이스케이프 하지 않은 경우**:

```jsp
<p>${bodyHtml}</p>
```


- - 만약 `bodyHtml`의 값이 `<script>alert('XSS');</script>`라면, 사용자는 경고창을 보게 될 것입니다. 즉, 브라우저가 이를 HTML로 해석하여 스크립트를 실행합니다.
- **이스케이프 한 경우**:

```jsp
${fn:escapeXml(bodyHtml)}
```


- - 이 경우, `<script>`는 `&lt;script&gt;`로 변환되어 사용자는 그대로 텍스트로 보여주게 됩니다. 이는 안전한 방식입니다.

### 결론

따라서 `${fn:escapeXml(bodyHtml)}`을 사용함으로써:

- 사용자의 입력을 안전하게 출력할 수 있으며,
- 의도치 않은 HTML 해석을 방지하여, 사용자와 시스템의 보안을 강화할 수 있습니다.



```jsp

```

### `data['tag' + index]`가 가능한 이유

EL에서는 `data['tag1']`처럼 점 표기법과 배열 표기법을 지원합니다. `data.tag1`과 `data['tag1']`은 동일합니다. 배열 표기법(`['key']`)은 문자열이나 동적으로 생성된 키를 사용할 때 유용합니다.