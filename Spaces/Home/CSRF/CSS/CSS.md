
```css
.table-hover tbody tr:hover ._ActivatorIcon {  
    opacity: 1; /* 테이블 행 hover 시 아이콘 불투명 */}  
  
._ActivatorIcon {  
    opacity: 0; /* 기본 상태에서 반투명 */    transition: opacity 0.3s ease; /* 부드러운 전환 효과 */}  
  
._ActivatorIcon.is-active {  
    opacity: 1; /* Active 상태에서도 불투명 */}
```


```css
.table-hover tbody tr:hover ._ActivatorIcon {
    opacity: 1; /* 테이블 행 hover 시 아이콘 불투명 */
}
```

- **설명**: 이 규칙은 사용자가 테이블의 행(`tr`)에 마우스를 올렸을 때(`:hover` 상태), 해당 행 안의 아이콘(`._ActivatorIcon`)의 불투명도를 `1`로 설정합니다.
- **효과**: 아이콘이 완전히 보이게 되어 사용자가 해당 행에 마우스를 올렸을 때 시각적으로 더 잘 인식할 수 있도록 돕습니다.

```css
._ActivatorIcon {
    opacity: 0; /* 기본 상태에서 반투명 */
    transition: opacity 0.3s ease; /* 부드러운 전환 효과 */
}
```

- **설명**: 기본적으로 아이콘의 불투명도를 `0`으로 설정합니다. 즉, 이 아이콘은 처음에는 보이지 않습니다.
- **`transition` 속성**: `opacity`가 변경될 때, 그 변화가 `0.3초`에 걸쳐 부드럽게 이루어지도록 설정합니다. 사용자가 테이블 행에 마우스를 올리면 아이콘이 서서히 나타나게 됩니다.




```css
._ActivatorIcon.is-active {
    opacity: 1; /* Active 상태에서도 불투명 */
}

```

- **설명**: 이 규칙은 아이콘이 `.is-active` 클래스를 가질 때 불투명도를 `1`로 설정합니다. 이는 아이콘이 활성화 상태에 있음을 나타냅니다.
- **효과**: 특정 동작(예: 클릭) 후 아이콘이 계속 보이도록 하여 사용자가 상호작용을 하였음을 알립니다.



css로 이벤트 처리 할수 있음 처리방법


1.1. 단순 배경색 변경
```css
.button {
    background-color: blue; /* 기본 색상 */
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    transition: background-color 0.3s ease; /* 부드러운 전환 효과 */
}

.button:hover {
    background-color: darkblue; /* Hover 시 색상 변경 */
}
```


1.2. 글자 색상 변경

```css
.link {
    color: black;
    text-decoration: none;
}

.link:hover {
    color: red; /* Hover 시 글자 색상 변경 */
    text-decoration: underline; /* 밑줄 추가 */
}
```


1.3. 아이콘 애니메이션
```css
.icon {
    opacity: 0; /* 기본 상태에서 반투명 */
    transition: opacity 0.3s ease; /* 부드러운 전환 효과 */
}

.item:hover .icon {
    opacity: 1; /* Hover 시 아이콘 불투명 */
}

```


### 2. CSS로 다른 상태 만들기

CSS를 사용하여 hover 상태 외에도 다양한 상태를 만들 수 있습니다.

**2.1. 포커스 상태**

입력 필드나 버튼에 포커스가 있을 때 스타일을 변경할 수 있습니다.

```css
input:focus {
    border: 2px solid blue; /* 포커스 시 테두리 색상 변경 */
}
```





**2.2. 활성 상태**

버튼 클릭 시 활성 상태에서 스타일을 변경할 수 있습니다.
```css
.button:active {
    background-color: lightblue; /* 클릭 시 배경 색상 변경 */
}
```




**2.3. 비활성 상태**

비활성 상태에서 스타일을 지정할 수 있습니다.

```css
.button:disabled {
    background-color: gray; /* 비활성 상태에서 배경 색상 변경 */
    cursor: not-allowed; /* 마우스 커서 변경 */
}

```

### 3. CSS Transition 및 Animation

CSS의 transition과 animation을 활용하여 hover 상태의 변화를 더욱 부드럽게 만들 수 있습니다.

**3.1. Transition 예시**

```css
.card {
    transform: scale(1); /* 기본 크기 */
    transition: transform 0.3s ease; /* 부드러운 전환 효과 */
}

.card:hover {
    transform: scale(1.05); /* Hover 시 크기 확대 */
}

```


3.2. Animation 예시
```css
@keyframes pulse {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.1);
    }
    100% {
        transform: scale(1);
    }
}

.card:hover {
    animation: pulse 1s infinite; /* Hover 시 펄스 효과 */
}

```


JavaScript 없이도 많은 효과를 간단하게 구현할 수 있습니다. 위의 예시들을 참고하여 다양한 hover 효과와 상태 변화를 적용할 수 있습니다. CSS를 통해 성능을 향상시키고 사용자 경험을 개선할 수 있습니다.









tooltip css
이렇게 기본 설정 

```css
:root {  
    --tooltip-text: ''; /* 기본 툴팁 텍스트 설정 */}
```

```css
/* Tooltip 가상 요소 스타일 */
th:after {
    content: var(--tooltip-text, ''); /* 툴팁의 내용 설정 */
    visibility: hidden; /* 초기 상태는 숨김 */
    width: 120px; /* 너비 설정 */
    background-color: black; /* 배경색 */
    color: #fff; /* 글자색 */
    text-align: center; /* 가운데 정렬 */
    padding: 8px; /* 패딩 */
    position: absolute; /* 절대 위치 설정 */
    z-index: 9999; /* z-index 설정 */
    bottom: 125%; /* 툴팁의 위치를 조정하여 th 바로 위에 위치 */
    left: 50%; /* 가운데 정렬 */
    transform: translateX(-50%); /* 가운데 정렬 보정 */
    opacity: 0; /* 초기 불투명도 */
    transition: opacity 0.2s; /* 부드러운 전환 효과 */
    border-radius: var(--p-border-radius-100); /* 둥근 모서리 설정 */
    pointer-events: none; /* 마우스 이벤트 무시 */
}

/* Tooltip의 "꼬리" 생성 */
th:before {
    content: ''; /* 삼각형 모양 생성 */
    position: absolute; /* 절대 위치 설정 */
    bottom: 100%; /* 툴팁 위에 위치 */
    left: 50%; /* 가운데 정렬 */
    margin-left: -5px; /* 삼각형을 가운데 정렬하기 위한 보정 */
    border-width: 5px; /* 삼각형 크기 조정 */
    border-style: solid; /* 경계 스타일 */
    border-color: black transparent transparent transparent; /* 삼각형 색상 설정: 위쪽으로 향하도록 설정 */
    z-index: 9999; /* z-index 설정 */
    visibility: hidden; /* 초기 상태는 숨김 */
    opacity: 0; /* 초기 불투명도 */
    transition: opacity 0.2s; /* 부드러운 전환 효과 */
}

/* Hover 상태에서 툴팁과 삼각형을 보이게 설정 */
th:hover:after,
th:hover:before
{
    visibility: visible; /* 보이게 설정 */
    opacity: 1; /* 불투명도 1로 설정 */
}

/* th 요소에 대한 상대 위치 설정 */
th {
    position: relative; /* 상대 위치 설정 */
}
```



SVG 태그에 `disabled` 속성을 직접 넣어도 브라우저는 이를 인식하지 않습니다. `disabled`는 `<button>`, `<input>` 같은 특정 HTML 요소에서만 동작하는 속성입니다.

```css
svg.disabled {
    pointer-events: none; /* 클릭 이벤트 비활성화 */
    opacity: 0.5; /* 흐리게 표시 */
    cursor: not-allowed; /* 마우스 커서 변경 */
}
```

이런식으로 직접 주거나  위에처럼  css로 줘야함
```jsp
<svg xmlns="http://www.w3.org/2000/svg" width="10" height="15" viewBox="0 0 10 15" fill="none" onclick="changePage(${categoryPageInfo.hasNextPage ?   categoryPageInfo.nextPage   : "" },'category')" style="${categoryPageInfo.hasNextPage ? '' : 'pointer-events: none; opacity: 0.5; cursor: not-allowed;'}">
```
