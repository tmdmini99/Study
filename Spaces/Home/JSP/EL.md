JSP(JavaServer Pages)에서 `${}`는 EL(Expression Language) 표현식을 나타냅니다. EL은 자바 빈의 프로퍼티나 값을 JSP의 전통적인 표현식(`<%= %>`)이나 액션 태그(`<jsp:useBean>`)를 사용하는 것보다 더 쉽고 간결하게 꺼낼 수 있게 해주는 기술입니다. 😊

---

### EL(Expression Language) 특징

- **간결한 문법**: `${}`를 사용하여 간결하게 값을 표현할 수 있습니다.
- **즉시 평가(Immediate evaluation)**: `${}`는 JSP가 실행될 때 즉시 해당 값이 평가되어 반영됩니다.
- **객체 프로퍼티 접근**: 객체의 프로퍼티 값을 쉽게 꺼낼 때 주로 사용됩니다.
- **서블릿 보관소 접근**: `JspContext`, `ServletRequest`, `HttpSession`, `ServletContext` 등에서 값을 꺼낼 때 사용됩니다.

예를 들어, `${member.no}` 또는 `${member["no"]}`와 같은 EL 표현식을 사용하여 `member` 객체의 `no` 프로퍼티 값을 쉽게 가져올 수 있습니다.

EL 표현식은 JSP 2.0에서 새롭게 추가된 스크립트 언어로, 기존의 스크립틀릿(`<% %>`)에서 업그레이드된 버전입니다. 이를 통해 JSP 페이지에서 Java 코드를 작성하지 않고도 변수, 객체, 메서드 등을 쉽게 참조할 수 있습니다1.