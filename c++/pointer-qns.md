## Notes
1. ```null``` vs ```nullptr```
- in java we don't have the notion of pointer, so we use ```null``` to represent uninitialized variables
- in c++ however, we should use ```nullptr``` instead to represent an uninitialized pointer variable
- e.g.,
  - java: 
  ```
   public boolean isSymmetric(TreeNode root) {
      if (root == null) return true;
      ...
  ```
  - c++:
  ```
  bool isSymmetric(TreeNode* root) {
      if (root == nullptr) return true;
      ...
  ```
