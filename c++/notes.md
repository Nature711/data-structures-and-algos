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

## Backtracking 
```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        unordered_map<char, vector<char>> map;
        map['2'] = {'a', 'b', 'c'};
        map['3'] = {'d', 'e', 'f'};
        map['4'] = {'g', 'h', 'i'};
        map['5'] = {'j', 'k', 'l'};
        map['6'] = {'m', 'n', 'o'};
        map['7'] = {'p', 'q', 'r', 's'};
        map['8'] = {'t', 'u', 'v'};
        map['9'] = {'w', 'x', 'y', 'z'};
        vector<string> res;
        string curr;
        if (digits.length() == 0) return res;
        backtrack(0, digits, curr, res, map);
        return res;
    }
    
    void backtrack(int i, string& digits, string& curr, vector<string>& res, unordered_map<char, vector<char>>& map) {
        if (i == digits.length()) {
            res.push_back(curr);
            return;
        }
        
        vector<char> choices = map[digits[i]];
        for (char choice: choices) {
            curr += choice;
            backtrack(i + 1, digits, curr, res, map);
            curr.pop_back();
        }
    }
};
```

### Declaring without initialization
```
vector<string> res;  //res is an empty vector {}
string curr;  //curr is a string of length 0, not even ""
```
### Pass by reference
- usually we pass string, vector, map by reference to avoid expensive copy
- no change on how we pass in the parameters
```
//function signature
void backtrack(int i, string& digits, string& curr, vector<string>& res, unordered_map<char, vector<char>>& map)

//invoke function
backtrack(0, digits, curr, res, map);
```
### Behavior of ```push_back()```
- ```push_back()``` creates a copy of the object and adds it to the end of the vector
- in this backtrack base case, we don't need to explicitly create a copy of the current state and add it to result; instead we can leverage the behevior of ```push_back``` since what is added to the vector is an independent copy and won't be affected by any change to the original state
```
if (i == digits.length()) {
      res.push_back(curr);
      return;
  }
```
