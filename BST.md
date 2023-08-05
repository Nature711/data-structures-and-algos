## Definition

Binary Search Tree is a binary tree where the key in each node is 
- greater than any key in the left sub-tree, and
- less than any key in the right sub-tree

## Traversal
- **inorder** traversal (left -> root -> right) of BST yields an array **sorted in ascending order**
 - application: [Two Sum - Input as BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/solution/)

### [Sorted array to BST](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/?envType=study-plan&id=level-2)
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

### [K-th smallest element in BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/?envType=study-plan&id=level-2)
```
 int res = -1, count;
 public int kthSmallest(TreeNode root, int k) {
     count = k;
     preorder(root);
     return res;
 }

 public void preorder(TreeNode root) {
     if (res != -1 || root == null) return;
     preorder(root.left);
     count--;
     if (count == 0) res = root.val;
     preorder(root.right);
 }
```


# Operations
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

### A point to note on base case
- this function returns a list of BST, each of which is constructed using all unique values in the range [low, high]
- very tempting to write the base case as:
  ```
  if (low == high) {
      res.add(new TreeNode(low));
      return res;
  }
  ```
  - consider calling ```helper(1, 2)```
    - for ```i == 1``` --> recursively calls: 
      - helper(1, 0) --> return an empty list (leftTrees)
      - helper(2, 2) --> return a list with one node 2 (rightTrees)
      - for each leftTree: leftTrees --> this part won't even run since leftTrees is an empty list
      - nothing added to ```res```
    - for ```i == 2``` --> recursively calls: 
      - helper(1, 1) --> return a list with one node 1 (leftTrees)
      - helper(3, 2) --> return an empty list (rightTrees)
      - for each leftTree: leftTrees, for each rightTree: rightTrees --> won't run inner for loop since rightTrees is an empty list
      - nothing added to ```res```
     - finally we got nothing...
 - thus we need to use ```if (low > high)``` to handle base case, and return a **list** containing a single null value (instead of just null!!!) so that both outer and inner for loop will run
```
public List<TreeNode> helper(int low, int high) {
      List<TreeNode> res = new ArrayList<>();
      if (low > high) {
          res.add(null);
          return res;
      }

      for (int i = low; i <= high; i++) {
          List<TreeNode> leftTrees = helper(low, i - 1);
          List<TreeNode> rightTrees = helper(i + 1, high);
          for (TreeNode leftTree: leftTrees) {
              for (TreeNode rightTree: rightTrees) {
                  TreeNode root = new TreeNode(i);
                  root.left = leftTree;
                  root.right = rightTree;
                  res.add(root);
              }
          }
      }
      return res;
  }
```
