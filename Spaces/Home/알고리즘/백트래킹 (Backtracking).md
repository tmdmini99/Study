### **백트래킹 (Backtracking)**

모든 경우를 시도하면서 조건이 안 맞으면 되돌아가는 방식

**예: 모든 순열 구하기**

```java
void permute(List<Integer> nums, List<Integer> path) {
    if (path.size() == nums.size()) {
        result.add(new ArrayList<>(path));
        return;
    }
    for (int num : nums) {
        if (path.contains(num)) continue;
        path.add(num);
        permute(nums, path);
        path.remove(path.size() - 1); // backtrack
    }
}
```

