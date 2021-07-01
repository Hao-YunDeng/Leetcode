### 347. Top K Frequent Elements
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> frequencies = new HashMap<>();
        for (int num : nums) {
            frequencies.put(num, frequencies.getOrDefault(num, 0) + 1);
        }
        List<Integer>[] buckets = new ArrayList[nums.length + 1];//no <>!!
        for (int key : frequencies.keySet()) {
            int f = frequencies.get(key);
            if (buckets[f] == null) {
                buckets[f] = new ArrayList<>();
            }
            buckets[f].add(key);
        }
        List<Integer> topK = new ArrayList<>();
        for (int i = buckets.length - 1; i >= 0; i--) {
            if (buckets[i] != null && topK.size() < k) {
                for (int num : buckets[i]) {
                    topK.add(num);
                }
            }
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = topK.get(i);
        }
        return res;
    }
}
```
### 451. Sort Characters By Frequency
```java
class Solution {
    public String frequencySort(String s) {
        HashMap<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray()) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }
        
        List<Character>[] buckets = new List[s.length() + 1];
        for (char c : freq.keySet()) {
            int f = freq.get(c);
            if (buckets[f] == null) {
                buckets[f] = new ArrayList<>();
            }
            buckets[f].add(c);
        }
        StringBuilder sb = new StringBuilder();
        for (int i = s.length(); i >= 0; i--) {
            if (buckets[i] == null) {
                continue;
            }
            for(char c : buckets[i]) {
                for (int j = 0; j < i; j++) {
                    sb.append(c);
                }
            }
        }
        return sb.toString();
    }
}
```
### 75. Sort Colors
```java
class Solution {
    public void sortColors(int[] nums) {
        if (nums.length == 1) return;
        
        int i = 0, zero = 0, two = nums.length - 1;
        while (i <= two) {
            if (nums[i] == 0) {
                swap(nums, zero, i);
                zero++;
            }
            if (nums[i] == 2) {
                swap(nums, i, two);
                two--;
            }
            if (nums[i] == 1) {
                i++;
            }
            if (zero > i) {
                i = zero;
            }
        }
    }
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
### 241. Different Ways to Add Parentheses
```java
class Solution {
    public List<Integer> diffWaysToCompute(String expression) {
        List<Integer> res = new ArrayList<>();
        
        // This is one possible recursion ending condition
        // not as beautiful as the one below
        
        // int j = 0;
        // for (;  j < expression.length(); j++) {
        //     char c = expression.charAt(j);
        //     if (c == '+' || c == '-' || c == '*'){
        //         break;
        //     }
        // }
        // if (j == expression.length()) {
        //     res.add(Integer.valueOf(expression));
        //     return res;
        // }

        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                List<Integer> left = diffWaysToCompute(expression.substring(0, i));
                List<Integer> right = diffWaysToCompute(expression.substring(i + 1));
                for (int l : left) {
                    for (int r : right) {
                        if (c == '+') {
                            res.add(l + r);
                        } 
                        else if (c == '-') {
                            res.add(l - r);
                        }
                        else {
                            res.add(l * r);
                        }
                    }
                }
            }
        }
        if (res.size() == 0) {
            res.add(Integer.valueOf(expression));
        }
        return res;
    }
}
```
### 95. Unique Binary Search Trees II
```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        return helper (1, n);
    }
    public List<TreeNode> helper(int l, int r) {
        List<TreeNode> res = new ArrayList<>();
        if (l > r) {
            res.add(null);
            return res;
        }
        for (int i = l; i <= r; i++) {            
            List<TreeNode> leftChildren = helper(l, i - 1);
            List<TreeNode> rightChildren = helper(i + 1, r);
            for (TreeNode left : leftChildren) {
                for (TreeNode right : rightChildren) {
                    TreeNode root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    res.add(root);
                }
            }
        }
        return res;
    }
}
```
### 252. Meeting Rooms
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if (intervals.length <= 1) return true;
        
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        });
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < intervals[i - 1][1]) {
                return false;
            }
        }
        return true;
    }
}
```
### 253. Meeting Rooms II
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int[] start = new int[intervals.length];
        int[] end = new int[intervals.length];
        for (int i = 0; i < intervals.length; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        int count = 0;
        int p1 = 0, p2 = 0;
        
        //General idea: meeting comes in at start times one by one
        //You always look at the earliest ending time when new meeting comes in
        //when noe over, open a new room and fit it in
        //when over, fit the meeting in and move to the next earliest ending time

        for (; p1 < intervals.length; p1++) {
            if (end[p2] > start[p1]) {
                //Meeting not over, cannot release a room
                //Have to open new for 
                //all coming meetings and hold p2
                count++;
            }
            else {
                //Meeting over, no need for a new room
                //and the curr start time fits in
                //Now look at the next earliest ending time
                p2++;
            }
        }
        return count;
    }
}
```
