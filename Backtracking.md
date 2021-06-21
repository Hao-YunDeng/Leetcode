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
### 93. Restore IP Addresses
```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        StringBuilder temp = new StringBuilder();
        backtracking(s, 0, temp, res);
        return res;        
    }
    
    public void backtracking(String s, int k, StringBuilder temp, List<String> res) {
        if (s.length() == 0 || k == 4) {
            if (s.length() == 0 && k == 4) {//4 parts in IP
                res.add(temp.toString());
            }
            return;
        }
        for (int i = 1; i <= 3 && i <= s.length(); i++) {//i is len, so <=, not just <!
            //each part can be 1, 2, 3 digits long. i for length
            if (i != 1 && s.charAt(0) == '0')  {
                //length of this part is not 1 but there's a leading 0, illegal
                break;
                //not just continue, because later i would work either
            }
            int len = temp.length();
            String part = s.substring(0, i);
            if (Integer.valueOf(part) <= 255) {
                if (len != 0) {
                    part = "." + part;
                }
                temp.append(part);
                backtracking(s.substring(i), k + 1, temp, res);
                temp.setLength(len);
            }
        }
        //return;
    }
}
```
