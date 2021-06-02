### 634. Find the Derangement of An Array
```java
class Solution {
    public int findDerangement(int n) {
        if (n < 2) return 0;
        int[] dp = new int[n + 1];
        dp[1] = 0;
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            dp[i] = (int)(((i - 1L) * (dp[i - 1] + dp[i - 2])) % 1000000007);
        }
        return dp[n];
        
    }
}
```
### 64. Minimum Path Sum
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        if (n == 0) return 0;
        int m = grid[0].length;

        int[][] dp = new int[n][m];
        
        dp[0][0] = grid[0][0];
                
        for (int i = 1; i < n; i++) dp[i][0] = dp[i - 1][0] + grid[i][0];
        for (int j = 1; j < m; j++) dp[0][j] = dp[0][j - 1] + grid[0][j];
       
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                dp[i][j] = grid[i][j] + Math.min(dp[i][j - 1], dp[i - 1][j]);
            }
        }
        return dp[n - 1][m - 1];                                                         
    }
}
```
### 303. Range Sum Query - Immutable
```java
class NumArray {
    int[] sums;
    public NumArray(int[] nums) {
        this.sums = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            if (i == 0) this.sums[i] = nums[i]; 
            else sums[i] = sums[i - 1] + nums[i];
        }    
    }
    
    public int sumRange(int left, int right) {
        if (left == 0) return sums[right];
        return sums[right] - sums[left - 1];
    }
}
```
