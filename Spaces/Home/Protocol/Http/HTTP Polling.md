### 1. http polling

서버로 일정 주기 요청 송신



![[Polling4.png]]

일정한 주기로 서버에 요청(Request)을 보내는 방법.  
`setTimeout`, `setInterval` 등으로 일정 주기마다 서버에 요청(Request)  
불필요한 Request 와 Connection을 생성하여 서버에 부담
요청 주기가 짧을 수록 부하가 커짐
일정 주기마다' 요청을 보내는 것이기 때문에 실시간이라고 보기에 애매
요청 주기가 짧으면 실시간 처럼 보이겠지만, 실제로 실시간은 x
HTTP 통신을 하기 때문에 Request, Response 헤더가 불필요하게 크다.


![[Polling1.png]]


**cf. polling**  
프로그램이 충돌 회피 또는 동기화 처리 등을 목적으로 다른 프로그램의 상태를 주기적으로 검사하여 일정한 조건을 만족할 때 송수신 등의 자료 처리를 하는 방식

- real-time 통신에서는 언제 통신이 발생할지 예측이 불가능하다
- 불필요한 request와 connection을 생성
- 사진을 보면, 이벤트가 없는데도 요청을 하게됨. 그러면 이벤트 발생 ‘후' 응답을 받을 때까지의 대기 간극이 생김. (-) 불필요한 시간
- real-time 통신이라고 하기 애매한할 정도의 실시간성  
    

### 2. http long-polling

서버에 요청을 보내고 이벤트가 생겨 응답 받을 때까지 서버 측에서 연결을 종료하지 않는것 . 응답을 받으면 끊고 다시 재요청


![[Polling2.png]]
**cf. long-polling**  
클라이언트가 웹 서버에세 새로운 내용이 있는지 물어보았을 때 웹 서버에서 새로운 이벤트가 없다면 대답해 주지 않다가 새로운 내용이 생기면 이 때 대답해 주는 방식

- 불필요한 request를 없앰(polling의 단점 일부 보완)
- 결국 많은 양의 메세지가 쏟아질 경우 polling과 같아짐..  
    

### 3. http streaming

서버에 요청 보내고 끊기지 않은 연결상태에서 끊임없이 데이터를 수신한다.

![[Polling3.png]]

- 클라이언트에서 서버로의 데이터 송신이 어렵다  
    

### 하지만...

결과적으로 이 모든 방법이 HTTP를 통해 통신하기 때문에 Request, Response 둘다 Header가 불필요하게 큼 (빠르게 데이터를 주고받아야할 때에는, 이게 걸림돌이 될 수 있다는 의미)



polling방식으로 확인
```js
$(document).on('click', 'button:has(span:contains("내보내기"))', function() {  
    const pathSegments = window.location.pathname.split('/');  
    const tableName = pathSegments[pathSegments.length - 1];  
  
    // 동적으로 로딩 모달 생성  
    const loadingModal = $(`  
        <div id="loadingModal" style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.5); display: flex; align-items: center; justify-content: center; z-index: 1000;">  
            <div class="loading-content" style="background: white; padding: 20px; border-radius: 5px; text-align: center;">  
                <p>파일을 준비 중입니다...</p>  
                <div class="spinner" style="margin-top: 10px; width: 30px; height: 30px; border: 4px solid #f3f3f3; border-top: 4px solid #3498db; border-radius: 50%; animation: spin 1s linear infinite;"></div>  
            </div>  
        </div>  
    `);  
  
    // 로딩 모달 애니메이션 정의  
    $('<style>')  
        .prop('type', 'text/css')  
        .html(`  
            @keyframes spin {                0% { transform: rotate(0deg); }                100% { transform: rotate(360deg); }            }        `).appendTo('head');  
  
    // 로딩 모달을 body에 추가  
    $('body').append(loadingModal);  
  
    // 동적으로 폼 생성  
    const form = $('<form>', {  
        action: '/csv/csv',  
        method: 'POST',  
        target: 'downloadFrame'  
    }).appendTo('body');  
  
    // 숨겨진 input 필드 생성 (table 파라미터 전달)  
    $('<input>', {  
        type: 'hidden',  
        name: 'table',  
        value: tableName  
    }).appendTo(form);  
  
    // CSRF 토큰 추가 (필요시)  
    $('<input>', {  
        type: 'hidden',  
        name: $('#csrfToken').attr('name'),  
        value: $('#csrfToken').val()  
    }).appendTo(form);  
  
    // iframe을 통해 다운로드 요청  
    const iframe = $('<iframe>', {  
        name: 'downloadFrame',  
        style: 'display: none;'  
    }).appendTo('body');  
  
    // 폼 제출  
    form.submit();  
  
    // 주기적으로 iframe의 상태를 확인하는 폴링 함수  
    const interval = setInterval(() => {  
        const iframeDocument = iframe[0].contentDocument || iframe[0].contentWindow.document;  
  
        // 응답 완료 시 로딩 모달 제거  
        if (iframeDocument.readyState === 'complete') {  
            clearInterval(interval); // 폴링 중지  
            loadingModal.remove();  
            form.remove();  
            iframe.remove();  
        }  
    }, 500); // 0.5초 간격으로 확인  
});
```




---
참조 - 