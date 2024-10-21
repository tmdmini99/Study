
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
});

```




