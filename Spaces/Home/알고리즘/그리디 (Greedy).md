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

