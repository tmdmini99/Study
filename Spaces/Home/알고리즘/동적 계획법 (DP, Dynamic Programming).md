
### **동적 계획법 (DP, Dynamic Programming)**

중복되는 하위 문제를 메모이제이션하여 푸는 방식

**예: 피보나치 수열**


```java
int[] dp = new int[n + 1];
dp[0] = 0;
dp[1] = 1;
for (int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}
```


```java
public int minCountToTarget(int[] numbers, int target) {
    int[] dp = new int[target + 1];
    Arrays.fill(dp, target + 1); // 초기화: 큰 값으로
    dp[0] = 0; // 0을 만들기 위해 숫자 0개 필요

    for (int i = 1; i <= target; i++) {
        for (int num : numbers) {
            if (i - num >= 0) {
                dp[i] = Math.min(dp[i], dp[i - num] + 1);
            }
        }
    }

    return dp[target] > target ? -1 : dp[target]; // 만들 수 없으면 -1
}

```


