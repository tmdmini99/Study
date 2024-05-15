.parent().siblings('.modal-content-body') : 부모의 가장 가까운 형제중 클래스가 '.modal-content-body'인 tag로


opener.$('\[popup-name="' + window.name + '"]').attr('popup-param').split(";");
1. opener.$('\[popup-name="' + window.name + '"]'): 팝업을 열었던 부모 창에서 현재 팝업 창의 popup-name 속성과 일치하는 요소를 찾습니다.
2. .attr('popup-param'): 해당 요소의 popup-param 속성 값을 가져옵니다.
3. .split(";"): 가져온 popup-param 값을 세미콜론(;)을 기준으로 분할하여 배열로 반환합니다.
4. 