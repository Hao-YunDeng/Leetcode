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
