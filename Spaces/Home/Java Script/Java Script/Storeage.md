
### 로컬 스토리지 vs. 세션 스토리지

- **로컬 스토리지 (`localStorage`)**: 브라우저가 종료되더라도 데이터가 유지됩니다. 즉, 페이지를 새로고침하더라도 값이 사라지지 않습니다.
- **세션 스토리지 (`sessionStorage`)**: 데이터는 탭이나 브라우저를 닫을 때 사라집니다. 즉, 동일한 세션 내에서만 데이터가 유지됩니다.


아래는 `localStorage`를 사용하여 변수를 저장하고 불러오는 방법입니다.
```js
// 변수의 값을 로컬 스토리지에 저장
function saveSearchEvent(value) {
    localStorage.setItem('searchEvent', value);
}

// 로컬 스토리지에서 변수의 값을 불러오기
function loadSearchEvent() {
    const value = localStorage.getItem('searchEvent');
    return value === 'true'; // 문자열로 저장되기 때문에 boolean으로 변환
}

// 페이지 로드 시 로컬 스토리지에서 값 불러오기
$(document).ready(function () {
    searchEvent = loadSearchEvent(); // 로컬 스토리지에서 값 불러옴
    console.log('searchEvent:', searchEvent);

    // 추가 작업...
});

// 페이지를 새로 고침할 때 변수를 저장하는 예시
$(window).on('beforeunload', function () {
    // 새로 고침 시 searchEvent 값을 로컬 스토리지에 저장
    saveSearchEvent(searchEvent);
});

```


### 사용 방법

1. **변수 저장**: `saveSearchEvent` 함수를 사용하여 변수를 `localStorage`에 저장합니다.
2. **변수 불러오기**: 페이지가 로드될 때 `loadSearchEvent` 함수를 사용하여 저장된 값을 불러옵니다.
3. **새로 고침 시 저장**: `beforeunload` 이벤트에서 변수를 로컬 스토리지에 저장합니다.


### 세션 스토리지 사용 예시

세션 스토리지를 사용하려면 위의 코드에서 `localStorage` 대신 `sessionStorage`를 사용하면 됩니다. 예를 들어:


```js
// 변수의 값을 세션 스토리지에 저장
function saveSearchEvent(value) {
    sessionStorage.setItem('searchEvent', value);
}

// 세션 스토리지에서 변수의 값을 불러오기
function loadSearchEvent() {
    const value = sessionStorage.getItem('searchEvent');
    return value === 'true'; // 문자열로 저장되기 때문에 boolean으로 변환
}

// 페이지 로드 시 세션 스토리지에서 값 불러오기
$(document).ready(function () {
    searchEvent = loadSearchEvent(); // 세션 스토리지에서 값 불러옴
    console.log('searchEvent:', searchEvent);

    // 추가 작업...
});

// 페이지를 새로 고침할 때 변수를 저장하는 예시
$(window).on('beforeunload', function () {
    // 새로 고침 시 searchEvent 값을 세션 스토리지에 저장
    saveSearchEvent(searchEvent);
    // 세션 스토리지값 삭제
    sessionStorage.removeItem('searchEvent');
});

```




### 1. **DOM 요소의 상태를 문자열로 저장**

`localStorage`에 저장할 수 있는 값은 문자열만 가능하므로, **DOM 요소의 상태**(예: `class`, `id`, `data-` 속성 등)를 **문자열**로 변환하여 저장합니다


```js
$(document).ready(function () {  
    AOS.init();  
})  
document.addEventListener("DOMContentLoaded", function () {  
    const rows = document.querySelectorAll("tbody.main-tbody tr");  
  
    function updateTotalPrice() {  
        let totalPrice = 0;   
  
        rows.forEach(row => {  
            const priceElement = row.querySelector(".price input");   
            const countElement = row.querySelector(".cnt-item input");   
  
            if (priceElement && countElement) {  
                const priceValue = priceElement.value.replace(/[^0-9]/g, '');   
                const countValue = countElement.value.replace(/[^0-9]/g, '');   
  
                const price = parseInt(priceValue, 10) || 0; // NaN일 경우 0 처리  
  
                totalPrice += price;   
            }  
        });  
  
        const totalElement = document.querySelector("#total p");  
        if (totalElement) {  
            totalElement.textContent = `상품 합계: ${totalPrice.toLocaleString()}원`;  
        }  
    }  
  
    function setupStockControl(container) {  
        const minusButton = container.querySelector(".minus");  
        const plusButton = container.querySelector(".add");  
        const countElement = container.querySelector(".cnt-item input");  
        const priceElement = container.closest("tr").querySelector(".price input");    
        const albumTitleElement = container.closest("tr").querySelector(".album-title p");   
  
        if (!albumTitleElement) {  
            console.error("Album title not found!");  
            return;  
        }  
  
        const albumName = albumTitleElement.textContent.trim();  // 앨범 이름  
        let discountPriceElements = container.closest("tr").querySelectorAll(".discount-price");   
        let discountPrice = 0;  
  
        // discountPrice가 없으면 original 가격 사용  
        if (discountPriceElements.length > 0) {  
            discountPrice = parseInt(discountPriceElements[0].textContent.replace(/[^0-9]/g, ''), 10);   
        } else {  
            let originalPrice = container.closest("tr").querySelector(".original");    
            if (originalPrice) {  
                discountPrice = parseInt(originalPrice.textContent.replace(/[^0-9]/g, ''), 10);  
            }  
        }  
  
        if (!minusButton || !plusButton || !countElement || !priceElement || isNaN(discountPrice)) {  
            console.error("Missing required elements for stock control.");  
            return;  
        }  
  
        // 로컬스토리지에서 가져오는 값을 초기화하는 방법  
        let count = localStorage.getItem(`stock-${albumName}`)   
? parseInt(localStorage.getItem(`stock-${albumName}`), 10)  
                    : parseInt(countElement.value, 10) || 0;  
  
        // 초기 수량 값 설정  
        countElement.value = count;  
  
        // 가격 업데이트  
        priceElement.value = (count * discountPrice).toLocaleString();    
  
        minusButton.addEventListener("click", () => {  
            if (count > 0) {  
                count--;  
                countElement.value = count;  
                priceElement.value = (count * discountPrice).toLocaleString();   
                localStorage.setItem(`stock-${albumName}`, count); // 쿠키에 저장  
                updateTotalPrice();   
            }  
  
            if (count === 0) {  
                localStorage.removeItem(`stock-${albumName}`); // 수량이 0이면 쿠키에서 제거  
                updateTotalPrice();   
            }  
        });  
  
        plusButton.addEventListener("click", () => {  
            count++;  
            countElement.value = count;  
            priceElement.value = (count * discountPrice).toLocaleString();    
            localStorage.setItem(`stock-${albumName}`, count); // 쿠키에 저장  
            updateTotalPrice();    
        });  
    }  
  
    rows.forEach(row => {  
        const controlContainer = row.querySelector(".cnt-input");  
        if (controlContainer) {  
            setupStockControl(controlContainer);   
        }  
    });  
  
  
    updateTotalPrice();  
});  
  
  
// 폐반 절판 처리  
document.addEventListener("DOMContentLoaded", function () {  
    const rows = document.querySelectorAll("tbody.main-tbody tr");  
  
    rows.forEach(row => {  
        const statusElement = row.querySelector(".product-status");  
        const controlElements = row.querySelectorAll(".control > *");  
        const priceElements = row.querySelectorAll(".price > *");  
  
        if (statusElement) {  
            const status = statusElement.textContent.trim();  
  
            if (status === "정상") {  
                controlElements.forEach(element => {  
                    element.style.display = "flex";  
                });  
                priceElements.forEach(element => {  
                    element.style.display = "flex";  
                });  
            } else {  
                controlElements.forEach(element => {  
                    element.style.display = "none";  
                });  
                priceElements.forEach(element => {  
                    element.style.display = "none";  
                });  
            }  
        }  
    });  
});
```