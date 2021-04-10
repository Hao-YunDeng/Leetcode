### 785. Is Graph Bipartite?
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int[] colors = new int[graph.length];
        for (int i = 0; i < graph.length; i++) {
            // if (colors[i] == 0 && !bfs(graph, colors, i)) {
            //     return false;
            // }
            if (colors[i] == 0 && !dfs(graph, colors, i, 1)) {
                return false;
            }
        }
        return true;
    }
    
    boolean bfs(int[][] graph, int[] colors, int node) {
        Queue<Integer> q = new LinkedList<>();
        colors[node] = 1;
        q.add(node);
        while (!q.isEmpty()) {
            int v = q.poll();
            for (int u : graph[v]) {
                if (colors[u] == 0) {
                    colors[u] = -colors[v];
                    q.add(u);                   
                }
                else if (colors[u] == colors[v]) return false;
                
            }
        }
        return true;
    }
    
    boolean dfs(int[][] graph, int[] colors, int v, int color) {
        if (colors[v] != 0) {
            return colors[v] == color;
        }
        colors[v] = color;
        for (int u : graph[v]) {
            if (!dfs(graph, colors, u, -color)) {
                return false;
            }
        }
        return true;
        
    }
}
```
