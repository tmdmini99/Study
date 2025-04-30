

### 1. **브루트포스 (Brute Force)**

모든 경우를 전부 시도하는 방식 (완전탐색)

**예: 두 수의 합이 target 되는 경우 찾기**

```java
for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] == target) {
            return new int[]{i, j};
        }
    }
}


```

