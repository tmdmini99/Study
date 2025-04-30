

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


