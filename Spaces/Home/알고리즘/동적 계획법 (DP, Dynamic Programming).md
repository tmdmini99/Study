
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


```java
long[] memo = new long[2001];

public long solution(int n) {
    return re(n);
}

public long re(int n){
    if (n == 1 || n == 2) return 1;
    if (memo[n] != 0) return memo[n];
    return memo[n] = re(n - 1) + re(n - 2);
}

```


## i칸에 도달하는 방법 수 = (i-1칸까지 오는 방법 수) + (i-2칸까지 오는 방법 수)


```java
int[] dp = new int[n + 1];
dp[1] = 1;
dp[2] = 2;
```

`long[] dp = new long[n + 1];`  
이 코드에서 `+1`을 해주는 이유는 **배열의 인덱스와 실제 문제에서 사용하는 숫자(n)를 일치시키기 위해서**

이렇게 갯수를 계속 더해주면 원하는 숫자의 갯수를 알수 있다
```java
class Solution {
    public long solution(int n) {
        long [] dp = new long[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i=3; i<=n; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
```

여기서 n이 1일 경우 에러가 발생 -> index는 0,1만 쓸수 있기 때문

```java
class Solution {
    public long solution(int n) {
        long [] dp = new long[n+1];
        dp[0] = 1;
        dp[1] = 2;
        for(int i=2; i<=n; i++){
            dp[i] = (dp[i-1] + dp[i-2])%1234567;
        }
        return dp[n-1];
    }
}
```




