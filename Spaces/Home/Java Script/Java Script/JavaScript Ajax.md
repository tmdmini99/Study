



```js
document.addEventListener("DOMContentLoaded", function() {
    // 버튼 클릭 이벤트 리스너
    document.querySelectorAll('.load-customer-data').forEach(button => {
        button.addEventListener('click', function() {
            const customerId = this.getAttribute('data-id');

            // AJAX 요청
            $.ajax({
				url: "https://jsonplaceholder.typicode.com/posts/1", // 요청 보낼 URL
				method: "GET",  // GET 요청 방식
				success: function(response) {
					// 성공적으로 데이터를 받아온 경우
					$("#result").html(`
						<h3>${response.title}</h3>
						<p>${response.body}</p>
					`);
				},
				error: function(error) {
					// 에러 발생 시
					$("#result").html("<p>Something went wrong.</p>");
				}
			});
        });
    });
});

```

```js
$.ajax({
	url: "https://jsonplaceholder.typicode.com/posts",  // POST 요청을 보낼 URL
	method: "POST", // POST 방식
	data: JSON.stringify(postData), // 데이터를 JSON 형식으로 변환
	contentType: "application/json; charset=UTF-8", // 전송할 데이터의 형식 지정
	success: function(response) {
		// 성공적으로 데이터를 보낸 경우 응답 데이터 출력
		$("#responseResult").html(`
			<h3>Post Created Successfully</h3>
			<p><strong>ID:</strong> ${response.id}</p>
			<p><strong>Title:</strong> ${response.title}</p>
			<p><strong>Body:</strong> ${response.body}</p>
		`);
	},
	error: function(error) {
		// 에러 발생 시
		$("#responseResult").html("<p>Something went wrong.</p>");
	}
});
```




이런식으로 메소드 밑에 @ResponseBody 사용
```java
  
@Controller  
@RequestMapping("/customers/*")  
@RequiredArgsConstructor  
public class CustomersController {  
    private final CustomersService customersService;  
  
    @GetMapping("customer")  
    @ResponseBody  
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {  
  
        List<OrdersVo> list = customersService.selectList(basicParamVo);  
        model.addAttribute("list", list);  
        return "customers/customers";  
    }  
}
```


이렇게 사용시 controller 모든 메소드에 responseBody 적용

```java
@Controller
@RequestMapping("/customers/*")
@RequiredArgsConstructor
@ResponseBody
public class CustomersController {
    private final CustomersService customersService;

    @GetMapping("customer")
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {

        List<OrdersVo> list = customersService.selectList(basicParamVo);
        model.addAttribute("list", list);
        return "customers/customers";
    }
} 
```



 
## 동적으로 실행



**동적으로 페이지를 이동**하고 HTML 콘텐츠를 교체하는 경우, 특히 **AJAX**로 요청한 **JSP 페이지**의 스크립트는 자동으로 실행되지 않습니다. 이는 보안상의 이유로, 동적으로 로드된 HTML 내에 포함된 `<script>` 태그가 **자동으로 실행되지 않도록** 되어 있기 때문입니다.

### 이유:

`innerHTML`을 사용하여 HTML을 삽입하거나 `fetch` API로 HTML을 로드하는 경우, 로드된 HTML의 **JavaScript 코드**는 **자동으로 실행되지 않습니다**. 이는 **브라우저의 보안 정책**에 따른 것입니다. `<script>` 태그가 포함된 HTML을 `innerHTML`로 삽입한다고 해서 그 안의 JavaScript 코드가 실행되는 것이 아니라, **HTML 구조만 삽입**됩니다.

### 해결책:

동적으로 페이지를 변경한 후에도 해당 페이지 내의 **JSP에서 포함된 JavaScript 코드**를 실행하려면, **스크립트를 명시적으로 실행**하거나 **동적으로 `<script>` 태그를 추가**해야 합니다.

### 방법:

1. **`innerHTML`로 HTML 삽입 후 스크립트 실행**
    
    - 새로운 HTML 콘텐츠에 포함된 `<script>` 태그를 **수동으로 실행**하거나 **동적으로 추가**해야 합니다.
2. **동적으로 `<script>` 태그 삽입하여 실행**
    

다음은 동적으로 HTML 콘텐츠를 로드한 후 **스크립트**를 실행하는 방법입니다.

#### 방법 1: 동적으로 `<script>` 태그 삽입


```js
// AJAX로 HTML 콘텐츠를 받아서 페이지에 적용
fetch(url, {
    method: 'GET',
    headers: {
        'Content-Type': 'text/html'
    }
})
.then(response => response.text())
.then(response => {
    // 서버에서 받은 HTML을 DOMParser로 파싱
    let newDocument = new DOMParser().parseFromString(response, 'text/html');
    let newContent = newDocument.getElementById('content'); // 업데이트할 부분 선택

    // 기존 content만 업데이트
    if (newContent) {
        document.getElementById('content').innerHTML = newContent.innerHTML;
        
        // 새로운 HTML 내의 <script> 태그를 찾아서 실행
        let scripts = newDocument.querySelectorAll('script');
        scripts.forEach(script => {
            let newScript = document.createElement('script');
            newScript.textContent = script.textContent; // 새로운 스크립트 내용
            document.body.appendChild(newScript); // 동적으로 <script> 태그를 body에 추가하여 실행
        });
    }
    renderIcons(); // 아이콘 다시 렌더링
    history.pushState({ id: url }, '', url); // URL 업데이트
})
.catch(error => {
    console.error('AJAX 요청 실패:', error);
});

```


#### 방법 2: 외부 스크립트 파일 로드

만약 동적으로 로드한 페이지가 **외부 스크립트**를 포함하고 있다면, 그 스크립트를 동적으로 로드할 수 있습니다.



```js
let scripts = newDocument.querySelectorAll('script');
scripts.forEach(script => {
    if (script.src) {
        let scriptTag = document.createElement('script');
        scriptTag.src = script.src; // 외부 스크립트의 src 속성
        document.body.appendChild(scriptTag); // 동적으로 외부 스크립트 로드
    }
});
```


### JSP 페이지에서 JavaScript 실행:

JSP 파일 내에서 JavaScript를 사용하는 경우에도, **AJAX로 JSP 파일을 로드**하고 그 콘텐츠를 페이지에 삽입한 후에는 **해당 스크립트가 실행되지 않기** 때문에, 위와 같은 방법으로 **동적으로 `<script>` 태그를 삽입**하거나 **스크립트를 실행**해야 합니다.

### 결론:

- **AJAX**나 **`fetch`**로 JSP 페이지를 로드한 경우, **스크립트**는 자동으로 실행되지 않기 때문에 **수동으로 실행**하거나 **동적으로 `<script>` 태그를 추가**하여 스크립트를 실행해야 합니다.
- `<script>` 태그를 동적으로 추가하는 방법이나 외부 스크립트를 동적으로 로드하는 방법을 활용하면, 페이지의 스크립트가 정상적으로 실행됩니다.


- **AJAX 요청에서 기본적으로 헤더를 추가**  
    대부분의 JavaScript 라이브러리(예: jQuery)는 AJAX 요청을 보낼 때 자동으로 `X-Requested-With` 헤더를 추가합니다.
    
    - 예: `XMLHttpRequest`의 기본 동작으로 인해 자동 추가.
    - 헤더의 값은 `"XMLHttpRequest"`로 설정됩니다.
- **서버 프레임워크가 기본 설정**  
    일부 서버 프레임워크(Spring 포함)나 프록시 서버가 요청에 대해 자동으로 `X-Requested-With` 헤더를 추가할 수 있습니다.
    
    - 예: Spring MVC는 요청을 처리하기 전 필터나 인터셉터 단계에서 헤더를 추가할 수 있음.
- **테스트 시 오해**  
    개발 중에 AJAX 요청이 아닌 일반 요청으로 실행했더라도, 브라우저나 디버깅 도구에서 헤더를 추가했을 가능성.
    
- **직접 헤더 추가**  
    이미 코드에 다음과 같은 설정이 있다면:

```js
$(document).ready(function() {  
  
    // 테이블에서 행 클릭 시 fetch로 데이터 요청  
    $("table tbody tr[data-id]").on("click", function() {  
        var rowId = $(this).data("id");  
        var currentUrl = window.location.pathname;  // 예: /board/3300  
        var detailUrl = currentUrl + 'Detail';  // 예: /board/3300Detail  
  
        // fetch로 AJAX 요청을 보내는 부분  
        $.ajax({  
            url: detailUrl,  // 요청 URL            method: 'GET',  // GET 방식으로 요청  
            data: {  
                sc_id: rowId  // 파라미터로 sc_id 전송  
            },  
            headers: {  
                'X-Requested-With': 'XMLHttpRequest',  // AJAX 요청을 알리기 위한 헤더  
                'Content-Type': 'application/json'     // 요청 본문 데이터 형식을 JSON으로 설정  
            },  
            success: function(data) {  
                console.log(data);  // 서버에서 받은 데이터 확인  
  
                var tbody = $('table tbody[data-detail]');  // tbody 요소 선택  
                console.log(tbody);  // 서버에서 받은 데이터 확인  
  
                var newDocument = $.parseHTML(data);  // 응답 데이터를 HTML로 파싱  
                var newContent = $(newDocument).find('table tbody[data-detail]');  // 파싱한 HTML에서 원하는 요소 찾기  
                console.log(newContent);  
  
                tbody.html(newContent.html());  // 기존 tbody의 내용을 새로 받은 데이터로 갱신  
  
                var sc_id = $('#sc_id');  // sc_id 요소 선택  
                console.log(sc_id);  
  
                // sc_id 값 설정  
                sc_id.val($(newDocument).find('#sc_id').val());  
                var actionUrl = $('#search-form').attr('action');  
                // var form = $('<form></form>');  // 새로운 form 태그 생성  
                // form.attr('method', 'POST');  // POST 방식으로 설정  
                // form.attr('action', actionUrl);  // 적절한 action URL을 설정  
                //  
                // // sc_id를 hidden 필드로 추가  
                // var scIdInput = $('<input>').attr('type', 'hidden').attr('name', 'sc_id').val(sc_id.val());  
                // form.append(scIdInput);                // var csrf = $('<input>').attr('type', 'hidden').attr('name', $('#csrf').attr('name')).val($('#csrf').val());                // form.append(csrf);                // form.css('display', 'none');  // 폼을 숨김  
                // $('body').append(form);  
                // form.submit();  // 폼 제출  
                // form.remove(); // 폼 삭제  
                // form 데이터를 객체 형태로 준비  
                var formData = {  
                    '_csrf': $('#csrf').val(),  // CSRF 토큰 값  
                    'sc_id': sc_id.val()  // sc_id 값  
                };  
  
                // AJAX로 폼 데이터를 POST 방식으로 서버에 비동기적으로 전송  
                $.ajax({  
                    url: actionUrl,  // 폼의 action URL 사용  
                    method: 'POST',  
                    data: formData,  // 폼 데이터를 전송  
                    success: function(response) {  
                        // 서버로부터 성공적인 응답을 받았을 때 처리  
                        console.log('폼 데이터 전송 성공:', response);  
                    },  
                    error: function(xhr, status, error) {  
                        // 요청 실패 시 오류 처리  
                        console.error("AJAX error:", error);  
                    }  
                });  
            },  
            error: function(xhr, status, error) {  
                // 요청 실패 시 오류 처리  
                console.error("AJAX error:", error);  
            }  
        });  
  
});  
});
```


request.ajax 사용
```js
// request.js 파일에서 공통 AJAX 요청 처리  
var request = {  
    ajax: function(url, method, data, successCallback, errorCallback) {  
        $.ajax({  
            url: url,           // 요청할 URL            method: method,     // GET, POST, PUT, DELETE 등  
            data: data,         // 요청할 데이터  
            success: function(response) {  
                if (successCallback) {  
                    successCallback(response);  
                }  
            },  
            error: function(xhr, status, error) {  
                if (errorCallback) {  
                    errorCallback(xhr, status, error);  
                }  
            }  
        });  
    }  
};
```




```js
function changePage(pageNum) {  
    // 현재 URL에서 경로만 추출  
    var url = window.location.href + "Category";  
    // AJAX 요청  
    request.ajax(url, 'GET', { sc_PAGE_NUM: pageNum }, function(response) {  
        var categoryData = response.categoryList;  
        var categoryPageInfo = response.categoryPageInfo;  
  
        // 테이블 내용 비우기  
        $('#categoryData').empty();  
  
        // 카테고리 데이터 테이블 생성  
        categoryData.forEach(function(item, index) {  
            var $row = $('<tr>');  
            $row.append($('<td>').text(index + 1)); // 번호  
            $row.append($('<td>').text(item.krName || 'N/A')); // 국문 이름  
            $row.append($('<td>').text(item.enName || 'N/A')); // 영문 이름  
            var $actionsTd = $('<td>');  
            $actionsTd.append($('<button>').text('수정').attr('onclick', 'editCategory(' + item.id + ')'));  
            $actionsTd.append($('<button>').text('삭제').attr('onclick', 'deleteCategory(' + item.id + ')'));  
            $row.append($actionsTd);  
            $('#categoryData').append($row);  
        });  
  
        // 페이징 처리  
        var $pagination = $('.pagination');  
        $pagination.empty();  
  
        // 이전 페이지 버튼  
        var prevDisabled = categoryPageInfo.hasPreviousPage ? '' : 'disabled';  
        $pagination.append(  
            $('<li>').addClass('page-item ' + prevDisabled)  
                .append($('<a>').addClass('page-link')  
                    .attr('href', 'javascript:void(0);')  
                    .attr('onclick', 'changePage(' + categoryPageInfo.prePage + ')')  
                    .html('&laquo;')));  
  
        // 페이지 번호들  
        categoryPageInfo.navigatepageNums.forEach(function(pageNum) {  
            var activeClass = categoryPageInfo.pageNum === pageNum ? 'active' : '';  
            $pagination.append(  
                $('<li>').addClass('page-item ' + activeClass)  
                    .append($('<a>').addClass('page-link')  
                        .attr('href', 'javascript:void(0);')  
                        .attr('onclick', 'changePage(' + pageNum + ')')  
                        .text(pageNum)));  
        });  
  
        // 다음 페이지 버튼  
        var nextDisabled = categoryPageInfo.hasNextPage ? '' : 'disabled';  
        $pagination.append(  
            $('<li>').addClass('page-item ' + nextDisabled)  
                .append($('<a>').addClass('page-link')  
                    .attr('href', 'javascript:void(0);')  
                    .attr('onclick', 'changePage(' + categoryPageInfo.nextPage + ')')  
                    .html('&raquo;')));  
    }, function(xhr, status, error) {  
        console.error("Error fetching category data: ", error);  
    });  
}
```



ajax로 response 처리 위 코드 활용

```js
request.ajax(url, 'GET', { sc_PAGE_NUM: pageNum }, function(response) {  
        var categoryData = response.categoryList;  
        var categoryPageInfo = response.categoryPageInfo;  
  
        // 테이블 내용 비우기  
        $('#categoryData').empty();  
  
        // 카테고리 데이터 테이블 생성  
        categoryData.forEach(function(item, index) {  
            var $row = $('<tr>');  
            $row.append($('<td>').text(index + 1)); // 번호  
            $row.append($('<td>').text(item.krName || 'N/A')); // 국문 이름  
            $row.append($('<td>').text(item.enName || 'N/A')); // 영문 이름  
            var $actionsTd = $('<td>');  
            $actionsTd.append($('<button>').text('수정').attr('onclick', 'editCategory(' + item.id + ')'));  
            $actionsTd.append($('<button>').text('삭제').attr('onclick', 'deleteCategory(' + item.id + ')'));  
            $row.append($actionsTd);  
            $('#categoryData').append($row);  
        });  
  
        // 페이징 처리  
        var $pagination = $('.pagination');  
        $pagination.empty();  
  
        // 이전 페이지 버튼  
        var prevDisabled = categoryPageInfo.hasPreviousPage ? '' : 'disabled';  
        $pagination.append(  
            $('<li>').addClass('page-item ' + prevDisabled)  
                .append($('<a>').addClass('page-link')  
                    .attr('href', 'javascript:void(0);')  
                    .attr('onclick', 'changePage(' + categoryPageInfo.prePage + ')')  
                    .html('&laquo;')));  
  
        // 페이지 번호들  
        categoryPageInfo.navigatepageNums.forEach(function(pageNum) {  
            var activeClass = categoryPageInfo.pageNum === pageNum ? 'active' : '';  
            $pagination.append(  
                $('<li>').addClass('page-item ' + activeClass)  
                    .append($('<a>').addClass('page-link')  
                        .attr('href', 'javascript:void(0);')  
                        .attr('onclick', 'changePage(' + pageNum + ')')  
                        .text(pageNum)));  
        });  
  
        // 다음 페이지 버튼  
        var nextDisabled = categoryPageInfo.hasNextPage ? '' : 'disabled';  
        $pagination.append(  
            $('<li>').addClass('page-item ' + nextDisabled)  
                .append($('<a>').addClass('page-link')  
                    .attr('href', 'javascript:void(0);')  
                    .attr('onclick', 'changePage(' + categoryPageInfo.nextPage + ')')  
                    .html('&raquo;')));  
    }, function(xhr, status, error) {  
        console.error("Error fetching category data: ", error);  
    });  
```

여기서 입력

```js
var categoryData = response.categoryList;  
var categoryPageInfo = response.categoryPageInfo;  
```

controller
```java
@RequestMapping("/3100Category")  
@ResponseBody  
public Map<String, Object> getKWM3100CategoryList(@ModelAttribute("BasicParamVo")BasicParamVo paramVo){  
    log.info("KWM3100Category ::: " + paramVo);  
    Map<String, Object> response = new HashMap<>();  
    if (paramVo.getSc_ID() == null){  
        List<KWM3100CategoryVo> list = (List<KWM3100CategoryVo>) service.selectCategory(paramVo);  
        response.put("categoryList", list);  
    }else{  
        PageHelper.startPage(paramVo.getSc_PAGE_NUM(), paramVo.getSc_PAGE_SIZE());  
        List<KWM3100CategoryVo> list = (List<KWM3100CategoryVo>) service.selectCategory(paramVo);  
        PageInfo<KWM3100CategoryVo> pageInfo = new PageInfo<>(list);  // PageInfo 객체 생성  
  
        response.put("categoryList", list);  
        response.put("categoryPageInfo", pageInfo);  // 페이징 정보 포함  
    }  
    return response;  // 이 객체가 JSON으로 반환됨  
}
```

ResponseBody로 보냄





만들어둔 request.ajax await 사용

```js
$('#addNewBtn').click(async function() {  
  
    var url = window.location.href + "Category";  
    var selectHTML = '<select id="category_id" class="form-control">';  
    // AJAX 요청  
    async function fetchCategoryData() {  
        const url = window.location.href + "Category";  
  
        // Promise로 감싸서 비동기 처리  
        return new Promise((resolve, reject) => {  
            request.ajax(url, 'GET', null, function(response) {  
                if (response && response.categoryList) {  
                    resolve(response.categoryList);  // 성공 시 resolve                } else {  
                    reject('No category data received');  // 실패 시 reject                }  
            });  
        });  
    }  
    const category = await fetchCategoryData();  // await로 비동기 처리  
    selectHTML += '<option value="" disabled selected>카테고리를 선택하세요</option>';  
  
   for (let i = 0; i < category.length; i++) {  
       selectHTML += `<option value="`+category[i].id+`">`+category[i].krName+`</option>`;  
   }  
    selectHTML += '</select>';  
    console.log(category);  

});
```

만들어둔  request.ajax를 aync function으로 변경
```js
async function fetchCategoryData() {  
        const url = window.location.href + "Category";  
  
        // Promise로 감싸서 비동기 처리  
        return new Promise((resolve, reject) => {  
            request.ajax(url, 'GET', null, function(response) {  
                if (response && response.categoryList) {  
                    resolve(response.categoryList);  // 성공 시 resolve                } else {  
                    reject('No category data received');  // 실패 시 reject                }  
            });  
        });  
    } 
    
```

awit 써서  변수에 저장
```js
const category = await fetchCategoryData();  // await로 비동기 처리  
```

이때 await 쓰는 부모도 aync로 선언해야함
```js
$('#addNewBtn').click(async function() {  })
```



## `requestDetail()` 함수


```js
// AJAX 요청을 위한 함수 정의
function requestDetail(detailUrl, data, successCallback, errorCallback) {
    $.ajax({
        url: detailUrl,
        method: 'GET',
        data: data,
        headers: {
            'X-Requested-With': 'XMLHttpRequest',
            'Content-Type': 'application/json'
        },
        success: function(data) {
            if (successCallback) successCallback(data);
        },
        error: function(xhr, status, error) {
            if (errorCallback) errorCallback(xhr, status, error);
        }
    });
}
```


#### 장점:

- **간단하고 직관적**입니다. 한 번 호출할 때 필요한 URL, 데이터, 콜백만 전달하면 되므로 코드가 간결합니다.
- **함수형 프로그래밍**의 장점으로, 함수 하나로 특정 작업을 처리하는 방식입니다.
- **상태 관리**가 필요 없으며, 독립적으로 동작합니다.

#### 단점:

- **확장성 부족**: 기능이 확장되거나 코드가 커질 경우, `requestDetail()` 함수 내에서 관리해야 할 내용이 많아져서 함수가 복잡해질 수 있습니다. 예를 들어, 추가적인 설정이나 여러 메서드를 처리해야 할 때 관리가 어려워질 수 있습니다.
- **기능 분리가 어려운 경우**: 여러 기능을 추가할 경우, `requestDetail` 함수가 너무 많은 책임을 가질 수 있습니다.



2. **객체를 이용한 방식 (`requestDetail.ajax()` 형태)**

```js
var requestDetail = {
    ajax: function(detailUrl, data, successCallback, errorCallback) {
        $.ajax({
            url: detailUrl,
            method: 'GET',
            data: data,
            headers: {
                'X-Requested-With': 'XMLHttpRequest',
                'Content-Type': 'application/json'
            },
            success: function(responseData) {
                if (successCallback) successCallback(responseData);
            },
            error: function(xhr, status, error) {
                if (errorCallback) errorCallback(xhr, status, error);
            }
        });
    }
};
```


#### 장점:

- **확장성**: `requestDetail` 객체 내에서 여러 메서드를 관리할 수 있으므로 기능을 추가하거나 수정할 때 유리합니다. 예를 들어, 다른 종류의 HTTP 요청(POST, PUT 등)을 처리하는 `post()`, `put()` 메서드를 추가할 수 있습니다.
- **구조적이고 명확한 책임 분리**: 메서드가 객체 내부에서 관리되므로 코드가 구조적으로 분리되어 유지보수가 쉬워집니다.
- **모듈화**: 객체를 사용하여 관련된 기능을 묶어서 관리할 수 있으므로, 향후 다른 코드에서 재사용하기가 용이합니다.

#### 단점:

- **구조가 복잡해짐**: 객체를 사용함에 따라 코드가 다소 복잡해질 수 있습니다. 특히 간단한 용도로 사용하는 경우에는 불필요한 구조일 수 있습니다.
- **이해도가 떨어질 수 있음**: `requestDetail.ajax()`처럼 메서드를 객체 내부에서 호출하는 방식은 함수형 방식보다 조금 더 복잡하게 보일 수 있습니다.





Promise와 콜백을 동시에 지원하는 코드:

```js
var requestDetail = {
    ajax: function(detailUrl, data, successCallback, errorCallback) {
        return new Promise(function(resolve, reject) {
            $.ajax({
                url: detailUrl,  // 요청 URL
                method: 'GET',  // GET 방식으로 요청
                data: data,
                headers: {
                    'X-Requested-With': 'XMLHttpRequest',  // AJAX 요청을 알리기 위한 헤더
                    'Content-Type': 'application/json'     // 요청 본문 데이터 형식을 JSON으로 설정
                },
                success: function(responseData) {
                    if (successCallback) successCallback(responseData); // 성공 콜백
                    resolve(responseData);  // Promise 성공
                },
                error: function(xhr, status, error) {
                    if (errorCallback) errorCallback(xhr, status, error); // 실패 콜백
                    reject(error);  // Promise 실패
                }
            });
        });
    }
};

```



1. **콜백 기반 사용**:

```js
requestDetail.ajax(
    detailUrl,
    data,
    function (data) { // 성공 콜백
        console.log(data);  // 서버에서 받은 데이터 확인
        var tbody = $('table tbody[data-detail]');
        console.log(tbody);

        var newDocument = $.parseHTML(data);
        var newContent = $(newDocument).find('table tbody[data-detail]');
        console.log(newContent);

        tbody.html(newContent.html());
        var sc_ID = $('#sc_ID');
    },
    function (xhr, status, error) { // 실패 콜백
        console.error("Error fetching category data:", error);
    }
);
```


2. **`async/await` 기반 사용**:

```js
$(document).on('click', '#editButton', async function () {
    var rowId = $(this).closest('tbody').data("detail");
    var currentUrl = window.location.pathname;
    var detailUrl = currentUrl + 'Detail';
    var data = { sc_ID: rowId };

    try {
        const response = await requestDetail.ajax(detailUrl, data);
        console.log(response); // 응답 데이터 처리
    } catch (error) {
        console.error("Error fetching category data:", error);
    }
});
```


- `requestDetail.ajax`가 Promise를 반환하면서도 **성공/실패 콜백**을 그대로 지원하므로 기존의 방식도 사용할 수 있습니다.
- `async/await` 방식은 콜백을 생략하고 바로 Promise를 활용하므로 코드가 깔끔해집니다.



## beforeSend & headers

- `beforeSend`는 요청을 보내기 전에 실행되며, `xhr` 객체를 이용해 동적으로 헤더나 다른 속성을 설정할 수 있습니다.
- 주로 **CSRF 토큰**을 설정하거나 **동적으로** 헤더를 추가할 때 사용됩니다.
- 비동기 요청에 대한 설정을 변경할 수 있는 시점을 제공합니다.


### 2. `headers`:

`headers`는 AJAX 요청을 정의할 때 요청을 보내기 전에 미리 정의된 HTTP 헤더를 설정하는 옵션입니다. `headers`는 `beforeSend`와 달리 한 번에 모든 헤더를 설정할 수 있으며, 이 방식은 요청이 시작될 때 자동으로 적용됩니다.


### 차이점:

| 특성             | `beforeSend`            | `headers`                                   |
| -------------- | ----------------------- | ------------------------------------------- |
| **사용 시점**      | 요청이 서버로 전송되기 전에 설정      | 요청을 보내기 전에 한 번 설정                           |
| **설정 방식**      | 동적으로 설정                 | 고정된 값으로 설정                                  |
| **주로 사용되는 경우** | CSRF 토큰처럼 동적으로 설정해야 할 값 | `Content-Type`, `X-Requested-With`처럼 고정된 헤더 |
| **유연성**        | 동적으로 값을 설정할 수 있음        | 미리 설정된 값을 사용                                |

beforeSend

```js
$.ajax({
    url: 'example.com',
    method: 'POST',
    data: { key: 'value' },
    beforeSend: function(xhr) {
        var csrfToken = $('#csrf').val();  // 페이지에 있는 hidden input에서 csrf token을 가져옵니다
        xhr.setRequestHeader('X-CSRF-TOKEN', csrfToken);  // CSRF 토큰을 헤더에 추가
    },
    success: function(response) {
        console.log(response);
    },
    error: function(error) {
        console.log(error);
    }
});
```

headers
```js
$.ajax({
    url: 'example.com',
    method: 'POST',
    data: { key: 'value' },
    headers: {
        'X-Requested-With': 'XMLHttpRequest',  // AJAX 요청을 알리기 위한 헤더
        'Content-Type': 'application/json'     // 요청 본문 데이터 형식을 JSON으로 설정
    },
    success: function(response) {
        console.log(response);
    },
    error: function(error) {
        console.log(error);
    }
});

```



`beforeSend` 콜백을 `ajax` 함수 호출 시에 동적으로 전달

```js
var request = {
    ajax: function(detailUrl, method, data, successCallback, errorCallback, beforeSendCallback) {
        return new Promise(function(resolve, reject) {
            $.ajax({
                url: detailUrl,  // 요청 URL
                method: method,     // GET, POST, PUT, DELETE 등
                data: data,
                headers: {
                    'X-Requested-With': 'XMLHttpRequest',  // AJAX 요청을 알리기 위한 헤더
                    'Content-Type': 'application/json'     // 요청 본문 데이터 형식을 JSON으로 설정
                },
                beforeSend: beforeSendCallback,  // 동적으로 beforeSend를 추가
                success: function(responseData) {
                    if (successCallback) successCallback(responseData); // 성공 콜백
                    resolve(responseData);  // Promise 성공
                },
                error: function(xhr, status, error) {
                    if (errorCallback) errorCallback(xhr, status, error); // 실패 콜백
                    reject(error);  // Promise 실패
                }
            });
        });
    }
};

```



```js
// 사용 예시
request.ajax(
    '/your/api/endpoint',  // 요청 URL
    'GET',  // HTTP 메소드
    { someData: 'example' },  // 요청 데이터
    function(response) {
        console.log('Success:', response);
    },
    function(xhr, status, error) {
        console.error('Error:', error);
    },
    function(xhr) {
        // 이 콜백 함수는 요청 전 beforeSend로 실행됩니다.
        var csrfToken = $('#csrf').val();  // 예시로 CSRF 토큰을 가져오는 코드
        xhr.setRequestHeader('X-CSRF-TOKEN', csrfToken);  // 헤더에 CSRF 토큰을 추가
    }
);

```



`beforeSend` 콜백을 정의할 때, **`csrfToken`을 가져오는 코드가 필요 없으면 그냥 생략해도 됩니다**.

```js

// 예시 1: `beforeSend` 콜백 생략

request.ajax(
    '/your/api/endpoint',  // 요청 URL
    'GET',  // HTTP 메소드
    { someData: 'example' },  // 요청 데이터
    function(response) {
        console.log('Success:', response);
    },
    function(xhr, status, error) {
        console.error('Error:', error);
    }  // beforeSend를 사용하지 않으므로 생략
);



// ------------------------- 예시 2: 빈 `beforeSend` 콜백 전달

request.ajax(
    '/your/api/endpoint',  // 요청 URL
    'GET',  // HTTP 메소드
    { someData: 'example' },  // 요청 데이터
    function(response) {
        console.log('Success:', response);
    },
    function(xhr, status, error) {
        console.error('Error:', error);
    },
    function(xhr) {
        // beforeSend 콜백이 빈 함수라면, CSRF 토큰을 설정하지 않아도 됩니다
    }
);


```



