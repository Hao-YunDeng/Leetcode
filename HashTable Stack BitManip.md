## HashTable Stack BitManip
### 535. Encode and Decode TinyURL
```java
public class Codec {
    HashMap<String, String> index = new HashMap(); //key, URL
    HashMap<String, String> revIndex = new HashMap(); //URL, key
    final String BASE_HOST = "http://tinyurl.com/";
    final String charSet = "abcdefghijklmnopqrstuvwxyz0123456789";
    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if(revIndex.containsKey(longUrl)) {
            return revIndex.get(longUrl);
        }
        StringBuilder sb = new StringBuilder();
        while(true) {         
            for(int i = 0; i < 6; i++) {
                int pos = (int) (Math.random() * charSet.length());
                sb.append(charSet.charAt(pos));
            }
            
            if(!index.containsKey(sb.toString())) {
                break;
            }             
        }
        String key = sb.toString();
        revIndex.put(longUrl, key);
        index.put(key, longUrl);
        return BASE_HOST + key;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return index.get(shortUrl.substring(BASE_HOST.length()));
    }
}
```
### 232. Implement Queue using Stacks
```java
class MyQueue {
    private Stack<Integer> s1;
    private Stack<Integer> s2;
    /** Initialize your data structure here. */
    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        s1.add(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(s2.isEmpty()) {
            while(!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }
        return s2.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if(s2.isEmpty()) {
            while(!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }        
        return s2.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }
}
```
### 155. Min Stack
```java
class MinStack {
    Stack<Integer> stack;
    Stack<Integer> minStack;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();   
    }
    
    public void push(int x) {
        stack.push(x);
        if(minStack.isEmpty() || x <= minStack.peek()) {
            minStack.push(x);
        }
    }
    
    public void pop() {
        int min = stack.pop();
        if(minStack.peek() == min) {
            minStack.pop();
        }
        
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```
### 739. Daily Temperatures
输入温度数组，返回一个数组，每个元素是温度数组相应元素后面还要几天才能升温。
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        Stack<Integer> stack = new Stack<>();
        
        int[] res = new int[T.length];
        for (int i = 0; i < T.length; i++) {
            while (!stack.isEmpty() && T[stack.peek()] < T[i]) {
                int idx = stack.pop();
                res[idx] = i - idx;
            }
            stack.push(i);                
        }
        return res;
    }
}
```
### 461. Hamming Distance
很简单，异或之后数1的个数，注意if语句不能少。
```java
class Solution {
    public int hammingDistance(int x, int y) {
        int cnt = 0;
        int z = x ^ y;
        while (z != 0) {
            if (z % 2 == 1) cnt++;
            z = z >> 1;            
        }
        return cnt;
        
    }
}
```
### 371. Sum of Two Integers
这是while写法，也可写成递归的，见cs-note
```java
class Solution {
    public int getSum(int a, int b) {
        
        int sum = 0;
        int carry = 0;
        
        while (b != 0) {
            sum = a ^ b;
            carry = (a & b) << 1;
            a = sum;
            b = carry;
        }
        return a;        
    }
}
```
