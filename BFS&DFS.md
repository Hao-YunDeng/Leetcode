### 1091. Shortest Path in Binary Matrix
```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid[0][0] == 1) return -1;
        int n = grid.length;
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[] {0, 0});
        int step = 0;
        while (!q.isEmpty()) {
            step++;
            int size = q.size();
            while (size-- > 0) {
                int[] v = q.poll();
                int x = v[0], y = v[1];
                if (grid[x][y] == 1) {
                    continue;
                }
                if (x == n - 1 && y == n - 1) {
                    return step;
                }
                for (int i = -1; i <= 1 ; i++) {
                    for (int j = -1; j <= 1; j++) {
                        if (i == 0 && j == 0) {
                            continue;
                        }
                        int newX = x + i, newY = y + j;
                        if (newX > n - 1 || newX < 0 || newY > n - 1 || newY < 0) {
                            continue;
                        }
                        if (grid[newX][newY] == 1) {
                            continue;
                        }
                        q.offer(new int[] {newX, newY});
                    }
                }
                grid[x][y] = 1;                
            }          
        }
        return -1;
    }
}
```
### 127. Word Ladder
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> words = new HashSet<>();
        words.addAll(wordList);
        words.remove(beginWord);
        
        if (!words.contains(endWord)) {
            return 0;
        }
        
        HashSet<String> visited = new HashSet<>();
        Queue<String> q = new LinkedList<>();
        
        q.offer(beginWord);
        visited.add(beginWord);
        
        int step = 0;
        
        while (!q.isEmpty()) {
            step++;
            int size = q.size();
            while (size-- > 0) {
                String word = q.poll();
                char[] chars = word.toCharArray();
                for (int i = 0; i < chars.length; i++) {
                    char originalChar = chars[i];
                    for (char c = 'a'; c <= 'z'; c++) {                          
                        if (c == originalChar) {
                            continue;
                        }
                        
                        chars[i] = c;                        
                        String newWord = new String(chars);
                        
                        if (newWord.equals(endWord)) {// && words.contains(newWord)
                            return step + 1;
                        }
                        
                        if (!visited.contains(newWord) && words.contains(newWord)) {
                            q.add(newWord);
                            visited.add(newWord);                          
                        }
                    }
                    chars[i] = originalChar;
                }
            }           
        }
        return 0;
    }
}
```
### 695. Max Area of Island
```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int area = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                area = Math.max(area, explore(grid, i, j));
            }
        }
        return area;
    }
    
    public int explore(int[][] grid, int i, int j) {
        int m = grid.length;
        int n = grid[0].length;
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || grid[i][j] == 0) {
            return 0;
        }
        grid[i][j] = 0;
        int area = 1;
        int[][] directions = new int[][] {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int[] direction : directions) {
            area += explore(grid, i + direction[0], j + direction[1]);
        }
        return area;
    }
}
```
### 200. Number of Islands
```java
class Solution {
    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int cc = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != '0') {
                    dfs(grid, i, j);
                    cc++;                    
                }
            }
        }
        return cc;
    }
    
    public void dfs(char[][] grid, int i, int j) {
        int m = grid.length;
        int n = grid[0].length;
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || grid[i][j] == '0') {
            return;
        }
        grid[i][j] = '0';
        dfs(grid, i - 1, j);
        dfs(grid, i + 1, j);
        dfs(grid, i, j - 1);
        dfs(grid, i, j + 1);
    }
}
```
### 547. Number of Provinces
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int cc = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(isConnected, i, visited);
                cc++;
            }                            
        }
        return cc;
    }
    
    public void dfs(int[][] isConnected, int i, boolean[] visited) {
        visited[i] = true;
        for (int j = 0; j < isConnected.length; j++) {
            if (!visited[j] && isConnected[i][j] == 1) {
                dfs(isConnected, j, visited);
            }
        }        
    }
}
```
### 130. Surrounded Regions
```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O') {
                dfs(board, i, 0);
            }
            if (board[i][n - 1] == 'O') {
                dfs(board, i, n - 1);
            }
        }
        
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O') {
                dfs(board, 0, j);
            }
            if (board[m - 1][j] == 'O') {
                dfs(board, m - 1, j);
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') board[i][j] = 'X';
                if (board[i][j] == 'V') board[i][j] = 'O';
            }
        }
        
        return;        
    }
    
    public void dfs(char[][] board, int x, int y) {
        int m = board.length;
        int n = board[0].length;
        if (x < 0 || x > m - 1 || y < 0 || y > n - 1 || board[x][y] == 'X' || board[x][y] == 'V') {
            return;
        }
        
        board[x][y] = 'V';
        
        int[] dx = new int[] {-1, 1, 0, 0};
        int[] dy = new int[] {0, 0, 1, -1};
        for (int i = 0; i < dx.length; i++) {
            dfs(board, x + dx[i], y + dy[i]);
        }
        return;
    }
}
```
### 417. Pacific Atlantic Water Flow
```java
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        boolean[][] canReachP = new boolean[m][n];
        boolean[][] canReachA = new boolean[m][n];
        
        for (int i = 0; i < m; i++) {
            dfs(heights, i, 0, canReachP);
            dfs(heights, i, n - 1, canReachA);
        }
        for (int i = 0; i < n; i++) {
            dfs(heights, 0, i, canReachP);
            dfs(heights, m - 1, i, canReachA);
        }
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (canReachP[i][j] && canReachA[i][j]) {
                    res.add(Arrays.asList(i, j));
                }
            }
        }
        return res;
    }
    
    public void dfs(int[][] heights, int x, int y, boolean[][] canReach) {
        int m = heights.length;
        int n = heights[0].length;
        if (canReach[x][y]) {
            return;
        }        
        canReach[x][y] = true;
        int[] dx = new int[] {1, -1, 0, 0};
        int[] dy = new int[] {0, 0, 1, -1};
        for (int i = 0; i < dx.length; i++) {
            if (x + dx[i] < 0 || x + dx[i] > m - 1 || y + dy[i] < 0 || y + dy[i] > n - 1) {
                continue;
            }
            if (heights[x][y] <= heights[x + dx[i]][y + dy[i]]) {
                dfs(heights, x + dx[i], y + dy[i], canReach);
            }
        }
        return;
    }
}
```
