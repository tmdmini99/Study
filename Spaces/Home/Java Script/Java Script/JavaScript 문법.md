.parent().siblings('.modal-content-body') : 부모의 가장 가까운 형제중 클래스가 '.modal-content-body'인 tag로


opener.$('\[popup-name="' + window.name + '"]').attr('popup-param').split(";");
1. opener.$('\[popup-name="' + window.name + '"]'): 팝업을 열었던 부모 창에서 현재 팝업 창의 popup-name 속성과 일치하는 요소를 찾습니다.
2. .attr('popup-param'): 해당 요소의 popup-param 속성 값을 가져옵니다.
3. .split(";"): 가져온 popup-param 값을 세미콜론(;)을 기준으로 분할하여 배열로 반환합니다.



```javaScript
// XMLHttpRequest 객체 생성
var xhr = new XMLHttpRequest();

// 요청을 초기화
xhr.open("GET", "/some-url/api", true);

// 요청이 성공했을 때 실행될 함수 설정
xhr.onload = function () {
  if (xhr.status >= 200 && xhr.status < 300) {
    // 서버 응답 처리
    console.log("Response:", xhr.responseText);
  } else {
    // 오류 처리
    console.error("Request failed with status: " + xhr.status);
  }
};

// 요청 전송
xhr.send();

```


## requestAjax

```javaScript

// 일반적인 ajax
function requestAjax(url, params, callback) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url, true);
    xhr.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
    
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
            callback(JSON.parse(xhr.responseText));
        } else if (xhr.readyState === 4) {
            console.error("Request failed with status: " + xhr.status);
        }
    };
    
    xhr.send(params);
}
--------------------------------------------
// 사용 예시 좀더 간단히 사용한 ajax
var params = {
    key1: "value1",
    key2: "value2"
};

requestAjax("/route/01130cud.bms", JSON.stringify(params), function(data) {
    console.log("Response:", data);
});

```




```javascript
$('#addRow').click(function () {
    // 기존 코드 생략...
    
    var row =
        // 새로운 행 HTML 생성...

    // 새로운 행을 데이터 테이블에 추가
    if ($('.newRow').length == 0) {
        $('#dataTable > tbody').prepend(row);
    } else {
        $('#dataTable > tbody > .newRow').last().after(row);
    }

    // 새로 추가된 행을 선택
    var selectRow = $('#dataTable > tbody > .newRow').last();

    // Datepicker 초기화
    $(selectRow).find('.datepicker').datepicker(datepickerOption);
    $('.newRow').closest('tr').css('background-color', '#FEFFD2');
    rowCount++;

    // 새로 추가된 행의 .btn_uncommon 버튼에 클릭 이벤트 바인딩
    $(selectRow).find('.btn_uncommon').click(function() {
        alert(1);
        return;
        // 기존 코드 생략...
    });
});

```

`$(selectRow).find('.btn_uncommon').click(function() {` 이벤트가 동작하는 이유는 jQuery의 이벤트 바인딩 방식 때문입니다. 조금 더 구체적으로 설명하자면, 다음과 같은 이유가 있습니다:

1. **동적으로 생성된 요소**: `$('#addRow').click(function () {` 핸들러는 사용자가 "추가" 버튼을 클릭할 때마다 새로운 행을 동적으로 생성합니다. 이 새로운 행에는 `.btn_uncommon` 클래스를 가진 버튼이 포함되어 있습니다.
    
2. **이벤트 바인딩**: 새로운 행이 생성된 후, `$(selectRow).find('.btn_uncommon').click(function() {` 코드를 통해 해당 행에 있는 `.btn_uncommon` 버튼에 클릭 이벤트가 바인딩됩니다. 이 이벤트 핸들러는 새로 생성된 버튼에만 바인딩되며, 기존 버튼에는 영향을 미치지 않습니다.
    
3. **클릭 이벤트의 작동**: 사용자가 `.btn_uncommon` 버튼을 클릭하면, 해당 버튼에 바인딩된 클릭 이벤트 핸들러가 실행됩니다. 이 핸들러는 이벤트가 발생한 요소를 기준으로 동작하므로, 동적으로 생성된 버튼에서도 정상적으로 작동합니다.



## JSTL 쓰는법
```sql
$("#sc_ROUTE_RUN_TYPE").val("${BasicParamVo.sc_ROUTE_RUN_TYPE}");
```


### **2. JavaScript의 `filter()`**

#### **용도**:

JavaScript의 `filter()`는 **배열**에서 특정 조건에 맞는 요소만 유지하는 데 사용됩니다.

#### **문법**:

```js
array.filter(callback(element, index, array), thisArg)
```


#### **인수**:

1. **callback**: 배열의 각 요소에 대해 실행되는 함수.
    - **`element`**: 현재 요소.
    - **`index`**: 요소의 인덱스.
    - **`array`**: 현재 배열.
2. **thisArg (선택)**: `callback` 함수 내부에서 사용할 `this` 값.

#### **예제**:

```js
// 1. 숫자 배열 필터링
const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(function(num) {
    return num % 2 === 0;
});
console.log(evenNumbers); // [2, 4]

// 2. DOM 요소 필터링 (배열로 변환 필요)
const elements = Array.from(document.querySelectorAll('.items'));
const activeElements = elements.filter(function(el) {
    return el.classList.contains('active');
});
console.log(activeElements); // [DOM 요소 리스트]
```


#### **특징**:

- JavaScript `filter()`는 **새 배열**을 반환합니다.
- 원본 배열은 변경되지 않습니다(불변성).


## onclick vs clickEvent

HTML 요소에 `onclick="openPopup('artistPopup')"` 속성이 직접 정의되어 있기 때문에 발생합니다. 브라우저는 `.new-add`를 클릭했을 때 **`onclick` 속성을 먼저 실행**하고, 그 후에 `$(document).on('click', '.new-add', ...)`로 바인딩된 이벤트 핸들러를 실행합니다.



마우스 클릭시 발생하는 이벤트 순서는 onmousedown -> onmouseup -> click 순이다.
참고로 onmousedown에서 alert를 띄우면 클릭해도 onmouseup, onclick 이벤트 모두 발생하지 않지만, onmouseup에서 alert를 띄우면 onclick까지 정상적으로 먹는다.




---
출처 - https://star12ap.github.io/frontend/mouseevent/