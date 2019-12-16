159. Longest Substring with At Most Two Distinct Characters
https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/submissions/
Given a string s , find the length of the longest substring t  that contains at most 2 distinct characters.
```
Example 1:

Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
```


Approach 1: Sliding Window

```java
class Solution {
  public int lengthOfLongestSubstringTwoDistinct(String s) {
    int n = s.length();
    if (n < 3) return n;

    // sliding window left and right pointers
    int left = 0;
    int right = 0;
    // hashmap character -> its rightmost position 
    // in the sliding window
    HashMap<Character, Integer> hashmap = new HashMap<Character, Integer>();

    int max_len = 2;

    while (right < n) {
      // slidewindow contains less than 3 characters
      if (hashmap.size() < 3)
        hashmap.put(s.charAt(right), right++);

      // slidewindow contains 3 characters
      if (hashmap.size() == 3) {
        // delete the leftmost character
        int del_idx = Collections.min(hashmap.values());
        hashmap.remove(s.charAt(del_idx));
        // move left pointer of the slidewindow
        left = del_idx + 1;
      }

      max_len = Math.max(max_len, right - left);
    }
    return max_len;
  }
}

```

876. Middle of the Linked List

Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.
```
Example 1:

Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
Example 2:

Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
```

Approach 1: Output to Array
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode[] A = new ListNode[100];
        int t = 0;
        while (head.next != null) {
            A[t++] = head;
            head = head.next;
        }
        return A[t / 2];
    }
}
```



15. 3Sum

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```java
public class Solution {
    /**
     * @param nums : Give an array numbers of n integer
     * @return : Find all unique triplets in the array which gives the sum of zero.
     */
    List<List<Integer>> results = new ArrayList<>();
    
    // This is soring results using 1st number as 1st key, 2nd number as 2nd key...
    class ListComparator<T extends Comparable<T>> implements Comparator<List<Integer>> {
      @Override
      public int compare(List<Integer> a, List<Integer> b) {
        for (int i = 0; i < Math.min(a.size(), b.size()); i++) {
          int c = a.get(i).compareTo(b.get(i));
          if (c != 0) {
            return c;
          }
        }
        return Integer.compare(a.size(), b.size());
      }
    }
    
    public List<List<Integer>> threeSum(int[] A) {
        if (A == null || A.length < 3) {
            return results;
        }
        
        Arrays.sort(A);
        // enumerate c, the maximum element
        int i;
        int n = A.length;
        for (i = 2; i < n; ++i) {
            // c is always the last in a bunch of duplicates
            if (i + 1 < n && A[i + 1] == A[i]) {
                continue;
            }
            
            // want to find all pairs of A[j]+A[k]=-A[i], such that
            // j < k < i
            twoSum(A, i - 1, -A[i]);
        }
        
        Collections.sort(results, new ListComparator<>());
        
        return results;
    }
    
    // find all unique pairs such that A[i]+A[j]=S, and i < j <= right
    private void twoSum(int[] A, int right, int S) {
        int i, j;
        j = right;
        for (i = 0; i <= right; ++i) {
            // A[i] must be the first in its duplicates
            if (i > 0 && A[i] == A[i - 1]) {
                continue;
            }
            
            while (j > i && A[j] > S - A[i]) {
                --j;
            }
            
            if (i >= j) {
                break;
            }
            
            if (A[i] + A[j] == S) {
                List<Integer> t = new ArrayList<>();
                t.add(A[i]);
                t.add(A[j]);
                t.add(-S); // t.add(A[right+1])
                results.add(t);
            }
        }
    }
}
```
47. Permutations II 带重复元素的排列 

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:

Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```java
public class Solution {
    /*
     * @param :  A list of integers
     * @return: A list of unique permutations
     */
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        if (nums == null) {
            return results;
        }
        
        Arrays.sort(nums);
        dfs(nums, new boolean[nums.length], new ArrayList<Integer>(), results);
        
        return results;
    }
    
    private void dfs(int[] nums,
                     boolean[] visited,
                     List<Integer> permutation,
                     List<List<Integer>> results) {
        if (nums.length == permutation.size()) {
            results.add(new ArrayList<Integer>(permutation));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) {
                continue;
            }
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) {
                continue;
            }
            
            permutation.add(nums[i]);
            visited[i] = true;
            dfs(nums, visited, permutation, results);
            visited[i] = false;
            permutation.remove(permutation.size() - 1);
        }
    }
 };
```

Find the median of integers using both maxHeap and minHeap
```java
public class FindMedian{

  private PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(20, Collections.reverseOrder());
  private PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>(20);
  
    public void addNumber(int value) {
        if (maxHeap.size() == minHeap.size()) {
            if (minHeap.peek() != null && minHeap.peek() < value) {
                maxHeap.offer(minHeap.poll());
                minHeap.offer(value);
            } else {
                maxHeap.offer(value);
            }

        } else {
            if (maxHeap.peek() != null && maxHeap.peek() > value) {
                minHeap.offer(maxHeap.poll());
                maxHeap.offer(value);
            } else {
                minHeap.offer(value);
            }

        }
    }

    /**
     * Returns the median value of the added values.
     * If maxHeap and minHeap are of different sizes, then maxHeap must have one extra element.
     *
     * @return median value
     */
    public double getMedian() {
        if (maxHeap.isEmpty()) {
            return -1;
        }
        if (maxHeap.size() == minHeap.size()) {
            return (minHeap.peek() + maxHeap.peek()) / 2.0;
        }
        return maxHeap.peek();
    }
}
```
