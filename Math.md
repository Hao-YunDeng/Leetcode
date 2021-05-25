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
