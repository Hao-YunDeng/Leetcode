## 树
### 104. Maximum Depth of Binary Tree
解法一：BFS，很慢，只快于7%
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        
        Queue<TreeNode> q = new LinkedList<>();
        HashMap<TreeNode, Integer> depth = new HashMap<>();
        q.offer(root);
        depth.put(root, 1);
        int max = 1;
        while(!q.isEmpty()) {
            TreeNode node = q.poll(); 
            if(node.left != null || node.right != null) {
                if(node.left != null) {
                q.offer(node.left);
                depth.put(node.left, depth.get(node) + 1);
                }
                
                if(node.right != null) {
                q.offer(node.right);
                depth.put(node.right, depth.get(node) + 1);
                }
                
                max = Math.max(max, depth.get(node) + 1);
            }          
        }
        return max;
        
    }
}
```
### 543. Diameter of Binary Tree
值得复习，似乎全局变量要是不用的话就会麻烦的多...
```java
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        ans = 0;
        depth(root);
        return ans - 1;
    }
    public int depth(TreeNode node) {
        if (node == null) return 0;
        int L = depth(node.left);
        int R = depth(node.right);
        ans = Math.max(ans, L+R+1);
        return Math.max(L, R) + 1;
    }
}
```
### 226. Invert Binary Tree
反正就是两种遍历，本题可以用后续遍历的DFS，先序后序无所谓。  
或者BFS + Queue
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) {
            return null;
        }
        TreeNode right = invertTree(root.right);
        TreeNode left = invertTree(root.left);
        root.right = left;
        root.left = right;
        return root;
        
    }
```
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()) {
            TreeNode node = q.poll();
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            if(node.left != null) q.add(node.left);
            if(node.right != null) q.add(node.right);
        }
        return root;
    }
}
```
}
### 617. Merge Two Binary Trees
本题最关键两行代码在于，merge操作记得返回，更新left, right，而不只是合并val
```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        root1.val = root1.val + root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;        
    }
}
```
### 112. Path Sum
判断tree有没有一条从顶到底的某条路径和等于targetSum
```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        targetSum -= root.val;
        if(root.left == null && root.right == null) {
            return targetSum == 0;
        }
        return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
    }
}
```
### 437. Path Sum III
思考和上题的关系：既然可以不从root出发，就有两种种可能，从root出发，不从root出发。如果不从root出发就跟root没关系了，问题本身转化为参数为root.left或者root.right的同一问题。另外判断标准可以不为leaf，由此，可由上题改进而来：  
不返回true/false而是返回计数；  
判断标准只看sum==0，不看leaf.
```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
        if(root == null) return 0;
        return pathSumStartWithRoot(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
    }
    public int pathSumStartWithRoot(TreeNode root, int sum) {
        if(root == null) return 0;
        sum -= root.val;
        
        int res = 0;        
        if(sum == 0) res = 1;
        res += pathSumStartWithRoot(root.left, sum) + pathSumStartWithRoot(root.right, sum);
        return res;
    }
}
```
### 572. Subtree of Another Tree
判断t是不是s的subtree。思路跟上题一样
```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s == null) return false;
        return isSubtreeWithRoot(s, t) || isSubtree(s.left, t) || isSubtree(s.right, t);        
    }
    public boolean isSubtreeWithRoot(TreeNode s, TreeNode t) {
        if(s == null && t == null) return true;
        if(s == null || t == null) return false;
        if(s.val != t.val) return false;
        //只有相等了没有return才会继续比较
        //如果return false，会return到召唤它递归的一级，层层返回 
        return isSubtreeWithRoot(s.left, t.left) && isSubtreeWithRoot(s.right, t.right);
        
    }
}
```
### 101. Symmetric Tree
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);        
    }
    public boolean isMirror(TreeNode s, TreeNode t) {
        if(s == null && t == null) return true;
        if(s == null || t == null) return false;
        if(s.val != t.val) return false;
        return isMirror(s.left, t.right) && isMirror(s.right, t.left);
    }
}
```
### 111. Minimum Depth of Binary Tree
求根节点到叶子节点的最短距离。本题我们总结一下这种递归：post order DFS。一般的递归思路：先想这个题能不能用递归做（假如知道子问题能不能解决本问题），然后想子问题应该返回什么，最后想初始条件，额外思考一下有没有陷阱，亦即特殊情况，用if修正。完事。具体来说本题思路就是，首先，可以用递归做，第二，需要左边最小，右边最小，二者中较小的加一即为本节点。初始条件不用多说。特殊情况就是左右当中如果有零不能算较小者，应该返回非零者加一。
```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        
        if(left == 0) return right + 1;
        if(right == 0) return left + 1;
        
        return Math.min(left, right) + 1;
    }
}
```
### 404. Sum of Left Leaves
树的所有“左叶子”的和。解题思路：首先本题可以用递归做，需要左子树的“左叶子”和，以及右子树的“左叶子”和，加起来即为返回值，特殊情况是，左子树已经是leaf了，应提前返回，此时左子叶的和即为本身值。
```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;   
        
        if(root.left != null) {
            if(root.left.left == null && root.left.right == null) {
                return root.left.val + sumOfLeftLeaves(root.right);
            }
        }         
        return sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);       
    }    
}
```
### 687. Longest Univalue Path
本题非常类似543题path最长值，添加了要求，要求这个path上的值都相等。计数方法就变成，如果和本节点相等就累加，不想等就记0。同时时刻记录着最大值。
```java
class Solution {
    int ans = 0;
    public int longestUnivaluePath(TreeNode root) {
        dfs(root);
        return ans;
    }
    public int dfs(TreeNode root) {
        if(root == null) return 0;
        int left = dfs(root.left);
        int right = dfs(root.right);
        
        int leftAns = root.left != null && root.left.val == root.val ? left + 1 : 0;
        int rightAns = root.right != null && root.right.val == root.val ? right + 1 : 0;
        
        ans = Math.max(ans, leftAns + rightAns);
        
        return Math.max(leftAns, rightAns);
        
    }
}
```
### 671. Second Minimum Node In a Binary Tree
```java
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        if(root == null) return -1;
        //return dfs(root, root.val);
        return bfs(root);
    }
    public int dfs(TreeNode root, int min) {
        if(root == null) return -1;
        
        if(root.val > min) return root.val;
        
        int l = dfs(root.left, min);
        int r = dfs(root.right, min);
        
        if(l == -1) return r;
        if(r == -1) return l;
        
        return Math.min(l, r);
    }
    
    public int bfs(TreeNode root) {
        if(root == null) return -1;
        int min = root.val;
        Long secondMin = Long.MAX_VALUE;
        boolean found = false;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()) {
            TreeNode node = q.poll();
            if(node.val > min && node.val < secondMin) {
                secondMin = new Long(node.val);
                found = true;
                continue;
            }
            if(node.left != null) q.add(node.left);
            if(node.right != null) q.add(node.right);
        }
        return found ? secondMin.intValue() : -1;
    }
}
```
### 637. Average of Levels in Binary Tree
很典型的BFS，技巧是用一个int size记录每层节点数。这个技巧还可用于BFS求层数。
```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {       
        List<Double> ans = new ArrayList<>();
        if(root == null) return ans;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()) {
            int size = q.size();
            Double sum = 0.0;
            for(int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if(node.left != null) q.add(node.left);
                if(node.right != null) q.add(node.right);
                sum += node.val;
            }
            ans.add(sum / size);
        }
        return ans;
    }
}
```
### 144. Binary Tree Preorder Traversal
这个题注意，声明ans是注意是list还是array list，要和后面的dfs函数接收变量匹配。
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        dfs(root, ans);
        return ans;
    }
    public void dfs(TreeNode node, List<Integer> list) {
        if(node == null) return;
        list.add(node.val);
        dfs(node.left, list);
        dfs(node.right, list);
    }
}
```
### 669. Trim a Binary Search Tree
多回味。可以写的更简洁，见答案，但是我记在这里，好理解的一种写法。
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return root;
        
        if (root.val <= high && root.val >= low) {
            root.left = trimBST(root.left, low, high);
            root.right = trimBST(root.right, low, high);
        }
        
        else if (root.val < low) root = trimBST(root.right, low, high);
        
        else if (root.val > high) root = trimBST(root.left, low, high);
        
        return root;
    }
}
```
### 236. Lowest Common Ancestor of a Binary Tree
其实跟235一样的。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || p == root || q == root) return root;
        
        TreeNode leftNode = lowestCommonAncestor(root.left, p, q);
        TreeNode rightNode = lowestCommonAncestor(root.right, p, q);
        
        if(leftNode != null && rightNode != null) return root;
        
        return leftNode != null ? leftNode : rightNode;
    }
}
```
### 109. Convert Sorted List to Binary Search Tree
```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return new TreeNode(head.val);
        
        ListNode fast = head, slow = head, pre = null;
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            pre = slow;
            slow = slow.next;
        }
        TreeNode root = new TreeNode(slow.val);
        pre.next = null;
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(slow.next);
        
        return root;
    }
}
```
### 208. Implement Trie (Prefix Tree)
```java
class Trie {
    
    class TrieNode {
        boolean isWord = false;
        HashMap<Character, TrieNode> children = new HashMap();
        
        public TrieNode() {
            this.children = new HashMap<>();
        }
    }
    
    TrieNode root;

    /** Initialize your data structure here. */
    public Trie() {
        this.root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode currNode = root;
        for(int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);            
            if(!currNode.children.containsKey(c)) {
                currNode.children.put(c, new TrieNode());
            }
            currNode = currNode.children.get(c);
        }
        currNode.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode currNode = root;
        for(int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);            
            if(!currNode.children.containsKey(c)) {
                return false;
            }
            currNode = currNode.children.get(c);
        }
        return currNode.isWord;
        
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode currNode = root;
        for(int i = 0; i < prefix.length(); i++) {
            char c =prefix.charAt(i);            
            if(!currNode.children.containsKey(c)) {
                return false;
            }
            currNode = currNode.children.get(c);
        }
        return true;
        
    }
}
```
### 677. Map Sum Pairs
每个节点带有以他为前缀的val和
```java
class MapSum {
    class TrieNode {
        Map<Character, TrieNode> children;
        int sum;
        
        public TrieNode() {
            this.children = new HashMap();
            this.sum = 0;
        }
    }

    /** Initialize your data structure here. */
    TrieNode root; 
    HashMap<String, Integer> map; //Haoyun: Record all the prefixes
    
    public MapSum() {
        root = new TrieNode();
        map = new HashMap();
    }
    
    public void insert(String key, int val) {
        int diff  = val - map.getOrDefault(key, 0);
        map.put(key, val);
        TrieNode curr = root;
        //curr.sum += diff; 
        //Haoyun: root is the word starting with empty, 
        //and all words start with empty. This is unnecessary
        for(char c : key.toCharArray()) {
            curr.children.putIfAbsent(c, new TrieNode());
            curr = curr.children.get(c);
            curr.sum += diff;
        }
    }
    
    public int sum(String prefix) {
        TrieNode curr = root;
        for(char c : prefix.toCharArray()) {
            if(curr.children.get(c) != null) {
                curr = curr.children.get(c);
            }
            else {
                return 0;
            }
        }
        return curr.sum;
    }
}
```
或者用array：
```java
class MapSum {
    class TrieNode {
        TrieNode[] children;
        int sum;
        
        public TrieNode() {
            this.children = new TrieNode[26];
            this.sum = 0;
        }
    }

    /** Initialize your data structure here. */
    TrieNode root; 
    HashMap<String, Integer> map; //Haoyun: Record all the prefixes
    
    public MapSum() {
        root = new TrieNode();
        map = new HashMap();
    }
    
    public void insert(String key, int val) {
        int diff  = val - map.getOrDefault(key, 0);
        map.put(key, val);
        TrieNode curr = root;
        for(char c : key.toCharArray()) {
            if(curr.children[c - 'a'] == null) {
                curr.children[c - 'a'] = new TrieNode();
            }
            curr = curr.children[c - 'a'];
            curr.sum += diff;
        }
    }
    
    public int sum(String prefix) {
        TrieNode curr = root;
        for(char c : prefix.toCharArray()) {
            if(curr.children[c - 'a'] != null) {
                curr = curr.children[c - 'a'];
            }
            else {
                return 0;
            }
        }
        return curr.sum;
    }
}
```
