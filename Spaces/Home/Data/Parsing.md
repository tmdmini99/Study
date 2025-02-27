파싱(Parsing)은 컴퓨터 과학 및 프로그래밍에서 특정 형식으로 구성된 데이터를 분석하고 그 의미를 이해하는 과정을 의미합니다. 파싱은 주로 텍스트 기반 데이터를 해석하거나, 프로그래밍 언어의 소스 코드를 이해하거나, 문서를 구조화하고 내용을 추출하는 데 사용됩니다.


![[Parsing.png]]

 **웹페이지에서 원하는 데이터를 추출하여 가공하기 쉬운 상태로 바꾸는** **것**이다.
웹페이지에서 떠다니는 데이터(실제로는 떠다니지 않지만)는 리스트, 딕셔너리 같은 자료구조와 달라 사용자 마음대로 접근하고 자르고 추가하고 지지고 볶기가 쉽지 않다. 그렇기 때문에 이런 **데이터들을 다루기 쉬운 형태로 바꿔주는 과정**이 필요한데, 이 역할을 하는 함수나 프로그램을 **파서(parser)** 라고 하며, 이 과정을 **파싱(parsing)** 이라고 한다.

자료형변환도 파싱의 일종이다.


파싱은 다양한 문맥에서 사용되며, 아래에서 몇 가지 예를 들겠습니다:

1. 문자열 파싱

- 문자열에서 특정 데이터를 추출하거나 원하는 형식으로 변환하는 프로세스입니다. 예를 들어, CSV 파일을 파싱하여 표 데이터를 추출하거나, JSON 문자열을 객체로 변환하는 것이 문자열 파싱의 예시입니다.

2. 언어 파싱

- 프로그래밍 언어 또는 마크업 언어의 소스 코드를 해석하고, 구문 오류를 확인하며, 실행 가능한 코드로 변환하는 프로세스를 포함합니다. 이는 컴파일러나 인터프리터에서 주로 수행됩니다.

3. HTML 및 XML 파싱

- 웹 페이지의 HTML 또는 XML 문서를 해석하여 웹 브라우저가 문서를 렌더링하거나, 웹 스크래핑 도구가 웹 사이트에서 데이터를 추출하는 데 사용됩니다.

4. 구문 분석 (Parsing)

- 자연어 처리에서 텍스트 문서를 분석하여 문장 구조, 어휘, 문법, 의미 등을 이해하는 과정입니다. 이를 통해 검색 엔진이 검색 쿼리를 이해하거나 기계 번역 시스템이 언어를 해석하는 데 사용됩니다.

5. 데이터 포맷 파싱

- 특정 데이터 형식(예: XML, JSON, YAML)을 분석하여 데이터를 추출하고 처리하는 프로세스입니다. 이러한 형식은 데이터 교환 및 저장에 사용되며, 응용 프로그램 간에 데이터를 공유할 때 중요한 역할을 합니다.

파싱은 컴퓨터 과학과 소프트웨어 개발에서 중요한 역할을 하며, 다양한 분야에서 데이터 처리와 해석에 사용됩니다.


Parsing 기법으로 XMl 파싱 기법인 DOM과 SAX / JSON 파싱 기법이 있다






---
참조 - https://velog.io/@mercurios0603/%ED%8C%8C%EC%8B%B1Parsing%EC%9D%B4%EB%9E%80

https://hengbokhan.tistory.com/134