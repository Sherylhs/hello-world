## BFS
* when to use BFS (Breadth First Search) 寬度優先搜索?
    * 層級遍歷 level order traversal
    * 由點到面 connected component
    * 拓墣排序 topological sorting
        * 一張圖有許多種「拓撲順序」。只要不違背圖上每一條邊的先後規定，要怎麼排列圖上的點都行。把圖上一條由 A 點連向 B 點的邊，想成是 A 必須排在 B 前方（ B 必須排在 A 後方）。「拓撲排序」用來找出合理的排列順序，讓每一個點的先後順序，符合每一條邊所規定的先後順序。cannot reverse or go back, there is no cycle or circle in this sorting.

## Leetcode 102 return level order traversal

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


## LeetCode 297. Serialize and Deserialize Binary Tree
serialize: converting a data structure or object into a sequence of bits.

deserialize: covert string to data structure.

* Used: ArrayList, StringBuilder, subString, split

```java
public class Codec {

    public String serialize(TreeNode root) {

        if (root == null) {
            return "{}";
        }
        ArrayList<TreeNode> list = new ArrayList<>();
        list.add(root);

        for (int i = 0; i < list.size(); i++) {
            TreeNode head = list.get(i); //head is a pointer
            if (head == null) {
                continue;
            }
            list.add(head.left);
            list.add(head.right);

        }

        while (list.get(list.size() - 1) == null) {
            list.remove(list.size() - 1);
        }
        
        //use StringBuilder with append method to make up string
        StringBuilder sb = new StringBuilder();
        sb.append("{");
        sb.append(root.val);

        for (int i = 1; i < list.size(); i++) {
            if (list.get(i) == null) {
                sb.append(",N");
            } else {
                sb.append("," + list.get(i).val);
            }
        }
        sb.append("}");
        return sb.toString();
    }

    public TreeNode deserialize(String data) {
        if (data.equals("{}")) {
            return null;
        }
        String[] nums = data.substring(1, data.length() - 1).split(",");

        ArrayList<TreeNode> list = new ArrayList<>();
        TreeNode root = new TreeNode(Integer.parseInt(nums[0]));
        list.add(root);

        boolean isLeft = true;                   //the first child node is left
        int index = 0;                           //used to point which node's left or right
        for (int i = 1; i < nums.length; i++) {  //start with the i = 1 node

            if (!nums[i].equals("N")) {
                TreeNode node = new TreeNode(Integer.parseInt(nums[i]));
                if (isLeft) {
                    list.get(index).left = node;
                } else {
                    list.get(index).right = node;
                }
                list.add(node);
            }
            if (!isLeft) {
                index++; //when the node is on the right(!isLeft when i is even), the index should move to the next node
            }
            isLeft = !isLeft;
        }
        return root;
    }

}
```
## Clone Graph

補充：
* HashMap
public class HashMap<K,V>
extends AbstractMap<K,V>
implements Map<Key,Value>, Cloneable, Serializable. 
HashMap contains an array of Node and a node is represented as a class which contains 4 fields :
1. int hash
2. K key
3. V value
4. Node next
```java
HashMap<String, Integer> map = new HashMap<>(); 
        map.put("vishal", 10); 
        map.put("sachin", 30); 
        map.put("vaibhav", 20);
        System.out.print(map);   //{vaibhav=20, vishal=10, sachin=30}
        if (map.containsKey("vishal"))  
           Integer a = map.get("vishal"); // a = 10
        map.clear(); //empty the map
```
- complexity for get and put method is constant.
- containsKey(Object key): return True if a specified key, mapping is present in the map.
- containsValue(Object value): return true if one or more keys is mapped to a specified value.
- clone(): return a shallow copy of the mentioned hash map.

* HashSet

