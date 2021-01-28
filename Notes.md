# Leetcode 笔记

## 378. Kth Smallest Element in a Sorted Matrix (Medium)
题目描述：矩阵行列分别升序，寻找返回第k小的元素
### 解法一：PriorityQueue with tuple class and comparator
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
### 解法二：PriorityQueue with tuple comparable
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
### 解法三：binary search
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
## 645. Set Mismatch
题目描述：array本应存储1 ~ n，但是有一个数missing，有一个数duplicated，找出两者，数组返回
### 解法一：排序之后扫描一次，发现array[i-1]==array[i]即duplicated，发现array[i-1]<a[i]+1,则array[i-1]+1即missing。时间复杂度为NlogN，排序空间复杂度logN

### 解法二：HashMap，先扫一遍计数，计数 = .getOrDefault(i, 0)+1，然后再扫一遍HashMap，如果containsKey再判断是否为2，否则不containsKey就是missing。时空均为N 。同样的，也可以用array计数。
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
    


