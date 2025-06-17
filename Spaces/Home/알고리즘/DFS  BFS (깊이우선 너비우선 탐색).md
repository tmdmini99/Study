

### **DFS / BFS (깊이우선 / 너비우선 탐색)**

그래프 탐색의 기본 방식

**DFS 예: 재귀로 모든 경로 탐색**


```java
void dfs(int node, boolean[] visited, List<List<Integer>> graph) {
    visited[node] = true;
    for (int next : graph.get(node)) {
        if (!visited[next]) dfs(next, visited, graph);
    }
}
```


```java
class Solution {
    public int solution(int[] numbers, int target) {
        return dfs(numbers, target, 0, 0);
    }

    private int dfs(int[] numbers, int target, int depth, int sum) {
        // 모든 숫자를 다 사용한 경우
        if (depth == numbers.length) {
            // 누적합이 target이면 경우의 수 +1
            return sum == target ? 1 : 0;
        }

        // 다음 숫자를 +로 더하는 경우와 -로 빼는 경우를 각각 재귀 호출
        int plus = dfs(numbers, target, depth + 1, sum + numbers[depth]);
        int minus = dfs(numbers, target, depth + 1, sum - numbers[depth]);

        return plus + minus;
    }
}

```

1단계: `numbers[0] = 4`
```mardown
                0
               / \
          +4       -4
```

2단계: `numbers[1] = 1`
```mardown
         +4               -4
        /  \             /   \
     +4+1  +4-1      -4+1   -4-1
      5     3         -3    -5
```
3단계: `numbers[2] = 2`
```mardown
         5        3        -3       -5
       /  \     /  \     /  \     /   \
      7   3    5   1    -1  -5   -3   -7
```


4단계: `numbers[3] = 1`
```mardown
    7     3     5     1    -1    -5   -3     -7
   / \   / \   / \   / \   / \   / \  / \    / \
  8  6 4   2  6   4 2   0 -2 -6 -4 -6 -2 -4 -6  -8
```

```java
// 마지막 노드 (depth == 4)
dfs(4, 4) → return 1;

// 위로 올라감
dfs(3, 5):
    plus = dfs(4, 6) → 실패 → return 0
    minus = dfs(4, 4) → 성공 → return 1
    return 0 + 1 = 1

dfs(2, 3):
    plus = dfs(3, 5) → return 1 (성공 경로)
    minus = dfs(3, 1) → 나중에 또 재귀 탐색 → return 0
    return 1 + 0 = 1

dfs(1, 4):
    plus = dfs(2, 5) → 실패 경로
    minus = dfs(2, 3) → return 1 (성공)
    return 0 + 1 = 1

dfs(0, 0):
    plus = dfs(1, 4) → return 1
    minus = dfs(1, -4) → 실패
    return 1 + 0 = **1**
```




---
## BFS


```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {

    int[] dr = new int[]{-1, 1, 0, 0};
    int[] dc = new int[]{0, 0, -1, 1};

    public int solution(int[][] maps) {

        int lr = maps.length;
        int lc = maps[0].length;

        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(new int[]{0, 0});

        int[][] visited = new int[lr][lc];
        visited[0][0] = 1; //자기 칸 포함이기 때문에 1로 시작

        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            int r = pos[0], c = pos[1];

            if (r == lr - 1 && c == lc - 1) return visited[r][c];

            for (int i = 0; i < 4; i++){
                int nr = r + dr[i], nc = c + dc[i];
                if (0 <= nr && nr < lr && 0 <= nc && nc < lc && maps[nr][nc] == 1 && visited[nr][nc] == 0) {
                    queue.offer(new int[]{nr, nc});
                    visited[nr][nc] = visited[r][c] + 1;
                }
            }    
        }

        return -1;
    }
}
```

여기서 

`while (!queue.isEmpty())`은 `int[] pos = queue.poll();`로 꺼내면서 지우고 만약 다음 탐색할 곳이 있으면
`queue.offer(new int[]{nr, nc});`를 넣어서 while문이 계속 돌게 된다.

여기서 

```java
for (int i = 0; i < 4; i++){
                int nr = r + dr[i], nc = c + dc[i];
                if (0 <= nr && nr < lr && 0 <= nc && nc < lc && maps[nr][nc] == 1 && visited[nr][nc] == 0) {
                    queue.offer(new int[]{nr, nc});
                    visited[nr][nc] = visited[r][c] + 1;
                }
            }
```

여길 통해 갈수 있는 방향을 모두 저장한 후 다음 껄 다시 가져와서 비교를 한다 근데 짧은 거리로 갈 경우 이미 도착해서 멈추기 때문에 최단 거리로만 유지가 가능