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

```



### keyup 과 input 이벤트의 차이
- `keyup` 이벤트는 사용자가 키를 누르고 뗄 때 발생하므로, 입력 필드에서 실시간으로 변경된 값을 감지하는 데 유용합니다.
- `input` 이벤트는 입력값이 변경될 때마다 발생하지만, 어떤 이유로 `keyup` 이벤트를 사용하고 싶으신 경우에 적합합니다.