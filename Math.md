### 504. Base 7
```java
class Solution {
    public String convertToBase7(int num) {
        if (num == 0) return "0";
        StringBuilder sb = new StringBuilder();
        boolean isNegative = (num < 0);
        if (isNegative) num = -num;
        
        while (num != 0) {
            sb.append(num % 7);
            num /= 7;
        }
        
        if (isNegative) sb.append("-");
        sb.reverse();
        return sb.toString();        
    }
}
```
### 168. Excel Sheet Column Title
```java
class Solution {
    public String convertToTitle(int columnNumber) {
        if (columnNumber == 0) return "";
        
        StringBuilder sb = new StringBuilder();
        while(columnNumber > 0) {
            columnNumber--;
            sb.append((char)(columnNumber % 26 + 'A'));
            columnNumber /= 26;
        }
        return sb.reverse().toString();
    }
}
```
### 415. Add Strings
```java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1, carry = 0;
        StringBuilder sb = new StringBuilder();
        while(i >= 0 || j >= 0 || carry > 0) {
            if (i >= 0) carry += num1.charAt(i--) - '0';
            if (j >= 0) carry += num2.charAt(j--) - '0';
            sb.append(carry % 10);
            carry /= 10;
        } 
        return sb.reverse().toString();
        
    }
}
```
