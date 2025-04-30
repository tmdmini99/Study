
### **동적 프로그래밍 (DP, Dynamic Programming)**

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

