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
### 123. Best Time to Buy and Sell Stock III
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int buy1 = Integer.MIN_VALUE;
        int sell1 = 0;
        int buy2 = Integer.MIN_VALUE;
        int sell2 = 0;
        
        for (int price : prices) {
            if (- price > buy1) buy1 = -price;
            if (price + buy1 > sell1) sell1 = price + buy1;
            
            if (- price + sell1 > buy2) buy2 = -price + sell1;
            if (price + buy2 > sell2) sell2 = price + buy2;
        }
        return sell2;
        
    }
}
```
### 188. Best Time to Buy and Sell Stock IV
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if (n == 0) return 0;
        
        if (k > n / 2) k = n / 2;
        
//         int[][][] dp = new int[n + 1][k + 1][2];
//         for (int j = 0; j < k + 1; j++) {
//             dp[0][j][0] = 0;
//             dp[0][j][1] = Integer.MIN_VALUE;
//         }
//         int max = 0;
//         for (int i = 1; i <= n; i++) {
//             for (int j = 1; j <= k; j++) {
                   //I don't worry about j = 0 since zero transaction automatically 0 already as default
//                 dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i - 1]);
                
//                 //In this notation, when I buy in, it's already considered a transaction.
//                 //This is helpful for making sure the max of the prev transaction
//                 //is a true max, since buying in will decrease its value.
                
//                 dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i - 1]);
//                 max = Math.max(max, dp[i][j][0]);
//             }
//         }
//         return max;
        
        int[][] dp = new int[k + 1][2];
        for (int j = 0; j <= k; j++) {
            dp[j][0] = 0;
            dp[j][1] = Integer.MIN_VALUE;
        }
        int max = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= k; j++) {
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i - 1]);
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i - 1]);
                max = Math.max(max, dp[j][0]);
            }    
        }
        return max;
    }
}
```
#### 现在我们总结四道股票题：
Cooldown: 因为没有交易次数限制，[days + 1][0, 1]. 因为涉及冷却时间，所以时间维度不可压缩  
transaction fee: 同样没有交易次数，[days + 1][0, 1]. 没有冷却时间，时间维度可以压缩，于是变成单变量DP  
限制两次交易：[days + 1][trans1, trans2][0, 1]. 时间可压缩，trans1, trans2, sell, buy可以变成四个变量buy1, sell1, buy2, sell2 的 DP  
k次交易：[days + 1][trans's + 1][0, 1]. 同样时间可压缩  

另外的两道股票交易题：一个是不限交易，无fee无冷却，只需把fee拿到题去除fee即可，[days + 1][0, 1]；另一个是只许交易一次，同样是[days + 1][0, 1]，但转移方式不同，这个类似于限制两次那种，独立的if条件使得买和卖独立演化，保证只一次（或两次）。
### 122. Best Time to Buy and Sell Stock II, no limitation
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n == 0) return 0;
        
        int[][] dp = new int[n + 1][2];
        
        dp[0][0] = 0;
        dp[0][1] = Integer.MIN_VALUE;
        
        for (int i = 1; i <= n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i - 1]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i - 1]);
        }
        return dp[n][0];
        
    }
}
```

### 121. Best Time to Buy and Sell Stock, only once
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n == 0) return 0;
        
        int have = Integer.MIN_VALUE, notHave = 0;
        
        for (int price : prices) {
            if (- price > have) have = -price;
            if (have + price > notHave) notHave = have + price;
        }
        return notHave;
    }
}
```
### 650. 2 Keys Keyboard
```java
class Solution {
    public int minSteps(int n) {
        int[] dp = new int[n + 1];
        for (int i = 0; i < n + 1; i++) dp[i] = i;
        
        dp[1] = 0;

        
        for (int i = 2; i <= n; i++) {
            for (int j = i / 2; j >= Math.sqrt(i); j--) {
                if (i % j == 0) {
                    dp[i] = dp[j] + i / j;
                    break;
                }
            }
        }
        return dp[n];
    }
}
```
### 我们先总结一下BS，特别是边界条件，用下面两道题确定一个范式：
### 704. Binary Search， 经典BS
```java
class Solution {
    public int search(int[] nums, int target) {
        int l = 0, h = nums.length - 1;
        while (l <= h) {
            int mid = (l + h) / 2;
            if (nums[mid] == target) return mid;
            
            if (nums[mid] < target) {
                l = mid + 1;
            }
            else {
                h = mid - 1;
            }
        }
        return - 1;
    }
}
```
### 852. Peak Index in a Mountain Array 数组的山峰，涉及答案以及答案左边两个元素
```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int l = 0, h = arr.length - 1;
        while (l < h) {
            int mid = (l + h) / 2;
            if (arr[mid - 1] < arr[mid]) {
                l = mid + 1; 
            }
            else {
                h = mid;
            }
        }
        return l - 1;
    }
}
```
### 69. Sqrt(x)
```java
class Solution {
    public int mySqrt(int x) {
        if (x == 1) return 1;
        int l = 1, h = x;
        while (l <= h) {
            int mid = (l + h) / 2;
            
            if (x / mid == mid) return mid;
            
            if (mid < x / mid) {
                l = mid + 1;
            }
            
            else if (mid > x / mid) {
                h = mid - 1;
            }
        }
        //因为向下取整，考虑到1向上跳, h向下跨越
        //所以返回l-1或者h均可
        return h;
    }
}
```
### 744. Find Smallest Letter Greater Than Target
```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int l = 0, h = letters.length - 1;
        while (l <= h) {
            int mid = (l + h) / 2;
            if (letters[mid] <= target) {
                l = mid + 1;
            }
            else {
                h = mid - 1;
            }
        }
        
        return l == letters.length ? letters[0] : letters[l];        
    }
}
```
### 153. Find Minimum in Rotated Sorted Array
```java
class Solution {
    public int findMin(int[] nums) {
        int l = 0, h = nums.length - 1; 
        
        while (l < h) {
            int mid = l + (h - l) / 2;
            if (nums[mid] > nums[h]) {
                l = mid + 1;
            }
            else {
                h = mid;
            }
        }
        return nums[l];
        
        //The following is wrong for [4, 5, 6, 7, 0, 1, 2]
        //because it falls into eles statement and
        //keep moving right, while we are looking for smallest.
        
        // while (l < h) {
        //     int mid = l + (h - l) / 2;
        //     if (nums[mid] <= nums[l]) {
        //         h = mid;
        //     }
        //     else {
        //         l = mid + 1;
        //     }
        // }
        // return nums[l];
    }
}
```
