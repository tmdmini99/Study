



```jsp
<div class="Polaris-Pagination__TablePaginationActions">
    <div>
        <button id="previousButton" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly Polaris-Button--disabled" aria-label="이전" aria-disabled="true" type="button" tabindex="-1">
            <span class="Polaris-Button__Icon">
                <span class="Polaris-Icon">
                    <!-- 왼쪽 화살표 SVG -->
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">
                        <path fill-rule="evenodd" d="M9.78 3.47a.75.75 0 0 1 0 1.06l-3.47 3.47 3.47 3.47a.749.749 0 1 1-1.06 1.06l-4-4a.75.75 0 0 1 0-1.06l4-4a.75.75 0 0 1 1.06 0"></path>
                    </svg>
                </span>
            </span>
        </button>
    </div>
    <div>
        <button id="nextButton" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly" aria-label="다음" type="button" tabindex="0">
            <span class="Polaris-Button__Icon">
                <span class="Polaris-Icon">
                    <!-- 오른쪽 화살표 SVG -->
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">
                        <path fill-rule="evenodd" d="M5.72 12.53a.75.75 0 0 1 0-1.06l3.47-3.47-3.47-3.47a.749.749 0 1 1 1.06-1.06l4 4a.75.75 0 0 1 0 1.06l-4 4a.75.75 0 0 1-1.06 0"></path>
                    </svg>
                </span>
            </span>
        </button>
    </div>
</div>

<script>
  let currentPage = 1; // 현재 페이지 (1 페이지부터 시작)
  const pageSize = 50; // 페이지당 데이터 개수
  const previousButton = document.getElementById('previousButton');
  const nextButton = document.getElementById('nextButton');

  // 페이지네이션 버튼 상태 업데이트
  function updateButtons(totalPages) {
    // 첫 페이지일 때 이전 버튼 비활성화
    previousButton.disabled = currentPage === 1;
    if (previousButton.disabled) {
      previousButton.classList.add('Polaris-Button--disabled');
    } else {
      previousButton.classList.remove('Polaris-Button--disabled');
    }

    // 마지막 페이지일 때 다음 버튼 비활성화
    nextButton.disabled = currentPage === totalPages;
    if (nextButton.disabled) {
      nextButton.classList.add('Polaris-Button--disabled');
    } else {
      nextButton.classList.remove('Polaris-Button--disabled');
    }
  }

  // 서버에 페이지네이션 데이터 요청
  async function fetchData(page) {
    const offset = (page - 1) * pageSize;
    
    try {
      const response = await fetch(`/api/data?page=${page}&pageSize=${pageSize}`);
      const result = await response.json();

      // 결과를 화면에 업데이트하는 로직 (result.data)
      console.log(result.data);

      // 총 페이지 수 받아서 버튼 상태 업데이트
      const totalPages = Math.ceil(result.totalCount / pageSize);
      updateButtons(totalPages);
    } catch (error) {
      console.error('데이터 가져오기 실패:', error);
    }
  }

  // 이전 버튼 클릭
  previousButton.addEventListener('click', () => {
    if (currentPage > 1) {
      currentPage--;
      fetchData(currentPage);
    }
  });

  // 다음 버튼 클릭
  nextButton.addEventListener('click', () => {
    currentPage++;
    fetchData(currentPage);
  });

  // 첫 페이지 로드 시 데이터 가져오기
  fetchData(currentPage);
</script>
```


