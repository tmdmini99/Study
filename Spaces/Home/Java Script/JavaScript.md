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