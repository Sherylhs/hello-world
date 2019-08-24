
## Leetcode 102 return level order traversal
## BFS
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
                TreeNode head = queue.poll();  //不停取出第n層的所有節點!!
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
## Leetcode 107. return bottom up Level Order Traversal
Collections.reverse() 


## Lintcode 242. Convert Binary Tree to Linked Lists by Depth
透過：
dummy 不變作為頭
taile 不停往右移動，去遍歷每一層級的所有Node直到沒有
再透過for循環進入下一層，直到最後一層

  0    -> null
dummy     
  |
tail

```java
public class Solution {
    /**
     * @param root the root of binary tree
     * @return a lists of linked list
     */
    public List<ListNode> binaryTreeToLists(TreeNode root) {
        List<ListNode> result = new ArrayList<>();
        
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        ListNode dummy = new ListNode(0);
        ListNode lastNode = null;
        
        while (!queue.isEmpty()) {
            lastNode = dummy;
            int size = queue.size();  //size要在外面先取好 長度才不會隨著for loop changes
            
            for (int i = 0; i < queue.size(); i++) {
                TreeNode head = queue.poll();
                lastNode.next = new ListNode(head.val);
                lastNode = lastNode.next;
                
                if (head.left != null) {
                    queue.offer(head.left);
                }
                if (head.right != null) {
                    queue.offer(head.right);
                }
            }
            result.add(dummy.next);
        }
        
        return result;
    }
}
```