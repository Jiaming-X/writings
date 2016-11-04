### Leetcode 272.

### **272. Closest Binary Search Tree Value II**

题目链接：[https:\/\/leetcode.com\/problems\/closest-binary-search-tree-value-ii\/](https://leetcode.com/problems/closest-binary-search-tree-value-ii/)

挺有意思的题目

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        LinkedList<TreeNode> stack1 = new LinkedList<TreeNode>();
        LinkedList<TreeNode> stack2 = new LinkedList<TreeNode>();
        
        buildStacks(root, target, stack1, stack2);
        
        for (int i = 0; i < k; i++) {
            if (stack1.isEmpty()) {
                result.add(stack2.peekLast().val);
                expandLarger(stack2);
            } else if (stack2.isEmpty()){
                result.add(stack1.peekLast().val);
                expandSmaller(stack1);
            } else {
                int left = stack1.peekLast().val;
                int right = stack2.peekLast().val;
                
                if (Math.abs(left - target) > Math.abs(right - target)) {
                    result.add(stack2.peekLast().val);
                    expandLarger(stack2);
                } else {
                    result.add(stack1.peekLast().val);
                    expandSmaller(stack1);
                }
            }
        }
        
        return result;
    }
    
    public void expandSmaller (LinkedList<TreeNode> stack) {
        TreeNode tmp = stack.pollLast().left;
        while (tmp != null) {
            stack.offerLast(tmp);
            tmp = tmp.right;
        }
    }
    
    public void expandLarger (LinkedList<TreeNode> stack) {
        TreeNode tmp = stack.pollLast().right;
        while (tmp != null) {
            stack.offerLast(tmp);
            tmp = tmp.left;
        }
    }
    
    public void buildStacks (TreeNode root, double target, LinkedList<TreeNode> stack1, LinkedList<TreeNode> stack2) {
        TreeNode curr = root;
        
        while (curr != null) {
            if (curr.val  <= target) {
                stack1.offerLast(curr);
                curr = curr.right;
            } else {
                stack2.offerLast(curr);
                curr = curr.left;
            }
        }
    }
}
```

