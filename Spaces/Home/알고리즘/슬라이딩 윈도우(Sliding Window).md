

## 슬라이딩 윈도우(Sliding Window)

배열이나 문자열 같은 연속된 데이터에서 **고정 길이 또는 조건을 만족하는 연속된 부분을 윈도우처럼 이동하면서 처리**하는 방법입니다.


"길이 3인 부분 배열 중 가장 큰 합을 구하라"  
이럴 때 단순하게는 `O(nk)` 걸리지만, 슬라이딩 윈도우를 쓰면 **`O(n)`**으로 줄일 수 있습니다.

```java
// 자바에서 슬라이딩 윈도우 예시 (길이 k의 최대 합)
int maxSum = 0, sum = 0;
for (int i = 0; i < k; i++) sum += arr[i];
maxSum = sum;
for (int i = k; i < arr.length; i++) {
    sum += arr[i] - arr[i - k];  // 윈도우 이동
    maxSum = Math.max(maxSum, sum);
}
```

