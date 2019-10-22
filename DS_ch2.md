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
```java
//Tree node:
 TreeNode {
   int val;
   TreeNode *left;
   TreeNode *right;
   TreeNode(int x) : val(x), left(NULL), right(NULL) {}
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
no key with no duplicate values.

Solution 1:
https://www.lintcode.com/problem/clone-graph/description
```java
class GraphNode {
    int val;
    List<GraphNode> neighbors;

    GraphNode(int _val) {
        val = _val;
        neighbors = new ArrayList<>();
    }
};

public class CloneGraph {
    /**
     * @param node: A  graph node
     * @return: A  graph node
     */
    public GraphNode cloneGraph(GraphNode node) {
        if (node == null) {
            return node;
        }

        // use bfs algorithm to traverse the graph and get all nodes.
        ArrayList<GraphNode> nodes = getNodes(node);  //ArrayList<GraphNode> nodes = new ArrayList<>()

        // copy nodes, store the old->new mapping information in a hash map
        HashMap<GraphNode, GraphNode> map = new HashMap<>();
        for (int i = 0; i < nodes.size(); i++) {
            map.put(nodes.get(i), new GraphNode(nodes.get(i).val)); //map裡面的node 現在只有點沒有鄰居關係
            //map put -> (key: Node(val:2, nb:[1,3]) / Node(val: 2, nb:null) )
        }

        // copy neighbors(edges)
        for (GraphNode n : nodes) {
            GraphNode newNode = map.get(n);
            for (GraphNode neighbor : n.neighbors) {
                GraphNode newNeighbor = map.get(neighbor);
                newNode.neighbors.add(newNeighbor);
            }
        }

        return map.get(node); //get key return value, which is the copied node
    }

    private ArrayList<GraphNode> getNodes(GraphNode node) {
        Queue<GraphNode> queue = new LinkedList<GraphNode>();
        HashSet<GraphNode> set = new HashSet<>();

        queue.offer(node);
        set.add(node);
        while (!queue.isEmpty()) {
            GraphNode head = queue.poll();
            for (GraphNode neighbor : head.neighbors) {
                if (!set.contains(neighbor)) {
                    set.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }

        return new ArrayList<GraphNode>(set);
    }
}
```
Solution 2:
https://leetcode.com/problems/clone-graph/
```java
class Node {
    int val;
    List<Node> neighbors;

    Node(int _val, List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}

public class Clone {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }
        HashMap<Integer, Node> map = new HashMap<>();
        map.put(node.val, new Node(node.val, new ArrayList<>())); //not yet put the neighbors in

        Queue<Node> queue = new LinkedList<>();
        queue.add(node);

        while (!queue.isEmpty()) {
            Node head = queue.poll();
            if (head.neighbors == null || head.neighbors.size() == 0) continue;
            for (Node n : head.neighbors) {
                if (!map.containsKey(n.val)) {
                    queue.add(n);
                    map.put(n.val, new Node(n.val, new ArrayList<>()));
                }
                map.get(head.val).neighbors.add(map.get(n.val));
                //connect the neighbors into the neighbors in map by getting n.val from the
            }

        }
        return map.get(node.val);
    }
}
```


