## **JSTL****이란?**

- JSP 표준 태그 라이브러리(여러 프로그램이 공통으로 사용하는 코드를 모아놓은 코드의 집합)의 약어

- 자신만의 태그를 추가할 수 있는 기능을 제공한다.

- 주로 JSTL의 Core에서 c를 사용하여 <c:if>, <c:forEach>등으로 사용한다.






## **EL (Expression Language)**

**<%= %> , out.println()과 같은 자바코드를** 

**더 이상 사용하지 않고 좀더 간편하게 출력을 지원하기 위한 도구.**

**배열이나 컬렉션에서도 사용되고, JavaBean의 프로퍼티에서도 사용됩니다.**


**Attribute형식에서는 <%= cnt + 1 %>를 쓰지 않고 ${cnt + 1}로 쓰고**

**Parameter형식에서는 ${param.abc}으로 씁니다.**

**여기서 cnt는 자바에서는 변수 이름이고, EL 식에서는 Attribute의 이름으로 해석되는데요.** 

**값을 찾을때 Attribute는 작은 Scope에서 큰 Scope로 찾습니다.**

**(page → request → session → application)**


 >attribute란? : 메소드를 통해 저장되고 관리되는 데이터 
 >
PageContext / Request에서 사용될때
>
setAttribute("key", value) → 값을 넣는다.  
>
getAttribute("key") → 값을 가져온다.
>
removeAttribue("key") → 값을 지운다.
>
session에서 사용될때
>
set / get / remove 동일하고 추가로++
>
invalidate( ) → 값을 전부 지운다.


![[EL1.jpg]]

**▼ 내장객체**

1) pageScope → 페이지Scope에 접근

2) request Scope → 리퀘스트Scope에 접근

3) sessionScope → 세션Scope에 접근

4) applicationScope → 어플리케이션Scope에 접근

5) param → 파라미터값 얻어올때 ( 1개의 Key에 1개의 Value )

6) paramValues → 파라미터값 배열로 얻어올때( 1개의 Key에 여러개의 Value)

7) header → 헤더값 얻어올때 ( 1개의 Key에 1개의 Value )

8) headerValues → 헤더값 배열로 얻어올때 ( 1개의 Key에 여러개의 Value )

9) cookie → ${cookie. key값. value값}으로 쿠키값 조회

10) initParam → 초기 파라미터 조회

11) pageContext → 페이지컨텍스트 객체를 참조할때

▼ paramValues 나 headerValues 사용법

① $ { paramValues . boadDto [0] }

② $ { paramValues ["bardDto"] [1] }

Values 옆에 점을 찍는 방법과 대괄호로 묶어 사용하는 2가지 방법이 있습니다.

대신 ①번에서는 인덱스가 0부터 시작하고 ②번에서는 인덱스가 1부터 시작하네요










---
참조  - https://hunit.tistory.com/203