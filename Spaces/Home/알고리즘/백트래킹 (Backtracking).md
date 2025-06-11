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

순열 구하기 1,2,3
```java
public class Permutations {
    static boolean[] visited;
    static List<Integer> path = new ArrayList<>();

    public static void main(String[] args) {
        int[] nums = {1, 2, 3};
        visited = new boolean[nums.length];
        dfs(nums);
    }

    static void dfs(int[] nums) {
        if (path.size() == nums.length) {
            System.out.println(path);
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) continue;

            visited[i] = true;
            path.add(nums[i]);
            dfs(nums);                  // 재귀
            path.remove(path.size() - 1); // 되돌리기
            visited[i] = false;         // 방문 초기화
        }
    }
}

```