1. iframe이란?

아이프레임은 HTML Inline Frame 요소이며 **inline frame의 약자**이다. 

효과적으로 **다른 HTML 페이지를 현재 페이지에 포함**시키는 중첩된 브라우저로

iframe 요소를 이용하면 해당 웹 페이지 안에 어떠한 제한 없이 다른 페이지를 불러와서 삽입 할 수 있다.

2. iframe 속성

1. iframe이란?
아이프레임은 HTML Inline Frame 요소이며 inline frame의 약자이다. 

효과적으로 다른 HTML 페이지를 현재 페이지에 포함시키는 중첩된 브라우저로

iframe 요소를 이용하면 해당 웹 페이지 안에 어떠한 제한 없이 다른 페이지를 불러와서 삽입 할 수 있다.

 

2. iframe 속성

| 속성명   | 속성값                                                       | 설명                                                      |
|----------|------------------------------------------------------------|-----------------------------------------------------------|
| height   | 픽셀                                                       | \<iframe> 요소의 높이를 명시함.                          |
| name     | 텍스트                                                     | \<iframe> 요소의 이름(name)을 명시함.                   |
| sandbox  | allow-forms\<br>allow-pointer-lock\<br>allow-popups\<br>allow-same-origin\<br>allow-scripts\<br>allow-top-navigation | \<iframe> 요소에 보일 콘텐츠에 대한 추가적인 제한 사항(restrictions)들의 집합을 명시함. |
| src      | URL                                                        | \<iframe> 요소에 보일 문서의 URL를 명시함.              |
| srcdoc   | HTML 코드                                                  | \<iframe> 요소에 보일 웹 페이지의 HTML 코드를 명시함.   |
| width    | 픽셀                                                       | \<iframe> 요소의 너비를 명시함.                          |
| target   | _blank\<br>_self\<br>_parent\<br>_top                    | 프레임명\<br>새창에서 열기\<br>내용을 현재 프레임 영역에서 열기(포커스가 있는 프레임 / 기본값)<br>내용을 부모 프레임 영역에 열기\<br>내용을 무조건 전체 영역에 열기\<br>해당 이름을 가진 프레임 영역에 열기 |



3. iframe 사용법
// 다른 HTML 페이지(티스토리 메인페이지)를 현재 페이지에 포함
// 기본테두리를 삭제함
// 티스토리 글쓰기 > HTML모드에서 내용 삽입
// iframe안에 요소가 있을 경우, iframe이 사용 불가능한 환경에서 대체되어 보여짐.
```jsp
<iframe src="https://www.tistory.com/" style="width:100%; height:300px">
	<p>현재 사용 중인 브라우저는 iframe 요소를 지원하지 않습니다!</p>
</iframe>
```



![[Pasted image 20241023134628.png]]




4. 주의사항

HTML5가 차세대 웹표준으로 정해진 이후, 다음에 이유를 때문에 iframe생성을 자제하는 것을 권장한다.  
- XSS(cross site scripting)에 취약  
- 반응형 웹 사이트가 대세인 오늘날의 트렌드와 상극이다.  
- 웹의 개념 모델과 일치하지 않기 때문에 검색 엔진에 문제를 일으킬 수 있다.   
- 웹접근성 저해의 요인이 될 수 있음으로 남용에 주의해야한다.  
- 프레임 구조가 가지고 있던 장점을 CSS와 jQuery로 해결 할 수 있다.










---
출처 - https://kje1218.tistory.com/10