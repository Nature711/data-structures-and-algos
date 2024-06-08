# Binary tree

## Tips to approach binary tree problem:
1. Can we get the answer by traversing the binary tree? If so, use a traversefunction with external variables to achieve it.
2. Can you define a recursive function that can deduce the answer to the original question from the answers to the sub-questions (subtrees)?
If so, write the definition of this recursive function and make full use of the return value of this function.
3. No matter which thinking mode you use, you must understand what each node of the binary tree needs to do and when it needs to be done (pre/in/post-order).

## Examples
### Maximum Depth of Binary Tree
#### Recursion 
```
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        // post-order
        return Math.max(leftMax, rightMax) + 1;
    }
```
- main logic happens at post-order position -- solution to the current problem relies on solutions to its sub-problems (i.e., maxDepth of both left & right subtree)
  
#### Traversal
```
    int max = 0;
    public int maxDepth(TreeNode root) {
        traverse(root, 0);
        return max;
    }
    
    public void traverse(TreeNode root, int currDepth) {
        if (root == null) {
            max = Math.max(max, currDepth);
            return;
        }
        // pre-order
        traverse(root.left, currDepth + 1);
        traverse(root.right, currDepth + 1);
    }
```

### Binary Tree Preorder Traversal
#### Recursion 
```
   public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;
        
        res.add(root.val);
        List<Integer> leftTree = preorderTraversal(root.left);
        List<Integer> rightTree = preorderTraversal(root.right);
        // post-order
        res.addAll(leftTree);
        res.addAll(rightTree);
        return res;
    }
```
- result of pre-order traversal of a binary tree = root node + pre-order traversal result of left subtree + pre-order traversal result of right subtree

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/3d173d33-01c4-4244-8924-2e4c7bd592ef)

#### Traversal
```
    List<Integer> res = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        traverse(root);
        return res;
    }
    
    public void traverse(TreeNode root) {
        if (root == null) return;
        // pre-order
        res.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
    }
```

## Deep understanding of pre/in/post-order 
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/b36094f1-824e-417c-899b-2e6165671550)
- pre-order: right before entering a node
- post-order: right after leaving a node
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/9912e147-1832-4e63-8b33-6c9a293eefac)

Q: What does the following function do?
```
void traverse(ListNode head) {
    if (head == null) {
        return;
    }
    traverse(head.next);
    // post-order
    print(head.val);
}
```
- solution: prints the values in the linked list in **reverse order**
  - intuition behind: `print` happens as the call stack shrinks in LIFO order -- late values get printed first

### In-order traversal of BST prints the values in sorted order

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/db2e954d-3145-40fa-bc70-3bb3bff83ba3)
- intuition behind
  - In-order traversal: left -> root -> right
  - BST: for each node N, all values in N.left are less than N.val; all values in N.right are greater than N.val -- N.val is in the middle

### Implications on writing code 
- the code in the pre-order position can only obtain the data passed by the parent node from the function parameters
- the code in the post-order position can not only obtain the parameter data, but also obtain the data passed back by the subtree through the function return value
