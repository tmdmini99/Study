
```javaScript
$.ajax({
  url: "/some-url/api", // 요청 URL
  type: "GET", // HTTP 메서드
  dataType: "json", // 응답으로 예상되는 데이터 타입
  success: function(data) {
    // 요청 성공 시 실행될 함수
    console.log("Response:", data);
  },
  error: function(xhr, status, error) {
    // 요청 실패 시 실행될 함수
    console.error("Request failed:", error);
  }
});

```


## JSON 데이터 가져오기

```javaScript
$.getJSON("/some-url/api", function(data) {
  console.log("Response:", data);
});

```


## requestAjax

```javaScript
//일반적인 jquery ajax
function requestAjax(url, params, callback) {
    $.ajax({
        url: url,
        type: "POST",
        contentType: "application/json",
        data: params,
        success: callback,
        error: function(xhr, status, error) {
            console.error("Request failed:", error);
        }
    });
}

// 사용 예시 좀더 간단히 사용한 ajax
var params = {
    key1: "value1",
    key2: "value2"
};

requestAjax("/route/01130cud.bms", JSON.stringify(params), function(data) {
    console.log("Response:", data);
});

```