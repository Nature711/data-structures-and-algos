
# [Find the Punishment Number of an Integer](https://leetcode.com/problems/find-the-punishment-number-of-an-integer/description/?envType=daily-question&envId=2025-02-15)
- related topics: recursion (divide n conquer), memoization, dp
## Recursion (without memo)
```
class Solution {
    public int punishmentNumber(int n) {
        int res = 0;
        for (int i = 1; i <= n; i++) {
            if (isPunishment(i)) res += i * i;
        }
        return res;
    }

    boolean isPunishment(int n) {
        int square = n * n;
        List<Integer> digitList = new ArrayList<>();
        while (square > 0) {
            int digit = square % 10;
            square /= 10;
            digitList.add(digit);
        }
        Collections.reverse(digitList);
        List<String> partitions = generatePartitions(digitList, 0, digitList.size() - 1);
        for (String partition: partitions) {
            String[] parts = partition.split(",");
            int sum = 0;
            for (String p: parts) {
                sum += Integer.valueOf(p);
            }
            if (sum == n) return true;
        }
        return false;
    }

    List<String> generatePartitions(List<Integer> digitList, int start, int end) {
        int mid = (end - start) / 2;
        List<String> res = new ArrayList<>();
        if (start > end) {
            res.add("");
            return res;
        }
        if (start == end) {
            res.add(Integer.toString(digitList.get(start)));
            return res;
        }
        List<String> part1 = generatePartitions(digitList, start, mid);
        List<String> part2 = generatePartitions(digitList, mid + 1, end);
        
        for (String sp1: part1) {
            for (String sp2: part2) {
                res.add(sp1 + sp2);
                res.add(sp1 + "," + sp2);
            }
        }
        return res;
    }
}
```
## Backtracking
### without memo
```
class Solution {
    public int punishmentNumber(int n) {
        int res = 0;
        for (int num = 1; num <= n; num++) {
            int square = num * num;
            String currString = String.valueOf(square);
            if (findPartitions(0, 0, currString, num)) {
                res += square;
            }
        }
        return res;
    }

    boolean findPartitions(int currIdx, int currSum, String numString, int targetSum) {
        if (currSum > targetSum) {
            return false;
        }
        if (currIdx == numString.length()) {
            return currSum == targetSum;
        }
        for (int i = currIdx + 1; i <= numString.length(); i++) {
            String left = numString.substring(currIdx, i);
            int leftValue = Integer.parseInt(left);
            String rem = numString.substring(i);
            if (findPartitions(i, currSum + leftValue, numString, targetSum)) {
                return true;
            }
        }
        return false;
    }
}
```
### with memo
- memo[i][j]: if it's possible to for numString[currIdx...end] to be partitioned and summed up to targetSum
```
class Solution {
    public int punishmentNumber(int n) {
        int res = 0;
        for (int num = 1; num <= n; num++) {
            int square = num * num;
            String currString = String.valueOf(square);
            int[][] memo = new int[currString.length()][num + 1];
            for (int[] row : memo) {
                java.util.Arrays.fill(row, -1);
            }
            if (findPartitions(0, 0, currString, num, memo)) {
                res += square;
            }
        }
        return res;
    }

    boolean findPartitions(int currIdx, int currSum, String numString, int targetSum, int[][] memo) {
        if (currSum > targetSum) {
            return false;
        }
        if (currIdx == numString.length()) {
            return currSum == targetSum;
        }
        if (memo[currIdx][currSum] != -1) { 
            return memo[currIdx][currSum] == 1;
        }
        for (int i = currIdx + 1; i <= numString.length(); i++) {
            String left = numString.substring(currIdx, i);
            int leftValue = Integer.parseInt(left);
            String rem = numString.substring(i);
            if (findPartitions(i, currSum + leftValue, numString, targetSum, memo)) {
                memo[currIdx][currSum] = 1;
                return true;
            }
        }
        memo[currIdx][currSum] = 0;
        return false;
    }
}
```
