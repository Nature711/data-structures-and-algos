# Traversal

- basic implementations: recursive vs. iterative

## Level order
- from left to right, level by level
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [1 2 3 4 5 6]
- implementation: queue
  - at each level, get the initial queue size (i.e., no of nodes at that level) --> determines how many times we need to poll from queue
  - for each node being polled out, add its left and right child into queue
  - complete traversal when queue is empty 
  - we can safely add the nodes of the next level to the current queue because we know exactly how many times we need to poll in order to traverse all the nodes at the current level (because we record down the queue size at each level before we start to traverse the nodes at that level)
- code:
```
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        
        if (root == null) return res;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> currLevel = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                currLevel.add(currNode.val);
                if (currNode.left != null) queue.offer(currNode.left);
                if (currNode.right != null) queue.offer(currNode.right);
            }
            res.add(currLevel);
        }
        
        return res;
    }
```

### bonus: DFS implementation
- during the DFS traversal, we maintain the results in a global array that is **indexed by the level**
    - i.e. the element ```array[level]``` would contain all the nodes that are at the same level
- global array would then be referred and updated at each step of DFS
- dfs function keeps track of the current level
    - local work: insert the current root to the specific element in global array (i.e., the list that keeps track of all nodes at that level)
    - recursive call: dfs(left), dfs(right)
```
List<List<Integer>> res = new ArrayList<>();
public List<List<Integer>> levelOrder(TreeNode root) {
    dfs(root, 0);
    return res;
}

public void dfs(TreeNode root, int level) {
    if (root == null) return;

    if (level >= res.size()) {
        List<Integer> currLevel = new ArrayList<>();
        currLevel.add(root.val);
        res.add(level, currLevel);
    } else {
        res.get(level).add(root.val);
    }
    dfs(root.left, level + 1);
    dfs(root.right, level + 1);
}
```

## Preoder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [1 2 4 5 3 6]
### recursive 
- pseudocode
``` 
print(root)
dfs(root.left)
dfs(root.right)
```
- java code:
```
List<Integer> res = new ArrayList<>();
public List<Integer> preorderTraversal(TreeNode root) {
    dfs(root);
    return res;
}

public void dfs(TreeNode root) {
    if (root == null) return;
    res.add(root.val);
    dfs(root.left);
    dfs(root.right);
}
```
### iterative
```
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            res.add(curr.val);
            curr = curr.left;
        }        
        curr = stack.pop();
        curr = curr.right;   
    }
    return res;
}
```

## Inoder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [4 2 5 1 3 6]
### recursive
``` 
dfs(root.left)
print(root)
dfs(root.right)
```
### iterative
```
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }        
        curr = stack.pop();
        res.add(curr.val);
        curr = curr.right;   
    }
    return res;
}
```

## Postorder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [4 5 2 6 3 1]
### recursive
``` 
dfs(root.left)
dfs(root.right)
print(root)
```
### iterative
```
   public List<Integer> postorderTraversal(TreeNode root) {
          List<Integer> res = new ArrayList<>();
          Stack<TreeNode> stack = new Stack<>();
          Set<TreeNode> peeked = new HashSet<>();
          TreeNode curr = root;

          while (curr != null || !stack.isEmpty()) {
              while (curr != null) {
                  stack.push(curr);
                  curr = curr.left;
              }        
              curr = stack.peek();
              if (!peeked.add(curr)) {
                  res.add(stack.pop().val);    
                  curr = null;
              } else curr = curr.right;
          }
          return res;
    }
 ```
 
 ![image](https://user-images.githubusercontent.com/77217430/205809998-b9c84680-e24f-40a3-89a6-830d014f96bc.png)


# Classic recursion problems

### find max tree depth
- current node asks its left and right child about their maxDepth, choose greater one, then add itself to depth
- base case: current node is null --> depth == 0
```
public int maxDepth(TreeNode root) {
    if (root == null) return 0; 
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### symmetric tree
- helper function takes in 2 trees and return if they're symmetric
- base case: both tree are null --> symmetric; one tree is null and the other is not --> non-symmetric
- Two trees are a mirror reflection of each other if:
    - Their two roots have the same value.
    - The right subtree of each tree is a mirror reflection of the left subtree of the other tree.
```
public boolean isSymmetric(TreeNode root) {
    if (root == null) return true;
    return isSymmetricHelper(root.left, root.right);
}

public boolean isSymmetricHelper(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null && t2 != null || t1 != null && t2 == null) return false;
    return t1.val == t2.val && isSymmetricHelper(t1.left, t2.right) && isSymmetricHelper(t1.right, t2.left); 
}
```

## invert binary tree
```
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    TreeNode oldLeft = root.left;
    root.left = invertTree(root.right);
    root.right = invertTree(oldLeft);
    return root;
}
```
# Lowest common ancestor (LCA)
- [LCA in BST](https://github.com/Nature711/my-leetcode-notes/blob/master/0235-lowest-common-ancestor-of-a-binary-search-tree/NOTES.md)
- [LCA in binary tree](https://github.com/Nature711/my-leetcode-notes/blob/master/0236-lowest-common-ancestor-of-a-binary-tree/NOTES.md)
    - [video explanation](https://www.youtube.com/watch?v=13m9ZCB8gjw)

# Height-balanced tree
## Conditions: 
1. difference between the left and right subtree for any node is not more than one: ```Math.abs(height(node.left) - height(node.right)) <= 1```
2. the left subtree is balanced: ```isBalanced(node.left```
3. the right subtree is balanced: ```isBalanced(node.right)```
## Algo 
- recursive / iterative

## Distance between tree nodes
- [All Nodes Distance K in Binary Tree](https://github.com/Nature711/data-structures-and-algos/blob/master/common-problems/distance-k-nodes-in-tree.md)
 
# N-ary tree
- similar to binary tree
- to visit its children, instead of doing ```node.left``` and ```node.right```, we do ```for (Node child: node.children) ...```
