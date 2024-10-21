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



### keyup 과 input 이벤트의 차이
- `keyup` 이벤트는 사용자가 키를 누르고 뗄 때 발생하므로, 입력 필드에서 실시간으로 변경된 값을 감지하는 데 유용합니다.
- `input` 이벤트는 입력값이 변경될 때마다 발생하지만, 어떤 이유로 `keyup` 이벤트를 사용하고 싶으신 경우에 적합합니다.



```js
$(document).on('click', '.Polaris-IndexFilters__ButtonWrap button:contains("취소")', function() {}
```

text 내용 가지고  이벤트 만들기



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