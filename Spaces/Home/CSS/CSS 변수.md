
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

```css
.newCustomer {
    /* --new-font-color가 없으면 orange를 사용 */
    color: var(--new-font-color, orange);  
}

.oldCustomer {
    /* --old-font-color가 없으면 var(--normal-font-color, blue)를 사용한다 */
    /* 그런데 또 --normal-font-color가 없으면 blue를 사용한다 */
    color: var(--old-font-color, var(--normal-font-color, blue)); 
}
```

### **CSS 변수 사용 주의사항**

#### **유효하지 않은 CSS 변수값이 설정 될 경우**

브라우저가 유효하지 않은 var()를 만나면 그 속성의 초기값이나 상속된 값을 사용한다.

예를들어 아래 .chicken 선택자에 변수값이 16px로 유효하지 않은 color 값이 들어간 셈이다. 이 경우, v.chicken의 부모 엘리먼트의 color 속성값을 상속받게 된다. 그런데 만일 .chicken의 부모 엘리먼트가 없을경우 브라우저의 기본 값이 들어간다.


```css
:root {
	--chicken-color: 16px;  
}
.chicken {
	color: blue;
}
.chicken2 {
	color: var(--chicken-color);  /* color: 16px은 유효하지 않음 */
}
```



#### **변수로 단위값 사용 주의사항**

만일 속성의 단위 값을 변수로 처리해야 할때 보통 아래와 같이 설정하면 된다고 생각할테인데, 실제 브라우저는 숫자와 단위 사위의 무의미한 공백이 발생하면서 두개의 토큰으로 인식하는 문제점이 발생한다.


```css
.block {
   --p: 33;
   width: var(--p)vw;
   height: var(--p)vh;
}

/*
.block {
   width: 33 vw; 띄어쓰기가 적용되어 에러를 발생시킨다
   height: 33 vh;
}
*/
```



따라서 이 경우 `calc()` 함수를 사용해야 원하는 의도를 이행할 수 있다.


```css
.block {
   --p: 33;
   width: calc(var(--p) * 1vw);
   height: calc(var(--p) * 1vh);
}
```



### **CSS 변수 활용 예제**

#### **color의 rgb를 변수화**

단순히 16진수 색상 코드(#fff)를 변수에 저장하는 것을 넘어서, rgb 및 alpha(투명도)를 변수로 세밀하게 조정 할 수도 있다.


```css
:root {
    --color: 240, 240, 240;
    --alpha: 0.8;
}

div {
	background-color: rgba(var(--color), var(--alpha)); /* rgba(240, 240, 240, 80%) */
}
```



#### **background-image의 url을 변수화**

요소의 배경 이미지 경로를 변수로 처리하여야 할때 주의해야할 점이 있다. 

예를들어 아래와 같이 이미지 경로만을 변수에 설정하고 속성 부분에서 ~~url(var(--img))~~ 식으로 호출하는 방법은 문법적으로 지원하지 않는다.


```css
:root {
	--img: "img/sample.jpg";
}

div {
    background: url(var(--img)); /* ERROR - 동작되지 않음 */
}
```


따라서 `url(이미지경로)` 자체를 변수화하여 사용하면 된다.


```css
:root {
	--img: url("img/sample.jpg");
}

div {
    background: var(--img);
}
```



## **JavaScript에서 CSS 변수 조작하기**

자바스크립트로 여타 스타일 속성을 변화 시켜 동적인 알록달록한 홈페이지를 구성했듯이 CSS 변수도 조작이 가능하다.

### **자바스크립트로 CSS 변수값 얻기**

예를 들어 앞의 예에서 루트 가상 클래스(:root)에 선언한 특정 변수의 값을 가져오는 스크립트 코드는 다음과 같다.

```css
:root {
	--hover: red;
}
```


```js
// 가상 클래스 요소 얻기
let root = document.querySelector(':root'); 

// window.getComputedStyle 메서드를 이용하면, 해당 요소에 전역적으로 적용된 모든 형태의 style을 반환한다
let variables = getComputedStyle(root); 

// 변수 값 얻기
variables.getPropertyValue('--hover');
```

### **자바스크립트로 CSS 변수값 변경하기**

그러나 여기서 주의할 점은 `getComputedStyle()` 메서드로 스타일 속성을 가져올순 있지만, 이 속성을 바로 변경할 수 없다는 점이다. 그래서 만일 자바스크립트로 변수의 값을 동적으로 조작하고 싶다면, 다른 스타일 속성 접근하듯이 **인라인 스타일**로 조작하는 수 밖에 없다.

다음은 CSS 변수에 배경 이미지 값을 설정하고, 버튼을 누르면 이 CSS 변수값을 변경함으로써 요소의 배경 이미지를 변화 시키는 예제이다. 먼저 아래와 같이 html 인라인 스타일에 배경 이미지의 CSS 변수를 정의해 놓는다.

```html
<div style="--bg-img: url(https://p4.wallpaperbetter.com/wallpaper/757/701/315/beach-high-definition-1920x1080-nature-beaches-hd-art-wallpaper-preview.jpg)"></div>

<button>배경 이미지 바꾸기</button>
```



```css
div {
    background: var(--bg-img);
    background-size: cover;
    width: 500px;
    height: 300px;
}
```


```js
document.querySelector('button').addEventListener('click', (e) => {
    const $div = document.querySelector('div');
    $div.style.setProperty('--bg-img', 'url(https://cdn.imweb.me/upload/S201903225c9441f9db892/219f92a94fb2a.jpg)');
})
```


```html
<div style="--bg-img: url(https://p4.wallpaperbetter.com/wallpaper/757/701/315/beach-high-definition-1920x1080-nature-beaches-hd-art-wallpaper-preview.jpg)"></div>

<button>배경 이미지 바꾸기</button>
```

```css
div {
  background: var(--bg-img);
  background-size: cover;
  width: 500px;
  height: 300px;
}
```


```js
document.querySelector('button').addEventListener('click', (e) => {
  document.querySelector('div').style.setProperty('--bg-img', 'url(https://cdn.imweb.me/upload/S201903225c9441f9db892/219f92a94fb2a.jpg)');
})
```


### **자바스크립트로 CSS 변수 다룰때 주의점**

#### **1. CSS 변수가 중복될 경우 우선순위**

이런식으로 css 변수를 활용할때 주의할점은 자바스크립트로 변수 값을 변경하는 경우 **인라인으로 CSS가 선언**되기 때문에, 별도의 css 파일에 정의한 변수보다 속성 적용 우선 순위가 높아 만일 변수를 중복 선언된 게 있을 경우 주의를 요하여야 한다. 

#### **2. 전역으로 CSS 변수를 사용해야 한다면 상위 요소에 선언**

또한 특정 html 태그 요소에 인라인으로 css 변수를 선언한다면, 그 변수는 해당 요소에만 들어있는 **지역변수**로서 취급 되기 때문에 다른 요소에서 가져다 쓰지 못한다. 이때는 \<html> 이나 \<body> 태그와 같은 상위 요소에 인라인 스타일로 css 변수를 저장하는 식으로, 위에서 배웠던 css 변수 상속 기능을 응용하면 된다.






---
출처 - https://inpa.tistory.com/entry/CSS-%F0%9F%93%9A-CSS%EC%9A%A9-%EB%B3%80%EC%88%98-variable-%EC%A0%95%EB%A6%AC