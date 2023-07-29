
# Overview
- 📖 Minimax is a search algorithm used in turn-based games to find the best move by exploring possible future positions.
- 🔄 The algorithm generates a game tree, with branches representing different moves, and evaluates each leaf node using a static evaluation function.
- ⚪ White (maximizing player) aims to maximize the evaluation, while black (minimizing player) tries to minimize it.

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/bf309873-3a3e-48fa-a41f-aa7596027984)

## Example: [Predict the Winner](https://leetcode.com/problems/predict-the-winner)

## Naive solution
- keeping track of the optimal score of both player at each state
- ```getScores``` returns an array, where arr[0], arr[1] denote the optimal score obtained by player1 and player2 in the current state, respectively
  
```
public boolean PredictTheWinner(int[] nums) {
        int[] res = getScores(0, nums.length - 1, 1, nums);
        return res[0] >= res[1];
    }
    
    public int[] getScores(int start, int end, int turn, int[] nums) {
        int[] res = new int[2];
        if (start == end) {
            if (turn == 1) {
                res[0] = nums[start];
                res[1] = 0;
            } else {
                res[0] = 0;
                res[1] = nums[start];
            }
            return res;
        } else {
            if (turn == 1) {
                int[] res1 = getScores(start + 1, end, 2, nums);
                int[] res2 = getScores(start, end - 1, 2, nums);
                if (res1[1] < res2[1]) {
                    res[0] = nums[start] + res1[0];
                    res[1] = res1[1];
                } else {
                    res[0] = nums[end] + res2[0];
                    res[1] = res2[1];
                }
            } else {
                int[] res1 = getScores(start + 1, end, 1, nums);
                int[] res2 = getScores(start, end - 1, 1, nums);
                if (res1[0] < res2[0]) {
                    res[1] = nums[start] + res1[1];
                    res[0] = res1[0];
                } else {
                    res[1] = nums[end] + res2[1];
                    res[0] = res2[0];
                }
            }
        }
        return res;
    }
```

## Optimized approach
- ```minimax``` returns the optimal score obtained by **player1** in the current state
- we don't need to maintain player2's score since its score is just (total - player1's score) -- the total score is fixed at each round
- if it's player1's turn in the current round, it tries to maximize the score:
   - if player1 chooses left element in the current move, it gains a score of ```nums[left]```, plus ```minimax(start + 1, end, 2)``` --- the score obtained by player1 when starting from player2 choosing from nums[start + 1, end]
   - if player1 chooses right element in the current move, it gains a score of ```nums[right]```, plus ```minimax(start, end - 1, 2)``` --- the score obtained by player1 when starting from player2 choosing from nums[start, end - 1]
   - since it's player1's turn, it will try to maximize this score --> what we return is the MAX between these 2 choices 
- if it's player2's turn in the current round, it tries to minimize the score:
   - if player2 chooses the left element, player1 is left to choose between nums[start + 1...end] and gains a score ```minimax(start + 1, end, 1)```
   - if player2 chooses the right element, player1 is left to choose between nums[start...end + 1] and gains a score ```minimax(start, end - 1, 1)```
   - since it's player2's turn, it will try to minimize player1's score --> what we return is the MIN between these 2 choices

```
    public boolean PredictTheWinner(int[] nums) {
        int total = 0;
        for (int num: nums) total += num;
        int player1Score = minimax(0, nums.length - 1, 1, nums, total);
        return player1Score >= (total - player1Score);
    }
    
    public int minimax(int start, int end, int turn, int[] nums, int total) {
        if (start == end) {
            if (turn == 1) return nums[start];
            else return 0;
        }
        if (turn == 1) {
            int left = nums[start] + minimax(start + 1, end, 2, nums, total);
            int right = nums[end] + minimax(start, end - 1, 2, nums, total);
            return Math.max(left, right);
        } else {
            int left = minimax(start + 1, end, 1, nums, total);
            int right = minimax(start, end - 1, 1, nums, total);
            return Math.min(left, right);
        }
    }
```

## DP approach
```
  public boolean PredictTheWinner(int[] nums) {
      int n = nums.length;
      int[][][] dp = new int[n][n][2];
      //dp[i][j][0] denotes the optimal score obtained by player1 if the current turn is player1
      //dp[i][j][1] denotes the optimal score obtained by player1 if the current turn is player2

      // Base case: Subproblems with size 1
      for (int i = 0; i < n; i++) {
          dp[i][i][0] = nums[i]; // Player1's turn --> pick the only choice left
          dp[i][i][1] = 0;       // Player2's turn --> Player1 got nothing
      }

      // Filling up the DP table for subproblems with **increasing size**
      for (int len = 2; len <= n; len++) { 
          for (int i = 0; i <= n - len; i++) {
              int j = i + len - 1;
              // Calculate the scores for both players for the current subarray
              int leftPlayer1 = nums[i] + dp[i + 1][j][1];          // Player 1 picks the left
              int leftPlayer2 = dp[i + 1][j][0];                    // Player 1's gain in the remaining if Player 2 chooses left 
              
              int rightPlayer1 = nums[j] + dp[i][j - 1][1];         // Player 1 picks the right
              int rightPlayer2 = dp[i][j - 1][0];                   // Player 1's gain in the remaining if Player 2 chooses right

              // Player1's turn -- choose MAX
              dp[i][j][0] = Math.max(leftPlayer1, rightPlayer1);

              // Player2's turn -- choose MIN of player1's gain from the remaining
              dp[i][j][1] = Math.min(leftPlayer2, rightPlayer2);
          }
      }

      // dp[0][n - 1] contains the scores for both players for the entire array
      return dp[0][n - 1][0] >= dp[0][n - 1][1];
  }
```

## A different perspective -- keeping track of Score Diff instead of score iteself
