# Overview

## Problem feature
- **optimal substructure** -- optimal solution of the current problem is based on the optimal solution to the subproblems
- **overlapping subproblem** -- solutions to the subproblems are reused during computation of many "bigger" problems

## Common approach
- define dp array 
- establish connection between the current problem ```dp[i]``` with the previously computed subproblems
- note: whenever we make use of a subproblem ```dp[j]```, must make sure it already holds the final answer (i.e., won't be updated later) 
- take note of how many subproblems each problem depends on
  - if each problem ```dp[i]``` only depends on its previous subproblem ```dp[i - 1]``` (or a constant number of its subproblems) --> can optimize space complexity to O(1)

## Fib
![image](https://user-images.githubusercontent.com/77217430/205531739-70d9fe2e-5d7d-4887-992c-94359127c905.png)

### Top-down recursion
```
  public int fib(int n) {
        if (n < 2) return n;
        return fib(n - 1) + fib(n - 2);
    }
```
![image](https://user-images.githubusercontent.com/77217430/205531609-f5dded2c-41c2-4d39-8947-9182132df562.png)

- overlapping subproblem --> use memo to reduce the overhead of recomputing the same value over and over again

### Top-down recursion with memo
```
HashMap<Integer, Integer> memo = new HashMap<>();
    public int fib(int n) {
        if (n < 2) return n;
        if (memo.containsKey(n)) return memo.get(n);
        int res = fib(n - 1) + fib(n - 2);
        memo.put(n, res);
        return res;
    }
```


### Top-down DP
```
 public int fib(int n) {
        if (n < 2) return n;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) dp[i] = dp[i - 1] + dp[i - 2];
        return dp[n];
    }
```
![image](https://user-images.githubusercontent.com/77217430/205531844-c4ba67dd-63a7-4b50-bfee-1d9536e7ce22.png)


### Bottom-up DP
```
 public int fib(int n) {
        if (n < 2) return n;
        int[] dp = new int[n + 2];
        dp[1] = 1;
        dp[1 + 1] += dp[1];
        dp[1 + 2] += dp[1];
        for (int i = 2; i < n; i++) {
            dp[i + 1] += dp[i];
            dp[i + 2] += dp[i];
        }
        return dp[n];
    }
```
![image](https://user-images.githubusercontent.com/77217430/205531864-ff3989dc-aaac-4f5a-a06e-39dc5b493e85.png)

### Space optimization
- because the current problem ```dp[i]``` only depends on 2 previous subproblems, we only need to store the result of 2 subproblems at any point of time (instead of using an array to store all previously computed subproblems)
- O(n) space --> O(1) 

```
  public int fib(int n) {
        if (n < 2) return n;
        int preprev = 0, prev = 1, curr = 0;
        for (int i = 2; i <= n; i++) {
            curr = preprev + prev;
            preprev = prev;
            prev = curr;
        }
        return curr;
    }
```

## Classical questions
- [Min cost climbing stairs](https://github.com/Nature711/my-leetcode-notes/blob/master/0746-min-cost-climbing-stairs/NOTES.md)
- [Unique paths](https://github.com/Nature711/my-leetcode-notes/blob/master/0062-unique-paths/NOTES.md)
- [House Robber](https://leetcode.com/problems/house-robber/)
- [Coin Change](https://leetcode.com/problems/coin-change/)

## DP with array
- usually, dp[i] represents some optimal (e.g., max product, min sum...) that is obtained from subarray 0...i
- to be able to draw connection between dp[i] (current problem) and previous subproblems (e.g., dp[i - 1]), we define the dp array in such a way that the optimal solution recorded in dp[i] requires using the i-th array element (i.e., nums[i] is involved)
- sometimes the globally optimal is not necessarily in dp[n], but we need to record a global optimal during computing from dp[0] to dp[n]

## Iterating over 2D array
### row wise 
```
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
     //dp[i][j] = ...
  }
}
```
![image](https://user-images.githubusercontent.com/77217430/211559553-4786bb1a-c742-4af0-809d-826f2c435a9f.png)

### row wise, half of matrix
```
for (int i = 0; i < n; i++) {
  for (int j = i; j < n; j++) {
     //dp[i][j] = ...
  }
}
```
![image](https://user-images.githubusercontent.com/77217430/211559606-110138d2-2fac-45bd-8d01-4408ce566ff9.png)

### row reverse
```
for (int i = n - 1; i >= 0; i--) {
  for (int j = i; j < n; j++) {
     //dp[i][j] = ...
  }
}
```
![image](https://user-images.githubusercontent.com/77217430/211559765-2264265a-d2cd-4bb5-a53c-567f51b1023d.png)

### diagonally, half of matrix
```
 for (int gap = 0; gap < n; gap++) {
    for (int i = 0, j = i + gap; j < n; i++, j++) {
        //dp[i][j] = ...
    }
 }
```
![image](https://user-images.githubusercontent.com/77217430/211559848-872bc164-05b9-4b80-ab4c-e20231b6b4e8.png)


### example questions 
- [Maximum product subarray](https://leetcode.com/problems/maximum-product-subarray/)
- [Longest Palindrome Substring](https://leetcode.com/submissions/detail/875423130/)
