
## **CSS 변수 기능**

복잡한 웹사이트 같은 경우 엄청난 양의 CSS를 가지고 있는데, 유지보수를 하다 보면 여러 곳에서 사용한 임의의 값을 한꺼번에 바꿔야 할 때가 있다. 그 값들을 하나씩 찾아서 일일이 바꾸려면 상당히 피곤할 것이고, 때에 따라 실수도 많을 것이다.

그래서 여타 프로그래밍 언어처럼 **변수**를 이용하면, 요소 전반에 걸쳐 사용되는 임의의 값을 변수에 저장하고 호출하면 보다 효율적인 프로그래밍이 가능해진다.


![[Pasted image 20241010153933.png]]


### ****CSS** 변수 선언 및 호출**

CSS에서 변수의 이름을 지정할 때는 변수 맨 앞에 ~~**--**~~ 를 붙여주어야 한다. 그리고 변수를 호출해 사용할 때는 `var(변수명)`형식을 사용한다.


```css
:root {
	--main-font-color: #000f22;  /* CSS 전역 변수 선언 */
}

div {
	color: var(--main-font-color);   /* CSS 변수 사용 */
}
```



### **CSS 변수 선언 스코프**

#### **전역 변수 스코프**

CSS의 \:root 의사클래스는 문서 트리의 루트 요소를 선택한다. 즉, HTML 문서의 루트 요소는 \<html> 요소이므로, :root와 html은 같다고 보면 된다.

보통 이 영역에 css 변수를 선언해 사용한다고 보면된다. 자바스크립트로 따지면 일종의 전역 변수 영역이라고 이해하면 된다. :root에서 CSS 변수를 선언하여 HTML 문서 어디에서나 해당 변수에 접근할 수 있도록 구성하는 것은 흔한 패턴이다.


![[Pasted image 20241010154044.png]]



#### **지역 변수 스코프**

요소 하나하나 마다 지역 변수로서 활용할 수 있다. 지역변수로 선언된 CSS 변수는 **변수명이 같은지라도** 전체 요소에 공유되지 않고 오로지 그 요소에서만 적용되게 된다.


```css
div {
	--font-color: #000f22;  /* CSS 지역 변수 선언 */
    color: var(--font-color);   /* CSS 변수 사용 */
}

p {
	--font-color: #000fff;  /* CSS 지역 변수 선언 */
    color: var(--font-color);   /* CSS 변수 사용 */	
}
```




### **CSS 변수의 상속**

CSS 변수 역시 **상속**되는 특성을 지니고 있다. 보통 HTML에서 자식 엘리먼트에 값이 지정되지 않은 경우 부모 엘리먼트의 값을 상속받는데, CSS 변수로 선언된 값이라도 예외 없이 상속된다.

- div.one의 font-size : 10px
- div.two의 font-size : 2rem
- div.three의 font-size : 10px (.one에서 변수를 상속 받음)


```html
<div class='one'>
  <div class='two'></div>
  <div class='three'></div>
</div>
```


```css
.one {
	--test: 10px;
    font-size: var(--test);
}
.two {
	--test: 2rem;
    font-size: var(--test); /* 2rem */
}

.three {
	font-size: var(--test); /* 10px */ /* .one에서 변수를 상속 받음 */
}
```




### **CSS 변수 대체값 설정**

만일 CSS 변수의 수가 너무 많아, 특정 변수가 정의 됬는지 안됬는지 모를떄, 변수값이 없다면, ~~var()~~ 를 중첩함으로써 여러 개의 **대체 변수**를 정의할 수 있다.








---
출처 - https://inpa.tistory.com/entry/CSS-%F0%9F%93%9A-CSS%EC%9A%A9-%EB%B3%80%EC%88%98-variable-%EC%A0%95%EB%A6%AC