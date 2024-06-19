# BST

## Definition
Binary Search Tree is a binary tree where the key in each node is 
- greater than any key in the left sub-tree, and
- less than any key in the right sub-tree

### In-order traversal of BST prints the values in sorted order (ascending)
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/db2e954d-3145-40fa-bc70-3bb3bff83ba3)
- intuition behind
  - In-order traversal: left -> root -> right
  - BST: for each node N, all values in N.left are less than N.val; all values in N.right are greater than N.val -- N.val is in the middle
- if we want the values to be in descending order, simply change the order of traversal to: right --> root --> left
```
// print the BST values in ascending order
void traverse(TreeNode root) {
    if (root == null) return;
    traverse(root.left);
    print(root.val);
    traverse(root.right);
}

// print the BST values in descending order
void traverse(TreeNode root) {
    if (root == null) return;
    traverse(root.right);
    print(root.val);
    traverse(root.left);
}
```
- interesting application: [Two Sum - Input as BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/solution/)

#### Convert BST to Greater Tree
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/db39ae34-9bcd-49ce-a8fe-ef4b69f302ae)

- greatest element -- no element greater than it --> updated sum = original value + 0
- elements in the middle --> updated sum = original value + sum of elements greater than it
- traversing fromt the greastest element, keeping track of the sum of values seen so far, adding the sum to the next node 
```
TreeNode convertBST(TreeNode root) {
    traverse(root);
    return root;
}

int sum = 0;
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    traverse(root.right);
    // update sum
    sum += root.val;
    // convert BST to GST
    root.val = sum;
    traverse(root.left);
}
```

#### [Sorted array to BST](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/?envType=study-plan&id=level-2)
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

#### [K-th smallest element in BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/?envType=study-plan&id=level-2)
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
- basic framework:
  1. find the desired node (to be inserted / deleted)
  2. perform the operation (no-op in the case of just searching)
- practice recursive mindset: instead of maintaining 2 pointers (one to the current position at which op is going to be performed,
the other to the parent of the current pos), by having the recursive function returning the root of the subtree containing the modification,
we then just need to perform necessary changes on the current root to link it to the subtree
  - insertion base case: reaching null --> return a new node with the value to be inserted -- recursion process will take care of connecting this node properly to its parent
  - deletion base case: reaching the node to be deleted --> 3 cases: node has no child, 1 child, or 2 chilren -- take care of them differently, return the new root after change
  
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
      if (root == null) return new TreeNode(val);

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
## Deletion
- Time complexity: same as search
  
- recursive:
```
 public TreeNode deleteNode(TreeNode root, int key) {
      if (root == null) return root;

        if (root.val == key) {
               if (root.left == null && root.right == null) return null;
                if (root.left == null && root.right != null) return root.right;
                if (root.left != null && root.right == null) return root.left;

                TreeNode newRoot = root.left;
                TreeNode pos = newRoot;
                while (pos.right != null) pos = pos.right;
                pos.right = root.right;
                return newRoot;

        }

        if (root.val < key) root.right = deleteNode(root.right, key);
        else root.left = deleteNode(root.left, key);
        return root;
}
```
- iterative: code is too nasty, check [here](https://leetcode.com/submissions/detail/1292999712/)

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
