

```js
const interval = setInterval(() => {  
            let currentWidth = parseFloat($('#loadingBar').css('width')) / $(window).width() * 100;  
            if (currentWidth < 100) {  
                updateLoadingBar(currentWidth + 1); // 퍼센트처럼 진행  
            } else {  
                clearInterval(interval); // 최대치에 도달하면 인터벌 종료  
            }  
        }, 100); // 100ms마다 1% 증가  
  
// 로딩 바 표시 함수  
function showLoading() {  
    document.getElementById('loadingContainer').style.display = 'block'; // 로딩 바 표시  
    updateLoadingBar(0); // 로딩 바 초기화  
    startLoading(); // 로딩 시작  
}  
  
// 로딩 바 숨김 함수  
function hideLoading() {  
    document.getElementById('loadingContainer').style.display = 'none'; // 로딩 바 숨김  
}  
  
// 로딩 바 업데이트 함수  
function updateLoadingBar(percent) {  
    $('#loadingBar').css('width', percent + '%'); // 로딩 바의 너비를 퍼센트로 설정  
}  
  
// 로딩 시작 함수  
function startLoading() {  
    const interval = setInterval(() => {  
        let currentWidth = parseFloat($('#loadingBar').css('width')) / $(window).width() * 100;  
        if (currentWidth < 100) {  
            updateLoadingBar(currentWidth + 1); // 퍼센트처럼 진행  
        } else {  
            clearInterval(interval); // 최대치에 도달하면 인터벌 종료  
            hideLoading(); // 완료 후 로딩 바 숨김  
        }  
    }, 100); // 100ms마다 1% 증가  
}  
  
    $(function (){  
        // HTML에 로딩 바 추가  
        $('body').append(`  
            <div id="loadingContainer" style="position: fixed; top: 0; left: 0; width: 100%; height: 5px; background-color: rgba(128, 128, 128, 0.7); display: none; z-index: 9999;">  
                <div id="loadingBar" style="width: 0; height: 5px; background: blue; transition: width 0.1s;"></div>  
            </div>  
        `);  
    })
```


