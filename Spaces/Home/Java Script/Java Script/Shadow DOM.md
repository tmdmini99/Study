## **웹 컴포넌트에 있어서 Shadow DOM의 역할은?**

DOM API는 자체적으로 캡슐화를 지원하지 않는다. 캡슐화를 지원하지 않는 다는 것은 스타일 정보가 다른 트리 요소로 누출될 수 있기 때문에 커스텀 요소를 개발하기 어렵고, id가 다른 요소간에 겹칠 수 있음을 의미한다.

**캡슐화**는 웹 컴포넌트의 중요한 개념이다. Shadow DOM은 마크업 구조, 스타일 및 동작을 **숨겨서**, 페이지의 **다른 코드와 충돌하지 않도록** 코드를 깨끗하게 유지해 준다.

## **Shadow DOM 이란?**



![[Shadow DOM1.jpg]]




 **Shadow host:** Shadow DOM이 붙어 있는 일반적인 DOM 노드

 **- Shadow tree:** Shadow DOM 안에 있는 DOM 트리

 **- Shadow root:** Shadow tree의 루트 노드

 **- Shadow boundary:** Shadow DOM이 끝나고 일반 DOM이 시작되는 곳

Shadow DOM 은 일반적인 DOM과 조작 방법(자식 요소 추가나 스타일을 넣는 등)에서 차이가 없지만, **shadow DOM 내부의 어떤 코드도 외부에 영향을 줄 수 없는 캡슐화**를 허용한다는 차이점이 있다.



![[Shadow DOM2.png]]



## **기본 사용법**

**Element.attachShadow()** 메서드를 이용해, 엘레멘트에 shadow root를 붙일 수 있다.

```js
let shadow = element.attachShadow({mode: 'open'});
let shadow = element.attachShadow({mode: 'closed'});
```

**open:** 바깥에서 작성된 javascript를 사용하여 shadow dom에 접근을 허용한다.

```js
const $myWebComponent = document.querySelector("my-web-component"); 
$myWebComponent.shadowRoot.querySelector("p").innerText = "수정됨!";
```




![[Shadow DOM3.gif]]



**closed:** 바깥에서 작성된 javascript를 사용하여 shadow dom에 접근할 수 없다.

```js
let $element = document.createElement("div"); 
$element.attachShadow({ mode: "closed" }); 
$element.shadowRoot // null
```


![[Shadow DOM4.gif]]



## **예제**

**1. HTMLElement를 상속받는 클래스를 만든다.**

```java
class PopUpInfo extends HTMLElement {
  constructor() {
    // Always call super first in constructor
    super();

    // write element functionality in here

    ...
  }
}
```

**2. shadow root를 만들어 붙인다.**

```js
// Create a shadow root
let shadow = this.attachShadow({mode: 'open'});
```

**3. shadow dom 구조를 만든다.** 

```js
// Create spans
let wrapper = document.createElement('span');
wrapper.setAttribute('class', 'wrapper');
let icon = document.createElement('span');
icon.setAttribute('class', 'icon');
icon.setAttribute('tabindex', 0);
let info = document.createElement('span');
info.setAttribute('class', 'info');

// Take attribute content and put it inside the info span
let text = this.getAttribute('data-text');
info.textContent = text;

// Insert icon
let imgUrl;
if(this.hasAttribute('img')) {
  imgUrl = this.getAttribute('img');
} else {
  imgUrl = 'img/default.png';
}
let img = document.createElement('img');
img.src = imgUrl;
icon.appendChild(img);
```

**4. shadow DOM을 스타일링한다.**

```js
// Create some CSS to apply to the shadow dom
let style = document.createElement('style');

style.textContent = `
.wrapper {
  position: relative;
}

.info {
  font-size: 0.8rem;
  width: 200px;
  display: inline-block;
  border: 1px solid black;
  padding: 10px;
  background: white;
  border-radius: 10px;
  opacity: 0;
  transition: 0.6s all;
  position: absolute;
  bottom: 20px;
  left: 10px;
  z-index: 3;
}

img {
  width: 1.2rem;
}

.icon:hover + .info, .icon:focus + .info {
  opacity: 1;
}`;
```

**5. shadow root에 shadow DOM을 붙인다.**

```js
// attach the created elements to the shadow dom
shadow.appendChild(style);
shadow.appendChild(wrapper);
wrapper.appendChild(icon);
wrapper.appendChild(info);
```

**6. 커스텀 엘레멘트 사용하기**

```js
// Define the new element
customElements.define('popup-info', PopUpInfo);
```

```html
<popup-info img="img/alt.png" data-text="Your card validation code (CVC)">
```

**7. 외부의 스타일 참조하기**

\<link> 태그로 외부 스타일시트를 참조하면 가능하다.

```js
// Apply external styles to the shadow dom
const linkElem = document.createElement('link');
linkElem.setAttribute('rel', 'stylesheet');
linkElem.setAttribute('href', 'style.css');

// Attach the created element to the shadow dom
shadow.appendChild(linkElem);
```

---

## **Shadow DOM을 붙일 수 있는 요소**

- 보안상의 이유로 shadow DOM을 가질 수 없는 태그가 있다. (예: \<a>)


![[Shadow DOM5.png]]




**다음은 shadow root를 연결할 수 있는 요소 목록이다.**

- custom element
- article
- aside
- blockquote
- body
- div
- footer
- h1 ~ h6
- header
- main
- nav
- p
- section
- span

**Q. button 태그는?**

**참고 사항**

- Shadow DOM 은 FireFox, Chorme, Opera, Safari 등 모든 브라우저에서 기본으로 지원한다. 크로미움 기반 엣지도 지원하지만 구 버전 엣지에서는 지원하지 않는다.

- \<link>요소는 shadow root의 paint를 차단하지 않으므로, 스타일시트가 로드되는 동안 스타일이 지정되지 않은 콘텐츠(FOUC)가 깜박일 수 있다.





```html
<div id="host"></div>

<script>
  // JavaScript를 사용하여 Shadow DOM을 생성합니다.
  const host = document.getElementById('host');
  const shadowRoot = host.attachShadow({ mode: 'open' });

  // Shadow DOM에 새로운 요소와 스타일을 추가합니다.
  shadowRoot.innerHTML = `
    <style>
      p {
        color: blue;
        font-size: 18px;
      }
    </style>
    <p>This is inside the Shadow DOM!</p>
  `;
</script>

```



```html
<div id="host"></div>

<script>
  // 호스트 요소를 선택합니다.
  const host = document.getElementById('host');

  // Shadow DOM을 'open' 모드로 생성합니다.
  const shadowRoot = host.attachShadow({ mode: 'open' });

  // 새로운 <span> 요소를 생성하여 Shadow DOM에 추가합니다.
  const span = document.createElement('span');
  span.textContent = "This is inside Shadow DOM!";

  // Shadow DOM에 <span> 요소를 추가합니다.
  shadowRoot.appendChild(span);
</script>

```



```html
<div id="host"></div>

<script>
  // 이미 존재하는 호스트 요소에 Shadow DOM이 있다면 접근합니다.
  const host = document.getElementById('host');

  // Shadow DOM에 접근합니다.
  const shadowRoot = host.shadowRoot; // 'open' 모드에서만 가능

  // 새로운 <span> 요소를 생성합니다.
  const newSpan = document.createElement('span');
  newSpan.textContent = "New content in Shadow DOM";

  // Shadow DOM에 <span> 요소를 추가합니다.
  if (shadowRoot) {
    shadowRoot.appendChild(newSpan);
  }
</script>

```


- **aScript 기술로서의 Shadow DOM**:
    
    - **Shadow DOM**은 **JavaScript**를 통해 생성되고 관리됩니다.
    - `attachShadow()` 메서드를 사용하여 Shadow DOM을 생성하고, 그 안에 JavaScript로 요소를 추가하거나 조작할 수 있습니다.
    - Shadow DOM 내부는 일반 DOM과는 별개로 동작하며, 외부 스크립트나 DOM 조작이 Shadow DOM 내부에 직접적으로 영향을 미치지 않도록 캡슐화하는 역할을 합니다.
- **CSS 기술로서의 Shadow DOM**:
    
    - **CSS**는 Shadow DOM 내에서 사용되며, Shadow DOM 내부의 스타일은 외부로부터 **독립적으로 적용**됩니다.
    - Shadow DOM 내부에서 정의된 스타일은 **캡슐화**되어, 외부의 CSS가 내부 요소에 영향을 미치지 않고, 반대로 내부 스타일도 외부 요소에 영향을 주지 않도록 합니다.
    - Shadow DOM의 이 특징 덕분에, 웹 컴포넌트의 스타일을 외부와 충돌 없이 안전하게 관리할 수 있습니다.






```jsp
<c:if test="${list[0].totalCount > 50}">  
    <div class="Polaris-IndexTable__PaginationWrapper">  
        <nav aria-label="페이지 매김" class="Polaris-Pagination Polaris-Pagination--table">  
            <div class="Polaris-Box" style="--pc-box-background: var(--p-color-bg-surface-secondary); --pc-box-padding-block-start-xs: var(--p-space-150); --pc-box-padding-block-end-xs: var(--p-space-150); --pc-box-padding-inline-start-xs: var(--p-space-300); --pc-box-padding-inline-end-xs: var(--p-space-200);">  
                <div class="Polaris-InlineStack" style="--pc-inline-stack-align: center; --pc-inline-stack-block-align: center; --pc-inline-stack-wrap: wrap; --pc-inline-stack-flex-direction-xs: row;">  
                    <div class="Polaris-Pagination__TablePaginationActions" data-buttongroup-variant="segmented">  
                        <div>  
                            <!-- 이전 페이지 버튼 -->  
                            <button id="previousURL" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly" aria-label="이전" type="button" onclick="navigatePage(-1, ${basicParamVo.pageNumber}, ${basicParamVo.totalPages})" ${basicParamVo.pageNumber==1 ? 'disabled' : '' }>  
                                <span class="Polaris-Button__Icon" id="previousIconContainer"></span>  
                            </button>  
                        </div>  
                        <div>  
                            <!-- 다음 페이지 버튼 -->  
                            <button id="nextURL" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly" aria-label="다음" type="button" onclick="navigatePage(1, ${basicParamVo.pageNumber}, ${basicParamVo.totalPages})" ${basicParamVo.pageNumber==basicParamVo.totalPages ? 'disabled' : '' }>  
                                <span class="Polaris-Button__Icon" id="nextIconContainer"></span>  
                            </button>  
                        </div>  
                    </div>  
                </div>  
            </div>  
        </nav>  
    </div>  
</c:if>
```





```js
function renderIcons() {  
    const previousIconContainer = document.getElementById('previousIconContainer');  
    const nextIconContainer = document.getElementById('nextIconContainer');  
  
    // 이전 페이지 아이콘  
    if (!previousIconContainer.shadowRoot) {  
        const shadowRootPrev = previousIconContainer.attachShadow({ mode: 'open' });  
        shadowRootPrev.innerHTML = `  
            <style>  
                .Polaris-Icon {  
                    width: 16px;                    height: 16px;                }  
            </style>            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" class="Polaris-Icon">  
                <path fill-rule="evenodd" d="M9.78 3.47a.75.75 0 0 1 0 1.06l-3.47 3.47 3.47 3.47a.749.749 0 1 1-1.06 1.06l-4-4a.75.75 0 0 1 0-1.06l4-4a.75.75 0 0 1 1.06 0"></path>  
            </svg>  
        `;  
    } else {  
        console.log('이전 페이지 아이콘이 이미 렌더링되었습니다.');  
    }  
  
    // 다음 페이지 아이콘  
    if (!nextIconContainer.shadowRoot) {  
        const shadowRootNext = nextIconContainer.attachShadow({ mode: 'open' });  
        shadowRootNext.innerHTML = `  
            <style>  
                .Polaris-Icon {  
                    width: 16px;                    height: 16px;                }  
            </style>            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" class="Polaris-Icon">  
                <path fill-rule="evenodd" d="M5.72 12.53a.75.75 0 0 1 0-1.06l3.47-3.47-3.47-3.47a.749.749 0 1 1 1.06-1.06l4 4a.75.75 0 0 1 0 1.06l-4 4a.75.75 0 0 1-1.06 0"></path>  
            </svg>  
        `;  
    } else {  
        console.log('다음 페이지 아이콘이 이미 렌더링되었습니다.');  
    }  
}
  
  
    $(document).ready(function () {  
        renderIcons(); // 초기 로딩 시 아이콘 렌더링  
    });
```


다른곳에서 쓴것도 랜더링 해줘야함



```js
// menu-component.js  
  
class SideMenu extends HTMLElement {  
    constructor() {  
        super();  
  
        const shadow = this.attachShadow({ mode: 'open' });  
  
        fetch('/components/aside.html')  
            .then(response => response.text())  
            .then(html => {  
                const parser = new DOMParser();  
                const doc = parser.parseFromString(html, 'text/html');  
                const template = doc.getElementById('aside');  
  
                shadow.appendChild(template.content.cloneNode(true));  
  
                const link = document.createElement('link');  
                link.setAttribute('rel', 'stylesheet');  
                link.setAttribute('href', '/styles/side-menu.css');    
                shadow.appendChild(link);  
  
                const link2 = document.createElement('link');  
                link2.setAttribute('rel', 'stylesheet');  
                link2.setAttribute('href', '/styles/styles.css');    
                shadow.appendChild(link2);  
  
                this.initMenu();  
            })  
            .catch(error => {  
                console.error('Error loading the template:', error);  
            });  
    }  
    initMenu() {  
    const items = this.shadowRoot.querySelectorAll(".menu > .list > .item");  
  
    items.forEach(item => {  
        const parentMenu = item.querySelector('.parent-menu');  
        const depthMenu = item.querySelector('.depth');  
  
        parentMenu.addEventListener('click', () => {  
            const isActive = item.classList.contains('active');  
  
            if (isActive) {  
                item.classList.remove('active');  
                if (depthMenu) {  
                    depthMenu.style.height = '0';  
                    depthMenu.style.transition = '0.25s';  
                }  
            } else {  
                item.classList.add('active');  
                if (depthMenu) {  
                    depthMenu.style.height = `${depthMenu.scrollHeight}px`;  
                    depthMenu.style.transition = `0.25s`;  
                }  
            }  
        });  
    });  
}  
    }  
  
// 커스텀 요소 정의  
customElements.define('side-menu', SideMenu);
```


`customElements.define('side-menu', SideMenu);` 커스터 태그 생성 코드

이렇게 사용시 안에 적용
```jsp
<side-menu></side-menu>
```


```js
import { Timer } from "./header/timer.js";  
import { getSubPageText,getSubPageTitle } from "../js/utils/page-title-utils.js";  
  
export class Header extends HTMLElement {  
    constructor() {  
        super();  
  
        const shadow = this.attachShadow({ mode: "open" });  
  
        fetch("/components/header.html")  
            .then((response) => response.text())  
            .then((html) => {  
                const parser = new DOMParser();  
                const doc = parser.parseFromString(html, "text/html");  
                const template = doc.getElementById("header");  
  
                shadow.appendChild(template.content.cloneNode(true));  
  
                // 스타일 링크 추가  
                const link = document.createElement("link");  
                link.setAttribute("rel", "stylesheet");  
                link.setAttribute("href", "/styles/header.css");  
                shadow.appendChild(link);  
  
                const link2 = document.createElement("link");  
                link2.setAttribute("rel", "stylesheet");  
                link2.setAttribute("href", "/styles/styles.css");  
                shadow.appendChild(link2);  
  
                // 타이머 연장 버튼 이벤트 추가  
                const extendButton = shadow.querySelector(".timer .extension");  
                if (extendButton) {  
                    extendButton.addEventListener("click", () => this.extendTimer());  
                }  
  
                this.initializeTimer(); // 타이머 초기화  
                this.setDynamicContent(); // URL에 따라 메인 또는 서브 페이지 구성  
            })  
            .catch((error) => {  
                console.error("Error loading the template:", error);  
            });  
    }  
  
    // 타이머 초기화  
    initializeTimer() {  
        const timerElement = this.shadowRoot.querySelector(".timer .time");  
  
        if (timerElement) {  
            this.timer = new Timer(  
                59 * 60 + 59, // 59분 59초  
                (remainingTime) => {  
                    timerElement.textContent = Timer.formatTime(remainingTime);  
                },  
                () => {  
                    window.location.href = "../index.html";  
                }  
            );  
  
            this.timer.start();  
        }  
    }  
  
    // 타이머 연장  
    extendTimer() {  
        if (this.timer) {  
            this.timer.reset(60 * 60); // 1시간으로 리셋  
        }  
    }  
  
    // URL에 따라 메인 검색창 또는 서브 페이지 제목 구성  
    setDynamicContent() {  
        const currentPath = window.location.pathname;  
        const shadow = this.shadowRoot;  
  
        const mainSearch = shadow.querySelector(".product-search-contain");  
        const subPageTitle = shadow.querySelector(".page-title");  
  
        if (currentPath === "/pages/main.html") {  
            // 메인 페이지 -> 메인 검색창 표시  
            mainSearch.style.display = "flex";  
            subPageTitle.style.display = "none";  
        } else {  
            // 서브 페이지 -> 서브 페이지 제목 표시  
            mainSearch.style.display = "none";  
            subPageTitle.style.display = "flex";  
  
            // 서브 페이지 제목 동적 설정  
            const subTitle = shadow.querySelector(".page-title h1");  
            const subText = shadow.querySelector(".page-title .box-title p");  
            subTitle.textContent = getSubPageTitle(currentPath); // 동적 제목 설정  
            subText.textContent = getSubPageText(currentPath); // 동적 내용 설정  
        }  
    }  
}  
  
// 커스텀 엘리먼트 정의  
customElements.define("comm-header", Header);
```



```jsp
<comm-header></comm-header>
```




---
출처 - https://leeproblog.tistory.com/185