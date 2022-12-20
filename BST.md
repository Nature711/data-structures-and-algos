## Properties

### Definition

Binary Search Tree is a binary tree where the key in each node is 
- greater than any key in the left sub-tree, and
- less than any key in the right sub-tree

### Traversal
- **inorder** traversal (left -> root -> right) of BST yields an array **sorted in ascending order**
 - application: [Two Sum - Input as BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/solution/)

### Sorted array to BST
- find array mid --> set as root
- recursively construct left and right subtree using elements to the left of mid and to the right of mid
```
public TreeNode sortedArrayToBST(int[] nums) {
       return helper(nums, 0, nums.length - 1);
   }

   public TreeNode helper(int[] nums, int low, int high) {
       if (low > high) return null;
       int mid = low + (high - low) / 2;
       TreeNode root = new TreeNode(nums[mid]);
       root.left = helper(nums, low, mid - 1);
       root.right = helper(nums, mid + 1, high);
       return root;
   }
```


## Search

- Time complexity: O(H) 
                   = O(logN) in the case of a balanced BST
  - N = no. of nodes in BST
  - H = tree height (= logN in the case of a balanced BST)
 
- recursive:
```
public TreeNode searchBST(TreeNode root, int val) {
    if (root == null || val == root.val) return root;

    return val < root.val ? searchBST(root.left, val) : searchBST(root.right, val);
}
```
- iterative:
```
 public TreeNode searchBST(TreeNode root, int val) {
      while (root != null) {
          if (root.val == val) return root;
          if (root.val > val) root = root.left;
          else root = root.right;
      }
      return null;
 }
```

## Insertion

- Time complexity: same as search

- recursive:
```
  public TreeNode insertIntoBST(TreeNode root, int val) {
      TreeNode newNode = new TreeNode(val);

      if (root == null) return newNode;

      if (val > root.val) root.right = insertIntoBST(root.right, val);
      else root.left = insertIntoBST(root.left, val);
      return root;
  }
```

- iterative:
```
public TreeNode insertIntoBST(TreeNode root, int val) {
    TreeNode newNode = new TreeNode(val);
    if (root == null) return newNode;
    TreeNode curr = root;

    while (true) {
        if (val < curr.val && curr.left == null) {
            curr.left = newNode;
            break;
        }
        else if (val > curr.val && curr.right == null) {
            curr.right = newNode;
            break;
        }
        else if (curr.val < val) curr = curr.right;
        else if (curr.val > val) curr = curr.left;
    }

    return root;
}
```

## Sorted
