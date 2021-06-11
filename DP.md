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
### 343. Integer Break
```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 100; //I don't need it
        dp[1] = 1;
    
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i / 2; j++) {
                //Call the smallest split the first split, so it won't be 
                //larger than half of the original
                dp[i] = Math.max(dp[i], Math.max(j * dp[i - j], j * (i - j)));
            }
        }
        return dp[n];
    }
}
```
### 91. Decode Ways
```java
class Solution {
    public int numDecodings(String s) {
        if (s.length() == 0) return 0;
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        
        for (int i = 2; i <= n; i++) {
            if (Integer.valueOf(s.substring(i - 1, i)) != 0) dp[i] += dp[i - 1];
            if (Integer.valueOf(s.substring(i - 2, i))  <= 26 && Integer.valueOf(s.substring(i - 2, i)) >= 10) dp[i] += dp[i - 2];
        }
        return dp[n];
        
    }
}
```
### 376. Wiggle Subsequence
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length == 0) return 0;
        int n = nums.length;
        int[] dpUp = new int[n];
        int[] dpDown = new int[n];
        dpUp[0] = 1;
        dpDown[0] = 1;
        int max = 1;
        for (int i = 1; i < n; i++) {
            if(nums[i] < nums[i - 1]) {
                dpDown[i] = dpUp[i - 1] + 1;
                dpUp[i] = dpUp[i - 1];
            }
            else if(nums[i] > nums[i - 1]) {
                dpUp[i] = dpDown[i - 1] + 1;
                dpDown[i] = dpDown[i - 1];
            }
            else {
                dpUp[i] = dpUp[i - 1];
                dpDown[i] = dpDown[i - 1];
            }
            max = Math.max(max, Math.max(dpUp[i], dpDown[i]));
        }
        return max;
    }
}
```
### 309. Best Time to Buy and Sell Stock with Cooldown
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[] have = new int[n + 1];
        int[] notHave = new int[n + 1];
        
        notHave[0] = 0;
        notHave[1] = 0;
        have[0] = Integer.MIN_VALUE;
        have[1] = - prices[0];
        //int maxRes = 0;
        for (int i = 2; i <= n; i++) {
            notHave[i] = Math.max(notHave[i - 1], have[i - 1] + prices[i - 1]);
            have[i] = Math.max(have[i - 1], notHave[i - 2] - prices[i - 1]);
            //maxRes = Math.max(maxRes, notHave[i]);
        }
        return Math.max(have[n], notHave[n]);
        //return maxRes;
    }
}
```
### 714. Best Time to Buy and Sell Stock with Transaction Fee
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[] have = new int[n + 1];
        int[] notHave = new int[n + 1];
        
        notHave[0] = 0;
        have[0] = Integer.MIN_VALUE / 2;
        //Note: if /2 then - fee can be put on either side; 
        //if not /2 then - fee on the MIN_VALUE side will
        //cause overflow, so must be on the non-MIN side!!
        for (int i = 1; i <= n; i++) {
            notHave[i] = Math.max(notHave[i - 1], have[i - 1] + prices[i - 1] - fee);
            have[i] = Math.max(have[i - 1], notHave[i - 1] - prices[i - 1] );
        }
        
        return Math.max(have[n], notHave[n]);
    }
   
}
```
