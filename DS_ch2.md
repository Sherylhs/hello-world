
##Leetcode 102 return level order traversal
##BFS
BFS用Queue(橫向遍歷)，DFS用stack(豎向遍歷)
https://leetcode.com/problems/binary-tree-level-order-traversal/
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List result = new ArrayList();

        if (root == null) {
            return result;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            ArrayList<Integer> level = new ArrayList<>();
            int size = queue.size();  //要先取出size

            for (int i = 0; i < size; i++) {
                TreeNode head = queue.poll();  //不停取出第n層的所有節點
                level.add(head.val);

                if (head.left != null) {
                    queue.offer(head.left);
                }
                if (head.right != null) {
                    queue.offer(head.right);
                }

            }
            result.add(level);

        }
        return result;
    }
}
```
##Leetcode 107. return bottom up Level Order Traversal
