### 17. Letter Combinations of a Phone Number
```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if (digits.length() == 0) return res;
        
        String[] keys = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        StringBuilder temp = new StringBuilder();
        
        backtracking(digits, 0, keys, temp, res);
        return res;
    }
    
    public void backtracking(String digits, int index, String[] keys, StringBuilder temp, List<String> res) {
        if (index == digits.length()) {
            res.add(temp.toString());
            return;
        }
        int len = temp.length();
        int num = digits.charAt(index) - '0';
        for (int i = 0; i < keys[num].length(); i++) {
            temp.append( keys[num].charAt(i));
            backtracking(digits, index + 1, keys, temp, res);
            temp.setLength(len);
        }        
        return;
    }
}
```
### 93. Restore IP Addresses
```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        StringBuilder temp = new StringBuilder();
        backtracking(s, 0, temp, res);
        return res;        
    }
    
    public void backtracking(String s, int k, StringBuilder temp, List<String> res) {
        if (s.length() == 0 || k == 4) {
            if (s.length() == 0 && k == 4) {//4 parts in IP
                res.add(temp.toString());
            }
            return;
        }
        for (int i = 1; i <= 3 && i <= s.length(); i++) {//i is len, so <=, not just <!
            //each part can be 1, 2, 3 digits long. i for length
            if (i != 1 && s.charAt(0) == '0')  {
                //length of this part is not 1 but there's a leading 0, illegal
                break;
                //not just continue, because later i would work either
            }
            int len = temp.length();
            String part = s.substring(0, i);
            if (Integer.valueOf(part) <= 255) {
                if (len != 0) {
                    part = "." + part;
                }
                temp.append(part);
                backtracking(s.substring(i), k + 1, temp, res);
                temp.setLength(len);
            }
        }
        //return;
    }
}
```
### 79. Word Search
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(board, i, j, word, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
    public boolean dfs(char[][] board, int x, int y, String word, int index) {
        char c = board[x][y];
        if (c != word.charAt(index)) {
            return false;
        }
        if (index == word.length() - 1) {
            return c == word.charAt(index);
        }
        
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};
        board[x][y] = '#';
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < dx.length; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];
            if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                if (board[newX][newY] == '#') {
                    continue;
                }
                if(dfs(board, newX, newY, word, index + 1)) {
                    //search from next to the (possibly) end
                    return true;
                }
            }
        }
        board[x][y] = c;
        return false;
    }
}
```
### 257. Binary Tree Paths
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        StringBuilder temp = new StringBuilder();
        dfs(root, temp, res);
        return res;
    }
    public void dfs(TreeNode root, StringBuilder temp, List<String> res) {
        if (root == null) return;
        
        //add the current one
        int len = temp.length();        
        if (len == 0) {
            temp.append(Integer.toString(root.val));
            len = temp.length();
        }
        else {
            temp.append("->" + root.val);
            len = temp.length();
        }
        //if the curr is an end, add this as a path and return 
        if (root.left == null && root.right == null) {
            res.add(temp.toString());
            return;
        }  
        
        dfs(root.left, temp, res);
        temp.setLength(len);
        dfs(root.right, temp, res);
        //temp.setLength(len);                
    }
    
}
```
