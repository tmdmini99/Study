

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