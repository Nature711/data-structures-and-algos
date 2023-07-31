## Question
Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.
You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character

## Solution
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/1869a906-3b42-49df-a187-6fa4aceb7555)
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/5f76a9f2-6026-4685-87c5-4ff6a923442a)
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/60c2a964-d60c-487e-b663-51c95675d708)

- sequence of computation: left --> right (j++); top --> bottom (i++)
- i.e., dp[i][j] relies on dp[i - 1][j] (1 row above), dp[i][j - 1] (1 row to left), dp[i - 1][j - 1] (diagonally left)

## Related questions
- [Minimum ASCII delete sum for two strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)
