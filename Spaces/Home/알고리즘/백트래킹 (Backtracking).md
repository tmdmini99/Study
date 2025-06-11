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

던전 돌때 제일 많이 돌수 있는 경우의 수
```java
import java.util.*;
class Solution {
    public int solution(int k, int[][] dungeons) {
        boolean[] visited = new boolean[dungeons.length];
        int[] maxCount = new int[1];

        dfs(k, dungeons, visited, 0, maxCount);

        return maxCount[0];
    }

    private void dfs(int currentK, int[][] dungeons, boolean[] visited, int count, int[] maxCount) {
        if (count > maxCount[0]) maxCount[0] = count;

        for (int i = 0; i < dungeons.length; i++) {
            if (visited[i]) continue;

            if (currentK >= dungeons[i][0]) {
                visited[i] = true;
                dfs(currentK - dungeons[i][1], dungeons, visited, count + 1, maxCount);
                visited[i] = false;
            }
        }
    }
}
```


dfs로 호출하고 그 안에서 for문이 돌면서 또 dfs를 호출하고 또 그안에서 for문 돌고 거기서 dfs를 호출
그리고 제일 마지막 dfs가 끝나면 for문이 돌면서 지우고 그다음 이 for문이 끝나면 또 밖에 for문이 마저 돌면서 dfs 호출이 끝남

```java
start: path = []

1 선택 (visited[0] = true)
├─ 2 선택 (visited[1] = true)
│   └─ 3 선택 (visited[2] = true)
│       └─ 출력: [1, 2, 3]
│       └─ 되돌리기: 3 제거, visited[2] = false
│
│   └─ 되돌리기: 2 제거, visited[1] = false
│
├─ 3 선택 (visited[2] = true)
│   └─ 2 선택 (visited[1] = true)
│       └─ 출력: [1, 3, 2]
│       └─ 되돌리기: 2 제거, visited[1] = false
│
│   └─ 되돌리기: 3 제거, visited[2] = false
│
└─ 되돌리기: 1 제거, visited[0] = false

2 선택 (visited[1] = true)
├─ 1 선택 (visited[0] = true)
│   └─ 3 선택 (visited[2] = true)
│       └─ 출력: [2, 1, 3]
│       └─ 되돌리기: 3 제거, visited[2] = false
│
│   └─ 되돌리기: 1 제거, visited[0] = false
│
├─ 3 선택 (visited[2] = true)
│   └─ 1 선택 (visited[0] = true)
│       └─ 출력: [2, 3, 1]
│       └─ 되돌리기: 1 제거, visited[0] = false
│
│   └─ 되돌리기: 3 제거, visited[2] = false
│
└─ 되돌리기: 2 제거, visited[1] = false

3 선택 (visited[2] = true)
├─ 1 선택 (visited[0] = true)
│   └─ 2 선택 (visited[1] = true)
│       └─ 출력: [3, 1, 2]
│       └─ 되돌리기: 2 제거, visited[1] = false
│
│   └─ 되돌리기: 1 제거, visited[0] = false
│
├─ 2 선택 (visited[1] = true)
│   └─ 1 선택 (visited[0] = true)
│       └─ 출력: [3, 2, 1]
│       └─ 되돌리기: 1 제거, visited[0] = false
│
│   └─ 되돌리기: 2 제거, visited[1] = false
│
└─ 되돌리기: 3 제거, visited[2] = false

```

이 상태가 되는 것


## STEP 1: 제일 첫 번째 dfs() 호출, path = []

- for문 i = 0: 숫자 1 선택
    
- visited[0] = true, path = [1]
    
- dfs() 재귀 호출
    

---

## STEP 2: 두 번째 dfs() 호출, path = [1]

- for문 i = 0: 방문했으니 건너뜀
    
- for문 i = 1: 숫자 2 선택
    
- visited[1] = true, path = [1, 2]
    
- dfs() 재귀 호출
    

---

## STEP 3: 세 번째 dfs() 호출, path = [1, 2]

- for문 i = 0,1: 방문했으니 건너뜀
    
- for문 i = 2: 숫자 3 선택
    
- visited[2] = true, path = [1, 2, 3]
    
- dfs() 재귀 호출
    

---

## STEP 4: 네 번째 dfs() 호출, path = [1, 2, 3]

- path 크기가 nums 길이와 같으니 출력
    
- 출력: `[1, 2, 3]`
    
- 여기서 재귀 끝나고, 바로 STEP 4의 for문으로 돌아가서 끝나니까, STEP 4에서 **return**
    

---

## STEP 5: STEP 3로 복귀 (path = [1, 2, 3] 상태)

- 이제 `path.remove(path.size() - 1);` → 3 제거 → path = [1, 2]
    
- `visited[2] = false`
    
- for문 i = 2가 끝나서 STEP 3의 for문 종료
    
- STEP 3 함수 종료 → return 하면서 STEP 2로 돌아감
    

---

## STEP 6: STEP 2로 복귀 (path = [1, 2])

- `path.remove(path.size() - 1);` → 2 제거 → path = [1]
    
- `visited[1] = false`
    
- for문 i=1 끝, i=2로 이동 (STEP 2 for문 계속)
    

---

## STEP 7: STEP 2 for문 i=2 선택

- `visited[2] = true`
    
- `path.add(3)` → path = [1, 3]
    
- dfs() 재귀 호출 → STEP 8 진행
    

---

## STEP 8: STEP 3 호출 (path = [1, 3])

- for문 i=0: 방문했으니 건너뜀
    
- for문 i=1: 방문 안 했으니 선택
    
- visited[1] = true, path = [1, 3, 2]
    
- dfs() 재귀 호출 → STEP 9
    

---

## STEP 9: STEP 4 호출 (path = [1, 3, 2])

- path 길이 같아서 출력 → `[1, 3, 2]`
    
- return → 되돌리기 시작
    
- `path.remove(2)` → 2 제거, path = [1, 3]
    
- visited[1] = false
    
- for문 끝나서 STEP 4 종료 → return
    

---

## STEP 10: STEP 3 복귀 (path = [1, 3])

- `path.remove(path.size() - 1);` → 3 제거, path = [1]
    
- visited[2] = false
    
- for문 끝나서 STEP 3 종료 → return
    

---

## STEP 11: STEP 2 복귀 (path = [1])

- for문 끝남 → STEP 2 종료 → return
    

---

## STEP 12: STEP 1 복귀 (path = [1])

- `path.remove(path.size() - 1);` → 1 제거, path = []
    
- visited[0] = false
    
- for문 i=0 끝, i=1로 넘어감
    

---

## STEP 13: STEP 1 for문 i=1 선택 (숫자 2)

- `visited[1] = true`
    
- `path.add(2)` → path = [2]
    
- dfs() 재귀 호출 → STEP 14