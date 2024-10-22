


```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Data View</title>
</head>
<body>
    <h1>Data List</h1>
    <div id="dataContainer" th:if="${data != null}">
        <ul>
            <li th:each="item : ${data}" th:text="${item}"></li>
        </ul>
    </div>
    
    <!-- 새로 고침 없이 데이터를 로드하는 버튼 -->
    <button id="loadDataButton">Load Data</button>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#loadDataButton').click(function() {
                $.get('/your/api/endpoint', function(data) {
                    $('#dataContainer').html(data); // 데이터를 업데이트
                });
            });
        });
    </script>
</body>
</html>

```



1. **Controller**: `/your/api/endpoint`에 GET 요청이 들어오면 `loadData` 메서드가 호출되어 `yourService`를 통해 데이터를 가져오고, `yourView`를 반환합니다.
2. **Service**: `getData()` 메서드는 실제 데이터베이스에서 데이터를 가져오는 로직을 포함할 수 있습니다.
3. **Thymeleaf 템플릿**: `yourView.html`에서는 `data` 모델 속성을 사용하여 리스트 형태로 데이터를 표시합니다. 또한, "Load Data" 버튼을 클릭하면 Ajax 요청을 통해 데이터를 동기적으로 가져오고, 결과를 업데이트합니다.

### 5. **실행 결과**

- 페이지를 처음 로드할 때 데이터가 표시됩니다.
- "Load Data" 버튼을 클릭하면, Ajax 요청이 `/your/api/endpoint`로 전송되고, 서버는 데이터를 다시 가져와 `dataContainer`를 업데이트합니다. 이 과정에서 페이지 전체가 새로 고쳐지지 않으며, 사용자는 부드럽게 데이터가 업데이트되는 것을 경험합니다.

이와 같은 방식으로 스프링 MVC와 Thymeleaf를 결합하여 동기 작업을 수행하면서도 사용자에게 비동기적으로 작동하는 듯한 경험을 제공할 수 있습니다!



