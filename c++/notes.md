## Pointers
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

## Arrays
1. Initializing fixed size array -- array size must be known at **compile time**
```
//array size not declared at const -- not working
int m = 2;
int nums[m] = {0}; //error: variable-sized object may not be initialized

//array size known at compile time -- ok
const int n = 2;
int nums[n] = {0};
```

2. Initializing array with default values
```
//1D
const int m = 2;
int nums[m] = {0};

//2D
const int m = 2, n = 3;
int nums[m][n] = {{0}};
```
