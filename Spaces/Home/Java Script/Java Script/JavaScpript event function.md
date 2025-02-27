버튼 hover 밑 클릭 이벤트로 지우기

```javaScript
 $(function() {  
  
  
    // isAscending 상태 변수를 전역에서 초기화  
    let isAscending = true; // 초기 상태: 오름차순  
  
  
    // 클릭 이벤트  
$('.orderChange').click(function() {  
    const icon = $(this).find('svg path'); // 현재 클릭한 버튼의 SVG 아이콘 선택  
    let isAscending = $(this).data('isAscending'); // 현재 버튼의 정렬 상태 가져오기  
  
    // 다른 버튼 클릭 시 모든 버튼의 상태 초기화  
    $('.orderChange').not(this).each(function() {  
        $(this).data('isAscending', true); // 다른 버튼은 오름차순으로 초기화  
        const otherIcon = $(this).find('svg path');  
  
        // 오름차순 아이콘으로 설정  
        otherIcon.eq(0).attr('fill-opacity', '1'); // 오름차순 화살표를 투명하게  
        otherIcon.eq(1).attr('fill-opacity', '0.33');  // 내림차순 화살표를 불투명하게  
        $(this).find('svg').hide(); // SVG 숨기기  
        $(this).find('svg').removeClass('show'); // 다른 버튼의 show 클래스 제거  
    });  
  
    // 첫 클릭은 무조건 내림차순으로 설정  
    if (isAscending === undefined) {  
        isAscending = true; // 처음 클릭 시 isAscending 값을 초기화  
    }  
  
    if (isAscending) {  
        // 내림차순 아이콘으로 설정  
        icon.eq(0).attr('fill-opacity', '0.33'); // 오름차순 화살표를 투명하게  
        icon.eq(1).attr('fill-opacity', '1'); // 내림차순 화살표를 불투명하게  
    } else {  
        // 오름차순 아이콘으로 설정  
        icon.eq(0).attr('fill-opacity', '1'); // 오름차순 화살표를 불투명하게  
        icon.eq(1).attr('fill-opacity', '0.33'); // 내림차순 화살표를 투명하게  
    }  
  
    // 클릭했을 때 SVG 보이도록 설정  
    $(this).find('svg').show().addClass('show'); // 클릭한 SVG에 show 클래스 추가 및 보이게 함  
  
    // 정렬 상태 토글  
    $(this).data('isAscending', !isAscending); // 정렬 상태 토글  
});  
  
// hover 이벤트  
$('.orderChange').hover(  
    function() {  
        const icon = $(this).find('svg path'); // 현재 버튼의 SVG 아이콘 선택  
  
        // 현재 클릭한 버튼이 아닌 경우에만 내림차순 아이콘으로 설정  
        if (!$(this).find('svg').hasClass('show')) {  
            icon.eq(0).attr('fill-opacity', '0.33');// 내림차순 화살표를 불투명하게  
            icon.eq(1).attr('fill-opacity', '1');  // 오름차순 화살표를 투명하게  
        }  
  
        $(this).addClass('show'); // hover 시 show 클래스 추가  
        $(this).find('svg').show(); // SVG 요소 보이기  
    },  
    function() {  
        $(this).removeClass('show'); // hover가 벗어날 때 show 클래스 제거  
        $(this).find('svg').not('.show').hide(); // show 클래스가 없는 SVG 숨기기  
    }  
);
  
  
  
  
  
});
```



input event
```js
$(document).on('input', '.Polaris-TextField__Input', function() {  
    const searchTerm = $(this).val(); // 입력값 가져오기  
    console.log('입력된 값:', searchTerm); // 입력값 출력  
  
    // 현재 주소를 가져옴  
    const url = new URL(window.location.href);  
  
    // 모든 쿼리 파라미터 제거  
    url.search = ''; // 쿼리 스트링을 빈 문자열로 설정하여 모든 파라미터 제거  
  
    // 추가 작업 수행  
    if (searchTerm) {  
        fetch(url.toString() + `?search=` + searchTerm, {  
            method: 'GET'  
        })  
        .then(response => response.text()) // 텍스트로 응답을 가져오기  
        .then(data => {  
            const searchResults = $('.Polaris-IndexTable'); // 클래스로 선택  
            searchResults.empty(); // 기존 결과 지우기  
  
            // HTML 내용으로 업데이트  
            let newDocument = $.parseHTML(data); // jQuery를 사용하여 HTML 파싱  
            let newContent = $(newDocument).find('.Polaris-IndexTable'); // 같은 클래스로 업데이트할 부분 선택  
  
            if (newContent.length > 0) { // newContent가 존재할 경우  
                searchResults.html(newContent.html()); // 기존 content만 업데이트  
            } else {  
                // newContent가 없을 경우 처리  
                console.warn('Polaris-IndexTable 클래스를 가진 요소를 찾을 수 없습니다. 서버 응답:', data);  
                searchResults.html('<div>검색 결과가 없습니다.</div>');  
            }  
        })  
        .catch(error => {  
            console.error('검색 요청 실패:', error);  
            const searchResults = $('.Polaris-IndexTable'); // 클래스로 선택  
            searchResults.html('<div>검색 중 오류가 발생했습니다.</div>');  
        });  
    }  
});
```


input에서 마지막 사용자가 입력 끝낸 후 3초후 동작
```js
let abortController;  
let debounceTimeout;  
let requestCount = 0;  
  
$(document).on('input', '.Polaris-TextField__Input', function() {  
    clearTimeout(debounceTimeout);  
    debounceTimeout = setTimeout(async () => {  
        // 이전 요청이 진행 중이면 취소  
        if (abortController) {  
            abortController.abort(); // 이전 요청 취소  
            console.log('Previous request aborted');  
        }  
  
        // 새로운 AbortController 생성  
        abortController = new AbortController();  
        const signal = abortController.signal;  
        abortController.requestId = $(this).val(); // 현재 입력값을 요청 ID로 사용  
  
        const searchTerm = $(this).val(); // 입력값 가져오기  
        console.log('입력된 값:', searchTerm);  
  
        const url = new URL(window.location.href);  
        if (searchTerm) {  
            url.searchParams.set('search', searchTerm);  
        } else {  
            url.searchParams.delete('search');  
        }  
  
        // pageNumber 파라미터 삭제  
        url.searchParams.delete('pageNumber');  
        requestCount++; // 요청 카운트 증가  
        showLoading();  
        updateLoadingBar(0);  
        isRequestActive = true; // 요청 활성 상태 설정  
  
        try {  
            const response = await fetch(url.toString(), {  
                method: 'GET',  
                signal: signal // signal 추가  
            });  
  
            if (response.ok) {  
                const data = await response.text();  
                const searchResults = $('.Polaris-IndexTable');  
                searchResults.empty(); // 기존 결과 지우기  
  
                // HTML 내용으로 업데이트  
                let newDocument = $.parseHTML(data);  
                let newContent = $(newDocument).find('.Polaris-IndexTable');  
  
                if (newContent.length > 0) {  
                    searchResults.html(newContent.html()); // 결과 업데이트  
                } else if ($(newDocument).find('._ResourceListItemsWrapper_qvkap_26').length > 0) {  
                    let resourceListContent = $(newDocument).find('._ResourceListItemsWrapper_qvkap_26');  
                    searchResults.html(resourceListContent.html()); // 업데이트  
                } else {  
                    console.warn('No results found in the response.');  
                    searchResults.html('<div>검색 결과가 없습니다.</div>');  
                }  
  
                // URL 업데이트  
                history.pushState(null, '', url.toString());  
            }  
        } catch (error) {  
            if (error.name === 'AbortError') {  
                console.log('요청이 취소되었습니다.'); // 요청이 취소된 경우  
            } else {  
                console.error('검색 요청 실패:', error);  
                const searchResults = $('.Polaris-IndexTable');  
                searchResults.html('<div>검색 중 오류가 발생했습니다.</div>');  
            }  
        } finally {  
            requestCount--; // 요청 카운트 감소  
            if (requestCount === 0) {  
                updateLoadingBar(100); // 로딩 바 업데이트  
                hideLoading(); // 로딩 바 숨김  
            }  
        }  
    }, 300); // 300ms 후에 요청  
});
```



### keyup 과 input 이벤트의 차이
- `keyup` 이벤트는 사용자가 키를 누르고 뗄 때 발생하므로, 입력 필드에서 실시간으로 변경된 값을 감지하는 데 유용합니다.
- `input` 이벤트는 입력값이 변경될 때마다 발생하지만, 어떤 이유로 `keyup` 이벤트를 사용하고 싶으신 경우에 적합합니다.



```js
$(document).on('click', '.Polaris-IndexFilters__ButtonWrap button:contains("취소")', function() {}
```

text 내용 가지고  이벤트 만들기



```js
// 디바운스 함수  
function debounce(func, wait) {  
    let timeout;  
    return function (...args) {  
        const context = this;  
        clearTimeout(timeout);  
        timeout = setTimeout(() => func.apply(context, args), wait);  
    };  
}  
  
$(document).on('input', '.Polaris-TextField__Input', debounce(function() {  
    const searchTerm = $(this).val(); // 입력값 가져오기  
    console.log('입력된 값:', searchTerm); // 입력값 출력  
  
    // 현재 주소를 가져옴  
    const url = new URL(window.location.href);  
  
    // searchTerm이 비어 있으면 search 파라미터 삭제  
    if (searchTerm) {  
        url.searchParams.set('search', searchTerm); // 검색어가 있을 경우  
    } else {  
        url.searchParams.delete('search'); // 검색어가 없을 경우 파라미터 삭제  
    }  
  
    // AJAX 요청  
    fetch(url.toString(), {  
        method: 'GET'  
    })  
    .then(response => response.text()) // 텍스트로 응답을 가져오기  
    .then(data => {  
        const searchResults = $('.Polaris-IndexTable'); // 클래스로 선택  
        searchResults.empty(); // 기존 결과 지우기  
  
        // HTML 내용으로 업데이트  
        let newDocument = $.parseHTML(data); // jQuery를 사용하여 HTML 파싱  
        let newContent = $(newDocument).find('.Polaris-IndexTable'); // Polaris-IndexTable 클래스 선택  
  
        if (newContent.length > 0) { // Polaris-IndexTable이 존재할 경우  
            searchResults.html(newContent.html()); // 기존 content만 업데이트  
        } else if ($(newDocument).find('._ResourceListItemsWrapper_qvkap_26').length > 0) { // _ResourceListItemsWrapper_qvkap_26 클래스 확인  
            let resourceListContent = $(newDocument).find('._ResourceListItemsWrapper_qvkap_26');  
            searchResults.html(resourceListContent.html()); // _ResourceListItemsWrapper_qvkap_26 내용으로 업데이트  
        } else {  
            // 둘 다 없을 경우 처리  
            console.warn('Polaris-IndexTable 또는 _ResourceListItemsWrapper_qvkap_26 클래스를 가진 요소를 찾을 수 없습니다. 서버 응답:', data);  
            searchResults.html('<div>검색 결과가 없습니다.</div>');  
        }  
  
        // URL 업데이트  
        history.pushState(null, '', url.toString()); // URL도 업데이트  
    })  
    .catch(error => {  
        console.error('검색 요청 실패:', error);  
        const searchResults = $('.Polaris-IndexTable'); // 클래스로 선택  
        searchResults.html('<div>검색 중 오류가 발생했습니다.</div>');  
    });  
}, 300)); // 300ms 후에 요청을 보내도록 설정
```


**디바운스 함수**: 입력 이벤트가 발생할 때마다 타이머를 설정하여 사용자가 입력을 멈춘 후 300ms 후에 AJAX 요청을 보내도록 설정합니다. 이로 인해 입력 중에 불필요한 요청이 발생하지 않게 됩니다.





## 새로 고침 이벤트

ex
```js
window.addEventListener('beforeunload', function (event) {
    if (performance.navigation.type === performance.navigation.TYPE_RELOAD) {
        console.log('페이지가 새로고침되었습니다.');
        // 여기서 새로고침 시 수행할 동작을 추가
    }
    
    // 기본적으로 사용자에게 새로고침 경고 메시지를 띄우고 싶다면 아래와 같이 처리
    event.preventDefault(); // 일부 브라우저에서 필요
    event.returnValue = ''; // 빈 문자열을 반환하여 경고 창 표시
});

```


tr 선택 tr:not
```js



$(document).on('click', 'tbody:not(.uncommon) tr.inventory', function(event) {
	alert(1);
})



$(document).on('click', 'tbody:not(.uncommon) tr:not(.inventory)', function(event) { 
	alert(2);
})
```


`onkeyup`는 HTML에서 사용되는 이벤트 속성으로, **키보드 키를 눌렀다가 뗐을 때** 발생하는 이벤트를 처리합니다.

---

### **`onkeyup`의 동작**

- 사용자가 키보드를 눌렀다가 손을 떼는 순간 이벤트가 발생합니다.
- 예를 들어, 사용자가 `"A"` 키를 누르고 손을 떼면 `onkeyup` 이벤트가 트리거됩니다.

`onkeyup` 이벤트는 **각 키를 눌렀다가 뗄 때마다 개별적으로 발생**합니다.  
즉, 사용자가 `"ab"`를 입력하면 다음과 같은 순서로 이벤트가 발생합니다:

---

### **동작 순서**

1. **`a` 키를 눌렀다가 뗌** → `onkeyup` 이벤트가 발생 (첫 번째 이벤트).
2. **`b` 키를 눌렀다가 뗌** → `onkeyup` 이벤트가 발생 (두 번째 이벤트).


### **사용법**

#### **1. HTML에서 사용**


```html
<input type="text" onkeyup="myFunction()">
```


- 사용자가 입력 필드에 키를 입력하고 손을 떼면 `myFunction()` 함수가 실행됩니다.

#### **2. JavaScript에서 사용**

```js
const inputField = document.getElementById("myInput");
inputField.onkeyup = function() {
    console.log("Key released!");
};
```


- 사용자가 해당 입력 필드에서 키를 눌렀다 떼면 콘솔에 `"Key released!"`가 출력됩니다.

#### **3. Event Listener로 사용**

```js
const inputField = document.getElementById("myInput");
inputField.addEventListener("keyup", function(event) {
    console.log(`Key released: ${event.key}`);
});
```

사용자가 키를 떼면 어떤 키가 눌렸는지 확인할 수 있습니다.

### **`onkeyup` 이벤트의 주요 특징**

- **Key 정보 확인**: 이벤트 객체를 통해 어떤 키가 눌렸는지 확인할 수 있습니다


```js
document.addEventListener("keyup", function(event) {
    console.log(`You pressed: ${event.key}`);
});
```

**입력 값 처리**: 입력 필드에서 사용자가 입력한 값을 실시간으로 처리할 수 있습니다


```js
const input = document.getElementById("myInput");
input.addEventListener("keyup", function() {
    console.log(input.value); // 사용자가 입력한 값을 출력
});
```


### **실제 사용 사례**

1. **검색창 자동 완성**

```js
function filterSearch() {
    const query = document.getElementById("searchInput").value;
    console.log(`Searching for: ${query}`);
}
```

```jsp
<input type="text" id="searchInput" onkeyup="filterSearch()">
```

폼 검증
```js
document.getElementById("emailInput").addEventListener("keyup", function() {
    const email = this.value;
    if (email.includes("@")) {
        console.log("Valid email");
    } else {
        console.log("Invalid email");
    }
});
```

실시간 데이터 업데이트
```js
const inputField = document.getElementById("nameInput");
const display = document.getElementById("displayName");

inputField.addEventListener("keyup", function() {
    display.textContent = inputField.value;
});
```

```jsp
<input type="text" id="nameInput" placeholder="Type your name">
<p>Your name is: <span id="displayName"></span></p>
```


자동 검색
```js
function filterDropdown(type) {  
    const input = document.getElementById(`${type}-input`);  
    console.log(input);  
    const filter = input.value.replace(/\s+/g, '').toLowerCase();    
    const dropdown = document.getElementById(`${type}-dropdown`);  
    const items = dropdown.getElementsByClassName("dropdown-item");  
  
    let visibleItems = false;    
    for (let i = 0; i < items.length; i++) {  
        const item = items[i];  
        const text = item.getElementsByClassName("category-item")[0].textContent.replace(/\s+/g, '').toLowerCase();  
          
        if (text.indexOf(filter) > -1) {  
            item.style.display = "";    
            visibleItems = true;    
        } else {  
            item.style.display = "none";   
        }  
    }  
  
    dropdown.style.display = visibleItems ? "block" : "none";  
}
```


이미지 미리보기

```js
function handleImageUpload(event) {  
    const file = event.target.files[0];  
    if (file) {  
        const reader = new FileReader();  
        reader.onload = function (e) {  
            const previewImg = document.getElementById('preview-img');  
            const emptyText = document.querySelector('.select-img-contain .empty');  
            if (previewImg) {  
                previewImg.src = e.target.result;  
                previewImg.classList.remove('hidden');  
            }  
            if (emptyText) {  
                emptyText.remove();  
            }  
        };  
        reader.readAsDataURL(file);  
    }  
}  
  
window.handleImageUpload = handleImageUpload;
```

```jsp
<div class="select-img-contain" onclick="document.getElementById('file-input').click();">  
    <div class="empty">이미지 선택</div>  
    <img id="preview-img" class="hidden" />  
    <input type="file" id="fileId" name="fileId">  
    <input id="file-input" type="file" accept="image/*" onchange="handleImageUpload(event)" hidden>  
</div>
```


```jsp
<div class="select-img-contain">
    <p class="empty">이미지를 업로드하세요.</p>
    <img id="preview-img" class="hidden" alt="미리보기" />
</div>
<input type="file" onchange="handleImageUpload(event)" />
```

### **코드 동작 설명**

1. **`function handleImageUpload(event)`**
    
    - 파일 업로드 이벤트를 처리하기 위한 함수입니다.
    - `event.target.files[0]`는 업로드된 파일 중 첫 번째 파일(단일 파일)을 가져옵니다.
2. **`if (file)`**
    
    - 파일이 정상적으로 선택되었는지 확인합니다.
    - 파일이 없으면 함수가 종료됩니다.
3. **`const reader = new FileReader();`**
    
    - `FileReader` 객체를 생성합니다.
    - `FileReader`는 파일(예: 이미지, 텍스트 등)을 읽고 브라우저에서 이를 처리할 수 있는 데이터 URL로 변환하는 데 사용됩니다.
4. **`reader.onload = function (e)`**
    
    - 파일이 성공적으로 읽혔을 때 실행되는 콜백 함수입니다.
    - `e.target.result`는 읽어들인 파일의 데이터 URL(이미지의 base64 인코딩 값)을 나타냅니다.
5. **`const previewImg = document.getElementById('preview-img');`**
    
    - HTML에서 `id="preview-img"`인 이미지를 찾습니다.
    - 이 이미지 태그는 업로드된 이미지를 미리보기로 보여줄 역할을 합니다.
6. **`const emptyText = document.querySelector('.select-img-contain .empty');`**
    
    - `.select-img-contain` 내부의 `.empty` 클래스를 가진 요소를 찾습니다.
    - 업로드 전에 표시되던 빈 상태 텍스트를 제거하기 위한 요소입니다.
7. **이미지 업데이트 및 상태 변경**
    
    - **`previewImg.src = e.target.result;`**
        - 읽어들인 이미지 데이터를 미리보기 이미지의 `src` 속성에 설정합니다.
    - **`previewImg.classList.remove('hidden');`**
        - 미리보기 이미지를 화면에 보이게 하기 위해 `hidden` 클래스를 제거합니다.
    - **`emptyText.remove();`**
        - 업로드 전에 표시되던 빈 상태 텍스트를 제거합니다.
8. **`reader.readAsDataURL(file);`**
    
    - 선택된 파일을 데이터 URL로 읽습니다.
    - 이 데이터 URL은 브라우저가 이미지 데이터를 렌더링할 수 있도록 합니다.
9. **`window.handleImageUpload = handleImageUpload;`**
    
    - 이 함수는 전역 범위에서 접근 가능하도록 `window` 객체에 할당됩니다.
    - HTML에서 인라인으로 `onchange="handleImageUpload(event)"` 같은 방식으로 호출할 수 있습니다.



## oninput


```jsp
<input type="text" oninput="handleInput()" />
<script>
  function handleInput() {
    console.log("Input changed");
  }
</script>
```


- **`oninput="functionName()"`** 형태로 **`oninput`**을 사용하여 이벤트가 발생할 때마다 해당 함수를 실행할 수 있습니다.
- **`oninput`**은 키보드 입력뿐만 아니라 **붙여넣기, 드래그&드롭** 등 다양한 방식으로 값을 변경할 때 발생합니다.


```js
window.addEventListener('beforeunload', (event) => {  
    const isPaginationClicked = document.activeElement.closest('.pagination-btn, .page-number');  
    const isMenuClicked = document.activeElement && document.activeElement.tagName === 'A';  
    alert(isMenuClicked);  
    if (isMenuClicked && !isPaginationClicked) {  
        localStorage.clear();  
    }  
});
```


### **`beforeunload`의 한계:**

- `beforeunload`는 **페이지를 떠나는 순간** 발생.
- **`event.target`이 null**이거나 **정확히 클릭 요소를 감지하지 못함.**
- **`document.activeElement`**를 사용해야만 클릭된 요소를 감지할 수 있음.


- **`document.activeElement.tagName === 'A'`:**
    - 현재 포커스된 요소가 `a` 태그인지 확인.
- **`alert` 추가:**
    - **클릭 감지 여부**를 `alert`로 시각적으로 확인.
- **`isPaginationClicked`:**
    - **페이지네이션 클릭 감지** 예외 처리.


### **해당 코드의 역할:**

```js
const isMenuClicked = document.activeElement && document.activeElement.tagName === 'A';

```


### **1. `document.activeElement`란?**

- **현재 포커스(클릭)된 요소**를 나타냅니다.
- **HTML 문서에서 사용자가 클릭하거나 포커스한 요소를 참조**합니다.

```js
console.log(document.activeElement); 
// 사용자가 클릭한 요소 출력
```

### **`document.activeElement.tagName === 'A'`란?**

- **포커스된 요소가 `a` 태그인지 확인**합니다.
- **`tagName`은 항상 대문자로 반환**됩니다.
- 따라서 **'A'**와 정확히 비교해야 합니다.

```js
if (document.activeElement.tagName === 'A') {
    console.log("현재 클릭한 요소는 <a> 태그입니다!");
}
```


### **`document.activeElement &&`란?**

- **`document.activeElement`가 존재하는지 먼저 검사**합니다.
- 예를 들어 **`<body>`나 `null`**이 될 수도 있기 때문입니다.
- **안전한 코딩**을 위해 **`&&` 연산자**를 사용하여 **`null` 체크**를 합니다.



## category Depth 



```js
document.querySelectorAll('.add-category').forEach(button => {  
    button.addEventListener('click', function(event) {  
        event.stopPropagation();  
  
        const subItem = this.closest('.sub-item');  
        const group = subItem.querySelector('.group');  
  
        this.style.display = 'none';  
  
        group.classList.add('show-edit-group');  
  
        enableCategoryNavigation(subItem);  
    });  
});  
document.querySelectorAll('.sub-item').forEach(item => {  
    item.addEventListener('click', function(event) {  
        // 하위 카테고리 수정 버튼, 추가 버튼 클릭 시 처리 안 함  
        if (event.target.closest('.sub-category-edit') || event.target.closest('.add-category')) {  
            return;  
        }  
  
        if (event.target.closest('.sub-category-save') || event.target.closest('.sub-category-cancel')) {  
            return;  
        }  
  
        if (event.target.closest('input')) {  
            return;  
        }  
  
        const addCategoryButton = this.querySelector('.add-category');  
        const group = this.querySelector('.group');  
        const categoryInput = document.getElementById('category-input');  
  
        // 'add-category' 버튼이 있고 'show-edit-group' 클래스가 없으면 category-input에 값 설정  
        if (addCategoryButton && !group.classList.contains('show-edit-group')) {  
            const input = this.querySelector('input');  
            categoryInput.value = input.value; // 해당 행의 input 값 설정  
        }  
  
        // 'add-category' 버튼이 없거나, 'show-edit-group' 클래스가 있을 경우에는 이동  
        if (!addCategoryButton || this.classList.contains('can-move-to-next-depth') || group.classList.contains('show-edit-group')) {  
            const currentDepth = this.closest('.categories');  
            const currentDepthNumber = parseInt(currentDepth.getAttribute('data-category-depth'), 10);  
            const nextDepth = document.querySelector(`.categories[data-category-depth="${currentDepthNumber + 1}"]`);  
  
            document.querySelectorAll('.categories').forEach(depth => {  
                depth.classList.remove('current-depth');  
            });  
            nextDepth.classList.add('current-depth');  
  
            const categoryValue = this.querySelector('input').value;  
            const referenceItems = nextDepth.querySelectorAll('.items');  
            referenceItems.forEach(referenceItem => {  
                const referenceValue = referenceItem.getAttribute('data-reference');  
                if (categoryValue === referenceValue) {  
                    referenceItem.style.display = 'block';  
                } else {  
                    referenceItem.style.display = 'none';  
                }  
            });  
  
            // 경로 업데이트  
            updateNavi(categoryValue);  
            resetButtonStates();  
        }  
    });  
});  
  
// 카테고리 이동 활성화  
function enableCategoryNavigation(subItem) {  
    subItem.classList.add('can-move-to-next-depth');  
}  
  
document.querySelectorAll('.sub-category-edit').forEach(button => {  
    button.addEventListener('click', function(event) {  
        event.stopPropagation();  
  
        const subItem = this.closest('.sub-item');  
        const input = subItem.querySelector('input');  
  
        input.removeAttribute('readonly');  
        subItem.querySelector('.sub-category-save').style.display = 'block';  
        subItem.querySelector('.sub-category-cancel').style.display = 'block';  
        subItem.querySelector('.sub-category-edit').style.display = 'none';  
        subItem.querySelector('.sub-category-delete').style.display = 'none';  
    });  
});  
  
document.querySelectorAll('.sub-category-save').forEach(button => {  
    button.addEventListener('click', function(event) {  
        event.stopPropagation();  
  
        const subItem = this.closest('.sub-item');  
        const input = subItem.querySelector('input');  
  
        const updatedValue = input.value;  
  
        updateCategoryValues(subItem, updatedValue);  
  
        changeButtonStates(subItem);  
  
        console.log("Updated data-category-value:", updatedValue);  
    });  
});  
  
document.querySelectorAll('.sub-category-delete').forEach(button => {  
    button.addEventListener('click', function(event) {  
        event.stopPropagation();  
  
        const subItem = this.closest('.sub-item');  
  
        subItem.remove();  
  
    });  
});  
function updateCategoryValues(subItem, updatedValue) {  
    subItem.setAttribute('data-category-value', updatedValue);  
    const referenceItems = document.querySelectorAll(`[data-reference="${subItem.getAttribute('data-category-value')}"]`);  
  
    setTimeout(() => {  
        referenceItems.forEach(item => {  
            item.setAttribute('data-reference', updatedValue);  
        });  
    }, 100);  
}  
  
function changeButtonStates(subItem) {  
    const input = subItem.querySelector('input');  
  
    input.setAttribute('readonly', 'true');  
  
    subItem.querySelector('.sub-category-save').style.display = 'none';  
    subItem.querySelector('.sub-category-cancel').style.display = 'none';  
    subItem.querySelector('.sub-category-edit').style.display = 'block';  
    subItem.querySelector('.sub-category-delete').style.display = 'block';  
}  
  
document.querySelectorAll('.sub-category-cancel').forEach(button => {  
    button.addEventListener('click', function(event) {  
        event.stopPropagation();  
  
        const subItem = this.closest('.sub-item');  
        const input = subItem.querySelector('input');  
        const initialValue = subItem.getAttribute('data-category-value'); // 기본값으로 돌아가야 하는 값  
  
        // input 값 되돌리기  
        input.value = initialValue;  
  
        input.setAttribute('readonly', 'true');  
        subItem.querySelector('.sub-category-save').style.display = 'none';  
        subItem.querySelector('.sub-category-cancel').style.display = 'none';  
        subItem.querySelector('.sub-category-edit').style.display = 'block';  
        subItem.querySelector('.sub-category-delete').style.display = 'block';  
    });  
});  
  
function updateNavi(categoryValue, currentDepth) {  
    const navi = document.querySelector('.navi');  
  
    if (!navi) {  
        return;  
    }  
  
    const allPaths = navi.querySelectorAll('.path-group');  
  
    const existingPath = Array.from(allPaths).find(path => path.querySelector('p').textContent === categoryValue);  
  
    if (existingPath) {  
        return;  
    }  
    allPaths.forEach(path => {  
        path.classList.remove('current-path');  
    });  
  
    const pathGroup = document.createElement('div');  
    pathGroup.classList.add('path-group');  
  
    const pathName = document.createElement('p');  
    pathName.textContent = categoryValue;  
    pathGroup.appendChild(pathName);  
  
    const pathIcon = document.createElement('img');  
    pathIcon.src = '/images/icons/product manage/navi-right-arrow.svg';  
    pathGroup.appendChild(pathIcon);  
  
    pathGroup.classList.add('current-path');  
  
    navi.appendChild(pathGroup);  
  
    if (currentDepth === 1) {  
        navi.style.display = 'none';  
    } else {  
        navi.style.display = 'flex';  
    }  
  
}  
function goBack() {  
    const navi = document.querySelector('.navi');  
    const categories = document.querySelectorAll('.categories');  
    const currentDepthElement = document.querySelector('.categories.current-depth');  
    const allPaths = navi.querySelectorAll('.path-group');  
  
    if (!navi || categories.length === 0 || !currentDepthElement) return;  
  
    // 현재 활성화된 카테고리 뎁스를 찾기  
    const currentDepth = Array.from(categories).findIndex(cat => cat === currentDepthElement) + 1;  
  
    console.log(currentDepth)  
  
    if (currentDepth === 2) navi.style.display = "none";  
  
    if (currentDepth > 1) {  
        categories[currentDepth - 1].classList.remove('active');  
        categories[currentDepth - 1].classList.add('hidden');  
        categories[currentDepth - 2].classList.remove('hidden');  
        categories[currentDepth - 2].classList.add('active');  
  
        categories[currentDepth - 2].classList.add('current-depth');  
        currentDepthElement.classList.remove('current-depth');  
  
        if (allPaths.length > 0) {  
            const lastPath = allPaths[allPaths.length - 1];  
            navi.removeChild(lastPath);  
        }  
    }  
}  
  
// 버튼 상태 리셋 함수  
function resetButtonStates() {  
    document.querySelectorAll('.sub-item').forEach(subItem => {  
        const input = subItem.querySelector('input');  
        input.setAttribute('readonly', 'true');  
  
        subItem.querySelector('.sub-category-save').style.display = 'none';  
        subItem.querySelector('.sub-category-cancel').style.display = 'none';  
        subItem.querySelector('.sub-category-edit').style.display = 'block';  
        subItem.querySelector('.sub-category-delete').style.display = 'block';  
    });  
}  
  
document.getElementById('category-input').addEventListener('input', function(event) {  
    const searchQuery = event.target.value.toLowerCase();  
    const subItems = document.querySelectorAll('.sub-item')  
  
    subItems.forEach(item => {  
        if (item.closest('.categories').getAttribute('data-category-depth') === "1") {  
            const itemText = item.querySelector('input').value.toLowerCase();  
  
            if (itemText.includes(searchQuery)) {  
                item.style.display = 'flex';  
            } else {  
                item.style.display = 'none';  
            }  
        }  
    });  
});  
document.getElementById('category-input').addEventListener('click', function(event) {  
    const dropdown = document.getElementById('category-dropdown');  
    dropdown.style.display = 'block';  
    event.stopPropagation();  
});  
  
document.addEventListener('click', function(event) {  
    const dropdown = document.getElementById('category-dropdown');  
    const categoryInput = document.getElementById('category-input');  
  
    if (!dropdown.contains(event.target) && event.target !== categoryInput) {  
        dropdown.style.display = 'none';  
    }  
});
```




depth +1로 무한정 depth 생성 가능
```js
document.querySelectorAll('.sub-item').forEach(item => {  
    item.addEventListener('click', function(event) {  
        // 하위 카테고리 수정 버튼, 추가 버튼 클릭 시 처리 안 함  
        if (event.target.closest('.sub-category-edit') || event.target.closest('.add-category')) {  
            return;  
        }  
  
        if (event.target.closest('.sub-category-save') || event.target.closest('.sub-category-cancel')) {  
            return;  
        }  
  
        if (event.target.closest('input')) {  
            return;  
        }  
  
        const addCategoryButton = this.querySelector('.add-category');  
        const group = this.querySelector('.group');  
        const categoryInput = document.getElementById('category-input');  
  
        // 'add-category' 버튼이 있고 'show-edit-group' 클래스가 없으면 category-input에 값 설정  
        if (addCategoryButton && !group.classList.contains('show-edit-group')) {  
            const hiddenInput = this.querySelector('input[type="hidden"]');  
            const textInput = this.querySelector('input[type="text"]');  
            categoryInput.value = textInput.value; // hidden input 값 설정  
            document.getElementById('category-id').value = hiddenInput.value;  
        }  
  
        // 'add-category' 버튼이 없거나, 'show-edit-group' 클래스가 있을 경우에는 이동  
        if (!addCategoryButton || this.classList.contains('can-move-to-next-depth') || group.classList.contains('show-edit-group')) {  
            const currentDepth = this.closest('.categories');  
            const currentDepthNumber = parseInt(currentDepth.getAttribute('data-category-depth'), 10);  
            let nextDepth = document.querySelector(`.categories[data-category-depth="${currentDepthNumber + 1}"]`);  
            let nextDepthNumber = currentDepthNumber + 1;  
            if (!nextDepth) {  
            const newDepth = document.createElement('div');  
            newDepth.classList.add('categories');  
            newDepth.setAttribute('data-category-depth', nextDepthNumber);  
            newDepth.innerHTML = `  
                <ul class="items" data-reference="">  
                    <li class="sub-item" data-category-value="">  
                        <input type="text" value="" readonly>  
                        <input type="hidden" value="" readonly>  
                        <p class="add-category">하위 카테고리 추가</p>  
                        <div class="group">  
                            <div class="sub-category-edit"><img src="/resources/images/icons/product manage/category-edit.svg"></div>  
                            <div class="sub-category-delete"><img src="/resources/images/icons/product manage/category-del.svg"></div>  
                            <div class="sub-category-save"><img src="/resources/images/icons/product manage/category-edit-save.svg"></div>  
                            <div class="sub-category-cancel"><img src="/resources/images/icons/product manage/category-edit-cancel.svg"></div>  
                        </div>  
                    </li>  
                </ul>  
            `;  
            currentDepth.parentNode.appendChild(newDepth);  
            nextDepth = newDepth;  
        }  
  
            document.querySelectorAll('.categories').forEach(depth => {  
                depth.classList.remove('current-depth');  
            });  
            nextDepth.classList.add('current-depth');  
  
            const hiddenInputValue = this.querySelector('input[type="hidden"]').value;  
            const categoryValue = this.querySelector('input').value;  
            const referenceItems = nextDepth.querySelectorAll('.items');  
  
            referenceItems.forEach(referenceItem => {  
                const referenceValue = referenceItem.getAttribute('data-reference');  
                if (hiddenInputValue === referenceValue) {  
                    referenceItem.style.display = 'block';  
                } else {  
                    referenceItem.style.display = 'none';  
                }  
            });  
  
  
  
            // 경로 업데이트 및 상태 초기화  
            updateNavi(categoryValue,null, hiddenInputValue);  
            resetButtonStates();  
        }  
    });  
});
```


커서 위치 저장

```js
$(document).on("keyup change", "input:text[numberhyphenonly]", function (event) {
    let input = $(this);
    let value = input.val();
    let caretPosition = input[0].selectionStart;  // ✅ 커서 위치 저장

    // 숫자와 하이픈 외 모든 문자 제거
    value = value.replace(/[^0-9\-]/gi, "");
    
    // ✅ 기존 커서 위치를 고려하여 업데이트
    input.val(value);
    input[0].setSelectionRange(caretPosition, caretPosition);  // ✅ 커서 위치 복원
});
```


커서 위치 지정

```js
const inputElement = document.querySelector("input");
inputElement.setSelectionRange(3, 3); // 커서를 3번째 글자 위치로 이동
```



- **`selectionStart`:**
    - **텍스트 선택의 시작 위치** (커서 위치).
- **`selectionEnd`:**
    - **텍스트 선택의 끝 위치.**
- **`setSelectionRange(start, end)` 메서드:**
    - **커서 위치를 직접 설정.**


## blur


`input`에 포커스가 빠질 때(`blur` 이벤트 발생 시) `input` 값을 유지하거나 사라지게 할 수 있습니다.

### ✅ JavaScript: `blur` 이벤트로 값 사라지게 하기

```js
// 인풋에서 포커스 아웃(다른 곳 클릭) 시 값이 사라지게 처리
document.getElementById('artist-input').addEventListener('blur', function (e) {
    const input = e.target;
    const hiddenInput = document.getElementById('artist-input-id');
    const dropdown = document.getElementById('artist-dropdown');

    // dropdown이 열려있는 경우에는 값 유지
    if (dropdown.style.display === 'none' || dropdown.style.display === '') {
        input.value = ''; // 값 제거
        hiddenInput.value = ''; // hidden input 값도 제거
    }
});
```


이벤트로 클릭시에는 값이 안사라지고 클릭하지 않고 넘어가거나 클릭후 수정시 값 사라지게

```js
function selectItem(type, itemName, itemId) {  
    const input = document.getElementById(`${type}-input`);  
    const inputId = document.getElementById(`${type}-input-id`);  
    console.log(input)  
    input.value = itemName;  
    inputId.value = itemId;  
    const dropdown = document.getElementById(`${type}-dropdown`);  
    dropdown.style.display = "none";  
    input.setAttribute('data-selected', 'true');  
}
```
여기서  밑에 플래그로 확인

```js
input.setAttribute('data-selected', 'true');
```


```js
function initDropdown(inputSelector, dropdownSelector, hiddenInputSelector) {  
    const inputField = document.querySelector(inputSelector);  
    const dropdown = document.querySelector(dropdownSelector);  
    const hiddenInput = document.querySelector(hiddenInputSelector);  
  
    inputField.addEventListener('input', function () {  
        dropdown.style.display = 'block';  
        inputField.setAttribute('data-selected', 'false');  // 직접 입력 시 해제  
        hiddenInput.value = '';  
    });  
  
    document.addEventListener('click', function (event) {  
        const isSelected = inputField.getAttribute('data-selected') === 'true';  
        if (!inputField.contains(event.target) && !dropdown.contains(event.target)) {  
            if (!isSelected) {  
                inputField.value = '';       // 값 삭제  
                hiddenInput.value = '';      // hidden input 값 삭제  
            }  
            dropdown.style.display = 'none';  
        }  
    });  
  
    inputField.addEventListener('blur', function () {  
        const isSelected = inputField.getAttribute('data-selected') === 'true';  
        if (!isSelected) {  
            inputField.value = '';  
            hiddenInput.value = '';  
        }  
    });  
}  
  
document.addEventListener('DOMContentLoaded', function () {  
    initDropdown('#artist-input', '#artist-dropdown', '#artist-input-id');  
    initDropdown('#media-input', '#media-dropdown', '#media-input-id');  
    initDropdown('#codes-input', '#codes-dropdown', '#codes-input-id');  
    initDropdown('#category-input', '#category-dropdown', '#category-input-id');  
});
```


태그 두개를 같이 검색해서 둘중 하나라도 있으면 표출
```js
function filterDropdown(type) {  
    const input = document.getElementById(`${type}-input`);  
    const filter = input.value.replace(/\s+/g, '').toLowerCase();  
    const dropdown = document.getElementById(`${type}-dropdown`);  
    const items = dropdown.getElementsByClassName("dropdown-item");  
  
    let visibleItems = false;  
    for (let i = 0; i < items.length; i++) {  
        const item = items[i];  
        const categoryText = item.getElementsByClassName("category-item")[0].textContent.replace(/\s+/g, '').toLowerCase();  
  
  
        const cidElement = item.getElementsByClassName("cid")[0];  
        const cidText = cidElement ? cidElement.textContent.replace(/\s+/g, '').toLowerCase() : '';  
  
        if (categoryText.indexOf(filter) > -1 || cidText.indexOf(filter) > -1) {  
            item.style.display = "";  
            visibleItems = true;  
        } else {  
            item.style.display = "none";  
        }  
    }  
  
    dropdown.style.display = visibleItems ? "block" : "none";  
}
```



`+`  `-` 버튼 클릭시 적용 되게 그리고 수동입력시에도 가능하게

```js
function setupStockControl(container) {  
    const minusButton = container.querySelector("#minus");  
    const plusButton = container.querySelector("#plus");  
    const countElement = container.querySelector("#cnt input[name='productCnt']");  
  
    if (!minusButton || !plusButton || !countElement) {  
        console.error("Missing required elements for stock control.");  
        return;  
    }  
  
    let count = parseInt(countElement.value, 10);  
      
    minusButton.addEventListener("click", () => {  
        if (count > 0) {  
            count--;  
            countElement.value = count;  
        }  
    });  
  
  
    plusButton.addEventListener("click", () => {  
        count++;  
        countElement.value = count;  
    });  
      
    countElement.addEventListener("input", () => {  
        let newValue = parseInt(countElement.value, 10);  
        count = isNaN(newValue) ? 0 : newValue;  
    });  
}
```




국문 영문 input data 맞추기

```js
function selectTag(type, tagName, checkbox, id) {  
    const existingTags = document.getElementById('tag-list').getElementsByClassName('tag');  
    let tagFound = false;  
  
    for (let i = 0; i < existingTags.length; i++) {  
        if (existingTags[i].textContent.trim() === `#${tagName}`) {  
            tagFound = true;  
            break;  
        }  
    }  
  
    if (checkbox.checked && !tagFound) {  
        const tagList = document.getElementById('tag-list');  
        const tagDiv = document.createElement('div');  
        tagDiv.classList.add('tag');  
        tagDiv.innerHTML = `  
        <div class="tag-box">  
            <div class="tag-name">#${tagName}</div>  
            <div class="tag-del" onclick="removeTag(this, '${type}-hidden-${id}')">  
                <svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">  
                    <path d="M1.64145 0.281628C1.26594 -0.093876 0.657132 -0.093876 0.281628 0.281628C-0.093876 0.657132 -0.093876 1.26594 0.281628 1.64145L3.64018 5L0.281628 8.35855C-0.093876 8.73406 -0.093876 9.34287 0.281628 9.71837C0.657132 10.0939 1.26594 10.0939 1.64145 9.71837L5 6.35982L8.35855 9.71837C8.73406 10.0939 9.34287 10.0939 9.71837 9.71837C10.0939 9.34287 10.0939 8.73406 9.71837 8.35855L6.35982 5L9.71837 1.64145C10.0939 1.26594 10.0939 0.657132 9.71837 0.281628C9.34287 -0.093876 8.73406 -0.093876 8.35855 0.281628L5 3.64018L1.64145 0.281628Z" fill="#006638"/>  
                </svg>  
            </div>  
            <input type="hidden" id="${type}-hidden-${id}" name="${type}Id" value="${id}">  
        </div>  
        `;  
        tagList.appendChild(tagDiv);  
  
        syncEnglishTag(type, id); // 영어 태그 동기화  
    } else if (!checkbox.checked && tagFound) {  
        removeTag(type, id, false);  
    }  
  
    document.getElementById(`${type}-dropdown`).style.display = "none";  
}  
  
function selectTagEn(type, tagName, checkbox, id) {  
    const existingTags = document.getElementById('tag-list-en').getElementsByClassName('tag');  
    let tagFound = false;  
  
    for (let i = 0; i < existingTags.length; i++) {  
        if (existingTags[i].textContent.trim() === `#${tagName}`) {  
            tagFound = true;  
            break;  
        }  
    }  
  
    if (checkbox.checked && !tagFound) {  
        const tagList = document.getElementById('tag-list-en');  
        const tagDiv = document.createElement('div');  
        tagDiv.classList.add('tag');  
        tagDiv.innerHTML = `  
        <div class="tag-box">  
            <div class="tag-name">#${tagName}</div>  
            <div class="tag-del" onclick="removeTag(this, '${type}-hidden-${id}-en')">  
                <svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">  
                    <path d="M1.64145 0.281628C1.26594 -0.093876 0.657132 -0.093876 0.281628 0.281628C-0.093876 0.657132 -0.093876 1.26594 0.281628 1.64145L3.64018 5L0.281628 8.35855C-0.093876 8.73406 -0.093876 9.34287 0.281628 9.71837C0.657132 10.0939 1.26594 10.0939 1.64145 9.71837L5 6.35982L8.35855 9.71837C8.73406 10.0939 9.34287 10.0939 9.71837 9.71837C10.0939 9.34287 10.0939 8.73406 9.71837 8.35855L6.35982 5L9.71837 1.64145C10.0939 1.26594 10.0939 0.657132 9.71837 0.281628C9.34287 -0.093876 8.73406 -0.093876 8.35855 0.281628L5 3.64018L1.64145 0.281628Z" fill="#006638"/>  
                </svg>  
            </div>  
            <input type="hidden" id="${type}-hidden-${id}-en" name="${type}IdEn" value="${id}">  
        </div>  
        `;  
        tagList.appendChild(tagDiv);  
        console.log(event.target.closest('li').getAttribute('data-id'));  
        syncKoreanTag(type, event.target.closest('li').getAttribute('data-id'));  
        // syncKoreanTag(type, id); // 한국어 태그 동기화  
    } else if (!checkbox.checked && tagFound) {  
        removeTagEn(type, id, true);  
    }  
  
    document.getElementById(`${type}-dropdown-en`).style.display = "none";  
}  
  
function syncKoreanTag(type, id) {  
    const koreanItem = document.querySelector(`#tag-dropdown .dropdown-item[data-id="${id}"]`);  
    const checkbox = koreanItem ? koreanItem.querySelector('input') : null;  
    if (checkbox && !checkbox.checked) {  
        checkbox.checked = true;  
        selectTag(type, checkbox.dataset.tag, checkbox, id);  
    }  
}  
  
function syncEnglishTag(type, id) {  
    const englishItem = document.querySelector(`#tag-dropdown-en .dropdown-item[data-id="${id}"]`);  
    const checkbox = englishItem ? englishItem.querySelector('input') : null;  
    if (checkbox && !checkbox.checked) {  
        checkbox.checked = true;  
        selectTagEn(type, checkbox.dataset.tag, checkbox, id);  
    }  
}
```



```js
function selectItem(type, itemName, itemId, langId) {  
    const input = document.getElementById(`${type}-input`);  
    const inputId = document.getElementById(`${type}-input-id`);  
    console.log(input)  
    input.value = itemName;  
    inputId.value = itemId;  
    const dropdown = document.getElementById(`${type}-dropdown`);  
    dropdown.style.display = "none";  
    input.setAttribute('data-selected', 'true');  
    syncEnglishField(type, itemId,langId);  
}  
function selectItemEn(type, itemName, itemId, langId) {  
    const input = document.getElementById(`${type}-input-en`);  
    const inputId = document.getElementById(`${type}-input-id-en`);  
    console.log(input)  
    input.value = itemName;  
    inputId.value = itemId;  
    const dropdown = document.getElementById(`${type}-dropdown-en`);  
    dropdown.style.display = "none";  
    input.setAttribute('data-selected', 'true');  
    // `event` 객체를 활용해 클릭된 요소의 `data-id` 가져오기  
    const clickedItemId = event.target.closest('.dropdown-item').getAttribute('data-id');  
    console.log(`Clicked item data-id: ${clickedItemId}`);  
  
    // 해당 data-id로 한국어 필드 동기화  
    syncKoreanField(type, clickedItemId,langId);  
}  
  
function syncEnglishField(type, itemId, langId) {  
    const englishDropdown = document.getElementById(`${type}-dropdown-en`);  
    const englishItems = englishDropdown.querySelectorAll('.dropdown-item');  
    englishItems.forEach(item => {  
        const itemHiddenId = item.getAttribute('data-id');  
        if (itemHiddenId === itemId) {  
            const itemNameEn = item.querySelector('.category-item').innerText;  
            console.log(itemNameEn);  
            const inputEn = document.getElementById(`${type}-input-en`);  
            const inputIdEn = document.getElementById(`${type}-input-id-en`);  
            inputEn.value = itemNameEn;  
            inputIdEn.value = langId;  
            inputEn.setAttribute('data-selected', 'true');  
        }  
    });  
}  
  
function syncKoreanField(type, itemId, langId) {  
    console.log(`syncKoreanField called with type: ${type}, itemId: ${itemId}`); // 디버깅용  
    const koreanDropdown = document.getElementById(`${type}-dropdown`);  
    const koreanItems = koreanDropdown.querySelectorAll('.dropdown-item');  
  
    koreanItems.forEach(item => {  
        const itemHiddenId = item.getAttribute('data-id');  
        console.log(`Checking item with data-id: ${itemHiddenId}`); // 디버깅용  
        if (itemHiddenId === itemId) {  
            const itemNameKr = item.querySelector('.category-item').innerText;  
            const inputKr = document.getElementById(`${type}-input`);  
            const inputIdKr = document.getElementById(`${type}-input-id`);  
            inputKr.value = itemNameKr;  
            inputIdKr.value = langId;  
            inputKr.setAttribute('data-selected', 'true');  
            console.log(`Updated Korean field: ${itemNameKr}, ${itemId}`); // 디버깅용  
        }  
    });  
}
```



입력시 동적 검사 후 보이기

```js
document.addEventListener("DOMContentLoaded", () => {  
    const input = document.getElementById("tag-search-input");  
    const tagList = document.querySelector(".tag-list ul");  
    const tagBox = document.querySelector(".tag-box");  
  
    if (!tagList || !tagBox) {  
        console.error("Tag list or tag box elements are missing.");  
        return;  
    }  
  
    const MAX_TAGS = 6;  
    let tags = [];  
    let debounceTimeout;  
  
    //  태그 표시 함수  
    function displayTags() {  
        tagBox.innerHTML = '';  
  
        const visibleTags = tags.slice(0, MAX_TAGS);  
        visibleTags.forEach(tag => {  
            const tagElement = document.createElement("div");  
            tagElement.classList.add("box", "tag-box");  
            tagElement.innerHTML = `  
                <div class="tag">#${tag}  
                    <input type="hidden" name="tagId" value="${tag}">  
                    <div class="tag-del">  
                        <img src="/resources/images/icons/cancelTag.svg">  
                    </div>  
                </div>  
            `;  
            tagBox.appendChild(tagElement);  
        });  
  
        const remainingTags = tags.length - MAX_TAGS;  
        if (remainingTags > 0) {  
            const plusTag = document.createElement("div");  
            plusTag.classList.add("tag", "plus-tag");  
            plusTag.textContent = `+${remainingTags}`;  
            tagBox.appendChild(plusTag);  
        }  
  
        updateCheckboxState(); //  체크박스 상태 유지  
    }  
  
    //  태그 추가 함수  
    function addTag(tagText) {  
        if (!tags.includes(tagText)) {  
            tags.push(tagText);  
            displayTags();  
        }  
    }  
  
    //  태그 삭제 함수  
    function removeTag(tagText) {  
        tags = tags.filter(tag => tag !== tagText);  
        displayTags();  
  
        //  태그 삭제 시 체크박스도 해제  
        document.querySelectorAll(".tag-list .tag-checkbox").forEach(checkbox => {  
            if (checkbox.dataset.tag === tagText) {  
                checkbox.checked = false;  
            }  
        });  
    }  
  
    //  동적으로 추가된 태그 삭제 이벤트 (이벤트 위임)  
    tagBox.addEventListener("click", function (event) {  
        if (event.target.closest(".tag-del")) {  
            const tagElement = event.target.closest(".box.tag-box");  
            const tagText = tagElement.querySelector(".tag").textContent.trim().substring(1);  
            removeTag(tagText);  
        }  
    });  
  
    //  태그 검색 (AJAX)    function handleTagSearchAjax(value) {  
        clearTimeout(debounceTimeout);  
        debounceTimeout = setTimeout(() => {  
            if (!value.trim()) {  
                document.querySelector(".tag-list").style.display = "none";  
                return;  
            }  
  
            let url = window.location.pathname + "ModalSearch";  
            let data = {  
                sc_MODAL: "tags",  
                sc_SEARCH: value  
            };  
  
            $.ajax({  
                url: url,  
                type: "GET",  
                data: data,  
                success: function (response) {  
                    tagList.innerHTML = '';  
                    if (response.length > 0) {  
                        response.forEach(item => {  
                            const tagName = item.tag_nm;  
                            const listItem = document.createElement("li");  
                            listItem.innerHTML = `  
                                <label class="item">  
                                    <input type="checkbox" class="tag-checkbox" data-tag="${tagName}">  
                                    <p class="font-black">${tagName}</p>  
                                </label>  
                            `;  
                            tagList.appendChild(listItem);  
                        });  
                        document.querySelector(".tag-list").style.display = "block";  
                        updateCheckboxState(); //  추가된 태그 리스트에서도 체크박스 상태 유지  
                    } else {  
                        document.querySelector(".tag-list").style.display = "none";  
                    }  
                },  
                error: function () {  
                    console.error("태그 검색 오류 발생");  
                }  
            });  
        }, 300);  
    }  
  
    //  태그 검색 입력 이벤트  
    input.addEventListener("input", function () {  
        handleTagSearchAjax(this.value);  
    });  
  
    //  태그 클릭 시 추가 및 제거 (체크박스 클릭 이벤트)  
    tagList.addEventListener("change", function (event) {  
        if (event.target.classList.contains("tag-checkbox")) {  
            const tagText = event.target.closest("label").querySelector(".font-black").textContent.trim();  
            if (event.target.checked) {  
                addTag(tagText);  
            } else {  
                removeTag(tagText);  
            }  
        }  
    });  
  
    //  태그 리스트가 다시 열릴 때, 기존 태그들이 체크 상태 유지  
    function updateCheckboxState() {  
        document.querySelectorAll(".tag-list .tag-checkbox").forEach(checkbox => {  
            const tagText = checkbox.closest("label").querySelector(".font-black").textContent.trim();  
            checkbox.checked = tags.includes(tagText);  
        });  
    }  
  
    //  외부 클릭 시 태그 리스트 숨김  
    document.addEventListener("click", function (event) {  
        if (!input.contains(event.target) && !document.querySelector(".tag-list").contains(event.target)) {  
            document.querySelector(".tag-list").style.display = "none";  
        }  
    });  
  
    //  체크박스 클릭 시 자동으로 태그 리스트 닫기  
    tagList.addEventListener("click", function (event) {  
        if (event.target.classList.contains("tag-checkbox")) {  
            setTimeout(() => {  
                document.querySelector(".tag-list").style.display = "none";  
                input.value = '';  
            }, 200);  
        }  
    });  
});
```



## blur와 alert은 같이 사용할수 없음


### **`blur` 이벤트가 여러 번 실행되는 문제 해결**

👉 **AJAX 요청이 완료되기 전에 `blur` 이벤트가 다시 발생하여 여러 번 실행됨**  
👉 **`alert()`이 실행되면 입력 필드가 포커스를 잃었다가 다시 얻으며 `blur`가 반복 호출됨**  
👉 **`focus()` 실행 후 `blur`가 다시 발생하면서 AJAX가 중첩 호출됨**

blur 대신 change 사용
```js
$("input[data-unique]").on("blur", function () {  
    if (isProcessing) return;  
    const $input = $(this);  
    let value = $input.val().trim();  
    let lang; // 언어 값 설정  
  
    if ($input.hasClass("product-en")) {  
       lang = "EN";  
    }  
  
    let dataToSend = { sc_LANG: lang };  
  
    //  `product-en`과 `product-kr` 클래스가 없는 경우 → `sc_SELL_NO` 사용  
    if (!$input.hasClass("product-en") && !$input.hasClass("product-kr")) {  
       let sellCodeValue = $("input[name='sellcode']").val()?.trim() || ""; // 셀코드 값  
       let codeInputValue = $("#code-input").val()?.trim() || ""; // 코드 입력값  
  
       // ✅ 입력 필드가 `code-input`이면 `sellcode`를 추가  
       if ($input.attr("id") === "code-input") {  
          value = sellCodeValue + value;  
       }  
       // ✅ 입력 필드가 `sellcode`이면 `code-input`을 추가  
       else if ($input.attr("name") === "sellcode") {  
          value = value + codeInputValue;  
       }  
  
       console.log("sc_SELL_NO 값:", value); // 디버깅용 콘솔 출력  
       dataToSend["sc_SELL_NO"] = value; // ✅ `sc_SELL_NO`로 전송  
    } else {  
       dataToSend["sc_PRODUCT_NM"] = value; //  기본 `sc_PRODUCT_NM`으로 전송  
    }  
  
    //  값이 비어 있으면 중복 검사 안 하고 초기화  
    if (value === "") {  
       resetValidation($input);  
       $input.attr("data-unique", "true");  
       return;  
    }  
    isProcessing = true;  
    //  AJAX 중복 검사 실행  
    $.ajax({  
       url: window.location.pathname + "Check",  
       type: "POST",  
       data: dataToSend, //  동적으로 `sc_PRODUCT_NM` 또는 `sc_SELL_NO` 전송  
       success: function (response) {  
          if (response) {  
  
             showValidationError($input, "중복된 값입니다.");  
             $input.attr("data-unique", "false"); // 중복됨  
  
  
  
             //  `display: none;` 상태라면 탭 전환 후 focus()             if ($input.css("display") === "none") {  
                switchTabAndFocus($input);  
             } else {  
                // ✅ `alert()` 대신 `setTimeout()`으로 포커스 유지  
                setTimeout(() => {  
                   alert("중복된 값입니다.");  
                   $input.focus();  
                }, 100);  
             }  
  
  
          } else {  
             resetValidation($input);  
             $input.attr("data-unique", "true"); // 사용 가능  
          }  
       },  
       error: function () {  
          console.error("AJAX 요청 실패");  
       },  
       complete: function () {  
          isProcessing = false; // ✅ AJAX 요청 완료 후 플래그 초기화  
       }  
    });  
});
```


```js
$("input[data-unique]").on("change", function () {  
    if (isProcessing) return;  
    const $input = $(this);  
    let value = $input.val().trim();  
    let lang; // 언어 값 설정  
  
    if ($input.hasClass("product-en")) {  
       lang = "EN";  
    }  
  
    let dataToSend = { sc_LANG: lang };  
  
    //  `product-en`과 `product-kr` 클래스가 없는 경우 → `sc_SELL_NO` 사용  
    if (!$input.hasClass("product-en") && !$input.hasClass("product-kr")) {  
       let sellCodeValue = $("input[name='sellcode']").val()?.trim() || ""; // 셀코드 값  
       let codeInputValue = $("#code-input").val()?.trim() || ""; // 코드 입력값  
  
       // ✅ 입력 필드가 `code-input`이면 `sellcode`를 추가  
       if ($input.attr("id") === "code-input") {  
          value = sellCodeValue + value;  
       }  
       // ✅ 입력 필드가 `sellcode`이면 `code-input`을 추가  
       else if ($input.attr("name") === "sellcode") {  
          value = value + codeInputValue;  
       }  
  
       console.log("sc_SELL_NO 값:", value); // 디버깅용 콘솔 출력  
       dataToSend["sc_SELL_NO"] = value; // ✅ `sc_SELL_NO`로 전송  
    } else {  
       dataToSend["sc_PRODUCT_NM"] = value; //  기본 `sc_PRODUCT_NM`으로 전송  
    }  
  
    //  값이 비어 있으면 중복 검사 안 하고 초기화  
    if (value === "") {  
       resetValidation($input);  
       $input.attr("data-unique", "true");  
       return;  
    }  
    isProcessing = true;  
    //  AJAX 중복 검사 실행  
    $.ajax({  
       url: window.location.pathname + "Check",  
       type: "POST",  
       data: dataToSend, //  동적으로 `sc_PRODUCT_NM` 또는 `sc_SELL_NO` 전송  
       success: function (response) {  
          if (response) {  
  
             showValidationError($input, "중복된 값입니다.");  
             $input.attr("data-unique", "false"); // 중복됨  
  
  
  
             //  `display: none;` 상태라면 탭 전환 후 focus()             if ($input.css("display") === "none") {  
                switchTabAndFocus($input);  
             } else {  
                // ✅ `alert()` 대신 `setTimeout()`으로 포커스 유지  
                setTimeout(() => {  
                   alert("중복된 값입니다.");  
                   $input.focus();  
                }, 100);  
             }  
  
  
          } else {  
             resetValidation($input);  
             $input.attr("data-unique", "true"); // 사용 가능  
          }  
       },  
       error: function () {  
          console.error("AJAX 요청 실패");  
       },  
       complete: function () {  
          isProcessing = false; // ✅ AJAX 요청 완료 후 플래그 초기화  
       }  
    });  
});
```