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
