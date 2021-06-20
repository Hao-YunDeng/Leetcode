### 17. Letter Combinations of a Phone Number
```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if (digits.length() == 0) return res;
        
        String[] keys = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        StringBuilder temp = new StringBuilder();
        
        backtracking(digits, 0, keys, temp, res);
        return res;
    }
    
    public void backtracking(String digits, int index, String[] keys, StringBuilder temp, List<String> res) {
        if (index == digits.length()) {
            res.add(temp.toString());
            return;
        }
        int len = temp.length();
        int num = digits.charAt(index) - '0';
        for (int i = 0; i < keys[num].length(); i++) {
            temp.append( keys[num].charAt(i));
            backtracking(digits, index + 1, keys, temp, res);
            temp.setLength(len);
        }        
        return;
    }
}
```
