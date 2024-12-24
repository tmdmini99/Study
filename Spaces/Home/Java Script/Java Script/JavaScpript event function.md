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
