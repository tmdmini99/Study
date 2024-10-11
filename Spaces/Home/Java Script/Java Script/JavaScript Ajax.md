



```js
document.addEventListener("DOMContentLoaded", function() {
    // 버튼 클릭 이벤트 리스너
    document.querySelectorAll('.load-customer-data').forEach(button => {
        button.addEventListener('click', function() {
            const customerId = this.getAttribute('data-id');

            // AJAX 요청
            fetch(`/api/customers/${customerId}`) // API 엔드포인트는 적절히 수정해야 합니다.
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    // 응답받은 데이터로 스팬 업데이트
                    const row = document.getElementById(`customer-${customerId}`);
                    if (row) {
                        row.querySelector('.customer-name').textContent = data.name || '이름 없음';
                        row.querySelector('.customer-location').textContent = data.location || '위치 없음';
                        row.querySelector('.customer-email').textContent = data.email || '이메일 없음';
                    }
                })
                .catch(error => {
                    console.error('There was a problem with the fetch operation:', error);
                });
        });
    });
});

```