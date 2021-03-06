## Arrays
### 378. Kth Smallest Element in a Sorted Matrix (Medium)
题目描述：矩阵行列分别升序，寻找返回第k小的元素
#### 解法一：PriorityQueue with tuple class and comparator
思路：利用PriorityQueue自动返回最小的功能，首先将矩阵第一行（或第一列）元素放入，返回最小者，然后放入最小者的后面一个，重复k次即可

时间复杂度：minHeap(early stop adding) O(min(K,N)+K∗logN)

空间复杂度：

备注：
 - 注意PriorityQueue 的三种constructor: [GeekforGeeks PriorityQueue with comparator](https://www.geeksforgeeks.org/implement-priorityqueue-comparator-java/)

 - 注意比较comparable and comparator [Comparable and Comparator](https://www.javatpoint.com/difference-between-comparable-and-comparator)
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
     int m = matrix.length, n = matrix[0].length;
        PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>(1, new tupleComaprator());//1 is the initial size; cannot be 0
        for(int j = 0; j < n; j++) pq.offer(new Tuple(0, j, matrix[0][j]));
        for(int i = 0; i < k - 1; i++) { 
            Tuple t = pq.poll();
            if(t.x == m - 1) continue;
         pq.offer(new Tuple(t.x + 1, t.y, matrix[t.x + 1][t.y]));
        }
        return pq.poll().val;
    }

    class Tuple {
        int x, y, val;
        public Tuple(int x, int y, int val) {
            this.x = x; this.y = y; this.val = val;
        }
    }
    
    class tupleComaprator implements Comparator<Tuple>{
        @Override
        public int compare(Tuple t1, Tuple t2) {
            return t1.val - t2.val;
        }
    }
}
```
#### 解法二：PriorityQueue with tuple comparable
思路：同上，注意比较，这里用的comparable

```java
public int kthSmallest(int[][] matrix, int k) {
    int m = matrix.length, n = matrix[0].length;
    PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>();
    for(int j = 0; j < n; j++) pq.offer(new Tuple(0, j, matrix[0][j]));
    for(int i = 0; i < k - 1; i++) { // 小根堆，去掉 k - 1 个堆顶元素，此时堆顶元素就是第 k 的数
        Tuple t = pq.poll();
        if(t.x == m - 1) continue;
        pq.offer(new Tuple(t.x + 1, t.y, matrix[t.x + 1][t.y]));
    }
    return pq.poll().val;
}

class Tuple implements Comparable<Tuple> {
    int x, y, val;
    public Tuple(int x, int y, int val) {
        this.x = x; this.y = y; this.val = val;
    }

    @Override
    public int compareTo(Tuple that) {
        return this.val - that.val;
    }
}
```
#### 解法三：binary search
思路：while lo<hi, 左侧含mid，cnt保证lo hi之间必有想要矩阵元，当lo=hi时返回，while内部：cnt<k时说明全左含mid都不够，lo=mid+1；否则说明含mid是够的，hi=mid，注意不减一

复杂度：N*log(max-min)
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
    int m = matrix.length, n = matrix[0].length;
    int lo = matrix[0][0], hi = matrix[m - 1][n - 1] + 20;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        int cnt = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n && matrix[i][j] <= mid; j++) {
                cnt++;
            }
        }
        if (cnt < k) lo = mid + 1;
        else hi = mid;
    }
    return lo;
        
    }
}
```
### 645. Set Mismatch
题目描述：array本应存储1 ~ n，但是有一个数missing，有一个数duplicated，找出两者，数组返回
#### 解法一：排序之后扫描一次
思路：发现array[i-1]==array[i]即duplicated，发现array[i-1]<a[i]+1,则array[i-1]+1即missing。时间复杂度为NlogN，排序空间复杂度logN

#### 解法二：使用计数HashMap或array扫描两次
思路：HashMap，先扫一遍计数，计数 = .getOrDefault(i, 0)+1，然后再扫一遍HashMap，如果containsKey再判断是否为2，否则不containsKey就是missing。时空均为N 。同样的，也可以用array计数。
``` java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int dup = -1, missing = -1;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }
        for(int i = 1; i <= nums.length; i++) {
            if(map.containsKey(i)) {
                if(map.get(i) == 2) {
                    dup = i;
                }
            }
            else {
                missing = i;
            }
        }
        return new int[] {dup, missing};
    }
}
```
    
#### 解法三：Negative Mark
思路：把数组中数-1当index，翻转数组中这个index的数，翻转之前检查是否曾经被翻转过（注意使用Math.abs()），如果是，不要再次翻转了，这个index曾经出现过，所以duplicated数就是此index + 1。这样一次循环结束后本应所有index上的数都被翻过恰好一次，除了从未出现的index，所以再扫一遍，未翻转过的数所在index从未出现过，missing = index + 1
复杂度： 时间N 空间1.
```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int dup = - 1, missing = -1;
        for(int num : nums) {
            if(nums[Math.abs(num) - 1] < 0) {
                dup = Math.abs(num);
            }
            else {
                nums[Math.abs(num) - 1] *= -1;
            }
        }
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] > 0) {
                missing = i + 1;
            }
        }
        return new int[] {dup, missing};
    }
}
```
#### 解法四：Swapping
思路：尚未明白

### 667. Beautiful Arrangement II
题目描述：将整数1~n重排为这样，相邻两数的差值只有k个不同的数，k<n.
解法：重排为1, 2, 3, ..., n-k-1, n-k; n, n-k+1, n-1, n-k+2, ..., n-k+1+[(k-1)/2], n-k+1+[k/2]
验证：差为1, 1, 1, ..., 1, 1（共n-k-1个）; k, k-1, k-2, k-3, ..., 1(共k个)  
```java
class Solution {
    public int[] constructArray(int n, int k) {
        int[] ans = new int[n];
        int c = 0;
        for(int i = 1; i <= n - k; i++) {
            ans[c++] = i;
        }
        for(int i = 0; i < k; i++) {
            ans[c++] = (i % 2 == 0) ? n - i / 2 : n - k + (i + 1) / 2;
        }
        return ans;
    }
}
```

### 697: Degree of an Array
degree of this array is defined as the maximum frequency of any one of its elements. Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums
就是直接扫几遍
```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        Map<Integer, Integer> firstOcc = new HashMap<>();
        Map<Integer, Integer> lastOcc = new HashMap<>();
        Map<Integer, Integer> count = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            if(!firstOcc.containsKey(nums[i])) {
                firstOcc.put(nums[i], i);
            }
            lastOcc.put(nums[i], i);
            count.put(nums[i], count.getOrDefault(nums[i], 0) + 1);
        }
        
        int max = Collections.max(count.values());
        
        int ans = nums.length;
        
        for(int i : count.keySet()) {
            if(count.get(i) == max) {
                ans = Math.min(ans, lastOcc.get(i) - firstOcc.get(i) + 1);
            }
        }
        return ans;
    }
}
```
### 565. Array Nesting
详见Excel，写的很清楚了，贴在这里主要学习一下  
1. 虽看起来两重循环嵌套实际只一层  
2. 传递值的新写法
```java
class Solution {
    public int arrayNesting(int[] nums) {
        int max = 0;
        for(int i = 0; i < nums.length; i++) {
            //这里并不需要if 非 -1 的条件，因为下一层只有非 -1 才做事 
            int count = 0;
            for(int j = i; nums[j] != -1; ) {
                int t = nums[j];                                               
                nums[j] = -1;
                j = t;
                count++;
            }
            max = Math.max(max, count);               
        }
        return max;
    }
}
```
### 189. Rotate Array
每个数顺延k位，用ON O1 做到。其实不好做，注意复习。
#### 解法一：每转一个就转到底，额外counter记录次数，挨个拿起来转，但是counter够了就退出。难点在写法。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        int count = 0;
        for(int i = 0; count < nums.length; i++) {
            int currIdx = i;
            int prevNum = nums[currIdx];
            do {
                int nextIdx = (currIdx + k) % nums.length;
                int tempNum = nums[nextIdx];
                nums[nextIdx] = prevNum;
                prevNum = tempNum;
                currIdx = nextIdx;
                count++;
            } while(i != currIdx);
        }
    }
}
```
#### 解法二就简单了：仔细想想，转k次之后长啥样？只不过是最后k个元素跑到前面，前面的n-k个元素顺延而已啊！要想in place实现，还需更进一步意识到，这种两大块交换顺序其实可以分解为，先整体全部反序，然后分别把前k个和后n-k个分别内部翻回来就行了。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k ,nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        while(start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```
### 151. Reverse Words in a String
Given an input string s, reverse the order of the words.
#### 本题需学习regular expression，split方法，以及trim函数
```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.split("\\s+");
        StringBuilder sb = new StringBuilder();
        for(int i = words.length - 1; i >=0; i--) {
            sb.append(words[i] + " ");
        }
        return sb.toString().trim();
    }
}
```
### 242. Valid Anagram
题目要求就是判断一个string是否是另一个string的重排。算法很简单，就是排序后再比较。难点在于string，char array的函数
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()) return false;
        char[] a = s.toCharArray();
        char[] b = t.toCharArray();
        Arrays.sort(a);
        Arrays.sort(b);
        return Arrays.equals(a, b);
    }
}
```
### 409. Longest Palindrome
给定纯字母字符串，用其中的字母写成回文串，最长多长？  
细想很简单：每个字母有多少对，就能写进回文串当中(count / 2 * 2)，其中还能多添加一个在正中间，巧妙之处在这里：能添加的字母个数必为奇，(if count % 2 == 1)，且只能添加一次 (&& length % 2 == 0)
```java
class Solution {
    public int longestPalindrome(String s) {
        int[] count = new int[123];
        for(char c : s.toCharArray()) {
            count[c]++;
        }
        int ans = 0;
        for(int v : count) {
            ans += v / 2 * 2;
            if(ans % 2 == 0 && v % 2 == 1) ans++;
        }
        return ans;

    }
}
```

### 205. Isomorphic Strings
我自己的解法！
```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if(s.length() != t.length()) return false;
        
        HashMap<Character, Character> map = new HashMap();
        for(int i = 0; i < s.length(); i++) {
            if(!map.containsKey(s.charAt(i))) {
                if(map.containsValue(t.charAt(i))) return false;
                map.put(s.charAt(i), t.charAt(i));
            }
            else {
                if(t.charAt(i) != map.get(s.charAt(i))) return false;
            }
        }
        return true;
    }
}
```
### 647. Palindromic Substrings
判断一个string里面有多少substring是回文串，这是extend法ON^2。另有高级ON法，待研究。
```java
class Solution {
    int cnt = 0;    
    public int countSubstrings(String s) {
        for(int i = 0; i < s.length(); i++) {
            extend(s, i, i);
            extend(s, i, i + 1);//这里不用担心右边界，extend自己会防止
        }
        return cnt;
    }
    public void extend(String s, int l, int r) {
        while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            cnt++;
            l--;
            r++;
        }
    }
}
```
### 9. Palindrome Number
判断一个数字是不是回文数。我的想法就是String.valueOf(int x)然后判断char
```java
class Solution {
    public boolean isPalindrome(int x) {
        String s = String.valueOf(x);
        for(int i = 0; i < s.length() / 2; i++) {
            if(s.charAt(i) != s.charAt(s.length() - i - 1)) return false;
        }
        return true;
    }
}
```
另外一个解法就是运用数学把数字翻转（优化可以是翻转后一半）
```java
class Solution {
    public boolean isPalindrome(int x) {
        int flip = 0;
        int y = x;
        while(y > 0) {
            flip = flip * 10 + y % 10;
            y = y / 10;
        }
        return flip == x;
    }
}
```
