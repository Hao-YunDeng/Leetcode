### 435. Non-overlapping Intervals
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {       
        Arrays.sort(intervals, new Comparator<int[]>() {
           @Override
           public int compare(int[] a,  int[] b) {
               //return a[0] - b[0]; //wrong!
               return a[1] - b[1]; //Works fine
               //return (a[1] < b[1]) ? -1 : ((a[1] == b[1]) ? 0 : 1);
           }
        });
        int end = intervals[0][1];
        int ans = 0;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= end) { // same ending point is acceptable             
                end = intervals[i][1];
            }
            else {
                ans++;                
            }
        }
        return ans;
    }
}
```
### 406. Queue Reconstruction by Height
```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                return (a[0] == b[0]) ? (a[1] - b[1]) : (b[0] - a[0]);
            }
        });
        List<int[]> list = new ArrayList<>();
        for (int[] p : people) {
            list.add(p[1], p);
        }
        // int[][] res = new int[people.length][2];
        // for (int i = 0; i < list.size(); i++) {
        //     res[i] = list.get(i);
        // }
        // return res;
        
        int[][] res = new int[people.length][2];
        return list.toArray(res);
        
        //return list.toArray(new int[0][0]);
    }
}
```
### 605. Can Place Flowers
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int len = flowerbed.length;
        int count = 0;
        for (int i = 0; i < len && count < n; i++) {
            if (flowerbed[i] == 1) {
                continue;
            }
            int left = (i == 0) ? 0 : flowerbed[i - 1];
            int right = (i == len - 1) ? 0 : flowerbed[i + 1];
            if (left == 0 && right == 0) {
                flowerbed[i] = 1;
                count++;
            }
        }
        return count >= n;
    }
}
```
