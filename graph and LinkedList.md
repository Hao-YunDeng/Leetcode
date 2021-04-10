### 785. Is Graph Bipartite?
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int[] color = new int[graph.length];
        for (int i = 0; i < graph.length; i++) {
            if (color[i] == 0 && !bfs(graph, color, i)) {
                return false;
            }
        }
        return true;
    }
    
    boolean bfs(int[][] graph, int[] color, int node) {
        Queue<Integer> q = new LinkedList<>();
        color[node] = 1;
        q.add(node);
        while (!q.isEmpty()) {
            int v = q.poll();
            for (int u : graph[v]) {
                if (color[u] == 0) {
                    color[u] = -color[v];
                    q.add(u);                   
                }
                else if (color[u] == color[v]) return false;
                
            }
        }
        return true;
    }
}
```
