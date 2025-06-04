### **그리디 (Greedy)**

매번 가장 최선의 선택을 하는 방식

**예: 동전 거스름돈 최소 개수**

```java
int[] coins = {500, 100, 50, 10};
int count = 0;
for (int coin : coins) {
    count += amount / coin;
    amount %= coin;
}
```

```java
import java.util.*;

class Solution {
    public String solution(String X, String Y) {
        int[] xCount = new int[10];
        int[] yCount = new int[10];

        for (char c : X.toCharArray()) xCount[c - '0']++;
        for (char c : Y.toCharArray()) yCount[c - '0']++;

        StringBuilder sb = new StringBuilder();

        for (int i = 9; i >= 0; i--) {
            int count = Math.min(xCount[i], yCount[i]);
            for (int j = 0; j < count; j++) sb.append(i);
        }

        if (sb.length() == 0) return "-1";
        if (sb.toString().replace("0", "").isEmpty()) return "0";

        return sb.toString();
    }
}

```


귤을 최소한으로 담는 법

```java
import java.util.*;
class Solution {
    public int solution(int k, int[] tangerine) {
        int answer = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for(int tang : tangerine){
            map.put(tang,map.getOrDefault(tang,0)+1);
        }
        List<Integer> list = new ArrayList<>(map.values());
        list.sort(Collections.reverseOrder());
        int sum = 0;
        for(int count : list){
            sum += count;
            answer++;
            if(sum >= k) break;
        }
        return answer;
    }
}
```
