## Notes
- always remember who is the caller (of the recursive function)
  -> because this is to whom this function is returning 

## Diameter of Binary Tree
- goal: find the longest distance between any pair of nodes in the whole tree

```
            1
          /   \
         2     3
        / \
       4   5
            \
             6
            / \
          ...  ...
```

- observation:
  - the longest path MAY NOT pass through root
  - define ```dfs(TreeNode root)``` to be a function that takes in a node ```root``` and returns the max no of edges down this node (either to left or right)
    - this is done by taking the max of (dfs(root.left), dfs(root.right)), plus 1 to connect it with current node
    - curr diameter from this node is dfs(root.left) + dfs(root.right)
    - try to update global max diameter
  
 ```
    int maxDiameter = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        dfs(root);
        return maxDiameter;
    }
    
    public int dfs(TreeNode root) {
        if (root == null) return 0;
        int left = dfs(root.left), right = dfs(root.right);
        maxDiameter = Math.max(maxDiameter, left + right);
        return 1 + Math.max(left, right);
    }
```
