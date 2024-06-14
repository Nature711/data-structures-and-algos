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
  - top-down processing
- post-order: right after leaving a node
  - bottom-up processing
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

## More examples
### Diameter of Binary Tree
- Diameter of a binary tree (rooted at N) = maxDepth(N.left) + maxDepth(N.right)
- Max diameter of a binary tree -- max diameter among all subtrees (including itself)
```
int maxDiameter = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return maxDiameter;
    }
    int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        // post-order
        int myDiameter = leftMax + rightMax;
        maxDiameter = Math.max(maxDiameter, myDiameter);

        return 1 + Math.max(leftMax, rightMax);
    }
```
- logic of calculating diameter of current tree is done in post-order position -- can make use of the return values of the subproblems (i.e., max depth of subtree)

### Invert Binary Tree
#### Recursion
```
public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        
        TreeNode leftInv = invertTree(root.left);
        TreeNode rightInv = invertTree(root.right);
        root.left = rightInv;
        root.right = leftInv;
        return root;
    }
```
#### Traversal
```
    public TreeNode invertTree(TreeNode root) {
        traverse(root);
        return root;
    }
    
    public void traverse(TreeNode root) {
        if (root == null) return;
        TreeNode tmpLeft = root.left;
        root.left = root.right;
        root.right = tmpLeft;
        // pre-order
        traverse(root.left);
        traverse(root.right);
    }
```
- main logic can be placed in either pre or post-order position
  - pre-order: top-down -- exchanging the entire left & right subtree first, then recursively going down
  - post-order: bottom-up -- exchanging leaf nodes first, then going up to exchange the parent

## Construction
- common strategy: tree = construct root + construct left subtree (root.left) + construct right subtree (root.right)

### Construct binary tree from traversal
- requirements:
  - all values in the tree are unique
  - traversal doesn't include NULL values
#### pre + in-order traversal
- pre: root -> left -> right; in: left -> root -> right
- first value in preorder -- root --> locate this value in inorder --> whatever to its left is left subtree, to its right is right subtree
- by knowing the size of the left & right subtree --> know where the right subtree start from in preorder as well

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/5b8f1f65-b27b-41f7-b7b8-914d0c92f96a)
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/08a301e2-5f0c-41be-a385-64b1da956af3)

```
    HashMap<Integer, Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);
        return build(preorder, inorder, 0, 0, inorder.length - 1);
    }
    
    TreeNode build(int[] preorder, int[] inorder, int preLow, int inLow, int inHigh) {
        if (inLow > inHigh) return null;
        
        int rootVal = preorder[preLow];
        TreeNode root = new TreeNode(rootVal);
        int inorderIdx = map.get(rootVal);
        root.left = build(preorder, inorder, preLow + 1, inLow, inorderIdx - 1);
        int leftTreeSize = inorderIdx - inLow;
        root.right = build(preorder, inorder, preLow + leftTreeSize + 1, inorderIdx + 1, inHigh);
        return root;
    }
```
#### in + post-order traversal
- in: left -> root -> right; post: left -> right -> root
- last value in postorder -- root --> locate this value in inorder --> whatever to its left is left subtree, to its right is right subtree
- by knowing the size of the left & right subtree --> know where the left subtree start from in postorder as well

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/713ee5d8-42ef-4f1d-bca8-58202d6f1e45)
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/b9900525-d738-48b0-8881-4edf7dc49e95)

#### pre + post-order traversal
- impossible -- unable to know the size of left & right subtree --> don't know where to recurse from
  
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/6188330f-bef0-43f8-8e67-5061b3e5ce49)

### Summary: using traversal to construct binary tree
- if NULL values are NOT included in traversal && all values in the tree are unique
  - pre + in-order, or
  - in + post-order
- if NULL values are included in traversal
  - pre-order
  - post-order
  - level-order

### Serialization
- main challenge: deserialization (from string back to tree) -- to ensure uniqueness of result
- based on the idea of using traversal to (uniquely) construct binary tree

