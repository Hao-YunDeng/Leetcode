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
### 46. Permutations
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        List<Integer> temp = new ArrayList<>();
        backtracking(nums, visited, temp, res);
        return res;
    }
    
    public void backtracking(int[] nums, boolean[] visited, List<Integer> temp, List<List<Integer>> res) {
        if (temp.size() >= nums.length) {
            res.add(new ArrayList<>(temp));
            //The below line will finally make all added temps's be []!
            //That's because the final temp when getting out of all recursions
            //will be empty!
            
            //res.add(temp);
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {            
            // if (visited[i]) {
            //     continue;
            // }            
            // visited[i] = true;
            // temp.add(nums[i]);
            // backtracking(nums, visited, temp, res);
            // temp.remove(temp.size() - 1); 
            // visited[i] = false;
            
            if (!visited[i]) {
                visited[i] = true;
                temp.add(nums[i]);
                backtracking(nums, visited, temp, res);
                //it will be wrong if the next two lines 
                //are out of the if statement
                temp.remove(temp.size() - 1); 
                visited[i] = false;
            }                        
        }
    }
}
```
### 47. Permutations II
```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        List<Integer> temp = new ArrayList<>();
        
        Arrays.sort(nums);
        
        backtracking(nums, visited, temp, res);
        return res;
    }
    
    public void backtracking(int[] nums, boolean[] visited, List<Integer> temp, List<List<Integer>> res) {
        if (temp.size() >= nums.length) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {            
            if (visited[i]) {
                continue;
            } 
            
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) {
                //if i and i - 1 are same, and when i - 1 hasn't 
                //been placed, you can't place i. In this way, 
                //equal elements have to be placed in order. 
                //Thus it removes repeatition
                continue;
            }
            
            visited[i] = true;
            temp.add(nums[i]);
            backtracking(nums, visited, temp, res);
            temp.remove(temp.size() - 1); 
            visited[i] = false;
                                   
        }
    }
}
```
### 77. Combinations
```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        backtracking(n, k, temp, res, 1);
        return res;
    }
    public void backtracking(int n, int k, List<Integer> temp, List<List<Integer>> res, int currStart) {
        if (temp.size() == k) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = currStart; i <= n; i++) {
            temp.add(i);
            backtracking(n, k, temp, res, i + 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```
### 39. Combination Sum
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        backtracking(candidates, target, temp, res, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, List<Integer> temp, List<List<Integer>> res, int index) {
        if (target == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = index; i < candidates.length; i++) {
            if (candidates[i] > target) {
                continue;
            }
            temp.add(candidates[i]);
            backtracking(candidates, target - candidates[i], temp, res, i);
            temp.remove(temp.size() - 1);
        }
    }
}
```
### 40. Combination Sum II
```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        boolean[] visited = new boolean[candidates.length];
        Arrays.sort(candidates);
        backtracking(candidates, target, temp, res, 0, visited);
        return res;
    }
    public void backtracking(int[] candidates, int target, List<Integer> temp, List<List<Integer>> res, int index, boolean[] visited) {
        if (target == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        for (int i = index; i < candidates.length; i++) {
            if (candidates[i] > target) {
                continue;
            }
            if (i > 0 && candidates[i] == candidates[i - 1] && !visited[i - 1]) {
                continue;
            }
            visited[i] = true;
            temp.add(candidates[i]);
            backtracking(candidates, target - candidates[i], temp, res, i + 1, visited);
            temp.remove(temp.size() - 1);
            visited[i] = false;
        }
    }
}
```
### 216. Combination Sum III
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        backtracking(k, n, temp, res, 1);
        return res;
    }
    public void backtracking(int k, int n, List<Integer> temp, List<List<Integer>> res, int currStart) {
        if (k == 0 || n == 0) {
            if (k ==0 && n == 0) {
                res.add(new ArrayList<>(temp));
            }            
            return;
        }
        for (int i = currStart; i <= 9; i++) {
            if (i > n) {
                continue;
            }
            temp.add(i);
            backtracking(k - 1, n - i, temp, res, i + 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```
### 78. Subsets
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        for (int i = 0; i <= nums.length; i++) {
            backtracking(nums, temp, res, 0, i);
        }       
        return res;
    }
    public void backtracking(int[] nums, List<Integer> temp, List<List<Integer>> res, int index, int size) {
        if (size == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = index; i < nums.length; i++) {
            temp.add(nums[i]);
            backtracking(nums, temp, res, i + 1, size - 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```
### 90. Subsets II
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        Arrays.sort(nums);
        for (int i = 0; i <= nums.length; i++) {
            backtracking(nums, temp, res, 0, i, visited);
        }       
        return res; 
    }
    
    public void backtracking(int[] nums, List<Integer> temp, List<List<Integer>> res, int index, int size, boolean[] visited) {
        if (size == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = index; i < nums.length; i++) {
            if (i > 0 && nums[i - 1] == nums[i] && !visited[i - 1]) {
                continue;
            }
            temp.add(nums[i]);
            visited[i] = true;
            backtracking(nums, temp, res, i + 1, size - 1, visited);
            temp.remove(temp.size() - 1);
            visited[i] = false;
        }
    }
}
```
### 131. Palindrome Partitioning
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        List<String> temp = new ArrayList<>();
        backtracking(s, temp, res, 0);
        return res;
    }
    public void backtracking(String s, List<String> temp, List<List<String>> res, int index) {
        if (index == s.length()) {
            res.add(new ArrayList<>(temp));
            return;
        }
        for (int i = index + 1; i <= s.length(); i++) {
            if (isPalindrome(s.substring(index, i))) {
                temp.add(s.substring(index, i));
                backtracking(s, temp, res, i);
                temp.remove(temp.size() - 1);
            }
        }
    }
    public boolean isPalindrome(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) {
            if (s.charAt(l) != s.charAt(r)) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
}
```
