## [Binary tree max path sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

![image_1588177330](https://user-images.githubusercontent.com/77217430/215255691-06046546-55be-412a-a97a-dc5ad9be7932.png)
![image_1588177335](https://user-images.githubusercontent.com/77217430/215255694-5d205dc2-52fb-4e4e-be7f-beac1b90c8db.png)
![image_1588177343](https://user-images.githubusercontent.com/77217430/215255699-8005df8f-08fc-40e3-a1ed-dbf0194ad9d1.png)
![image_1588177356](https://user-images.githubusercontent.com/77217430/215255700-5be3d79a-a3e0-4d2c-bb57-6cf5f47f41de.png)
![image_1588177373](https://user-images.githubusercontent.com/77217430/215255704-ab83847d-d08e-4c60-8ab6-bcf5c8f970f5.png)

- note the difference between:
  - looking for the max path involving the current node in process -- this path can go through the root (i.e., "bent")
    - update of global max: ```globalMax = Math.max(globalMax, root.val + gainFromLeft + gainFromRight);```
    
    ![image](https://user-images.githubusercontent.com/77217430/215256675-7306b3c0-c0b4-4e70-bd96-26d5030d3bec.png)

  - what we return for the node which starts the recursion stack -- because we can either go left or right from current node, but not both
    - max path going down current node: ```root.val + Math.max(gainFromLeft, gainFromRight);```
     
    ![image](https://user-images.githubusercontent.com/77217430/215256701-e5d88db3-c36c-4586-8e59-fe427364c44d.png)

```
int globalMax = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        maxPathFromNode(root);
        return globalMax;
    }
    
    public int maxPathFromNode(TreeNode root) { //sum of the max path we can get involving root
        if (root == null) return 0;
        
        int gainFromLeft = Math.max(maxPathFromNode(root.left), 0);
        int gainFromRight = Math.max(maxPathFromNode(root.right), 0);
        
        globalMax = Math.max(globalMax, root.val + gainFromLeft + gainFromRight);
        
        return root.val + Math.max(gainFromLeft, gainFromRight);
    }
```

## [Diamter of binary tree](https://leetcode.com/problems/diameter-of-binary-tree/)

![image](https://user-images.githubusercontent.com/77217430/215257188-efd85cf9-ba46-4510-89c7-968a2b44b534.png)

- note the difference between:
  - the global variable ```globalMax``` -- updated during each recursive call: what's the max diameter in whole tree
  - the return value from recursive call -- returned to the calling root node ```root```: what's the max depth of the subtree rooted at ```root```
  - 
```
int globalMax = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        maxDepthFromNode(root);
        return globalMax; 
    }
    
    public int maxDepthFromNode(TreeNode root) {
        if (root == null) return 0;
        
        int maxFromLeft = maxDepthFromNode(root.left);
        int maxFromRight = maxDepthFromNode(root.right);
        
        globalMax = Math.max(globalMax, maxFromLeft + maxFromRight);
        
        return 1 + Math.max(maxFromLeft, maxFromRight);
    }

```
