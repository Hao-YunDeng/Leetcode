### 127. Word Ladder
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> words = new HashSet<>();
        words.addAll(wordList);
        words.remove(beginWord);
        
        if (!words.contains(endWord)) {
            return 0;
        }
        
        HashSet<String> visited = new HashSet<>();
        Queue<String> q = new LinkedList<>();
        
        q.offer(beginWord);
        visited.add(beginWord);
        
        int step = 0;
        
        while (!q.isEmpty()) {
            step++;
            int size = q.size();
            while (size-- > 0) {
                String word = q.poll();
                char[] chars = word.toCharArray();
                for (int i = 0; i < chars.length; i++) {
                    char originalChar = chars[i];
                    for (char c = 'a'; c <= 'z'; c++) {                          
                        if (c == originalChar) {
                            continue;
                        }
                        
                        chars[i] = c;                        
                        String newWord = new String(chars);
                        
                        if (newWord.equals(endWord)) {// && words.contains(newWord)
                            return step + 1;
                        }
                        
                        if (!visited.contains(newWord) && words.contains(newWord)) {
                            q.add(newWord);
                            visited.add(newWord);
                            
                        }
                    }
                    chars[i] = originalChar;
                }
            }           
        }
        return 0;
    }
}
```
