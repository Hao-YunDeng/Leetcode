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
