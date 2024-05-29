
```
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

```
$.getJSON("/some-url/api", function(data) {
  console.log("Response:", data);
});

```