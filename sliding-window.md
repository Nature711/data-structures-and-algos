### Goal
- aims to reduce the use of nested loop --> replaced by a single loop

## How to use
1. Find the size of window required (can be fixed / variable)
2. Compute the result for the 1st window
3. Use a loop to slide the window by 1, and keep computing the result window by window

## Examples
- Given an array of integers of size ‘n’, calculate the maximum sum of ‘k’ consecutive elements in the array

- Naive approach: nested for loop
```
const getMaxSumOfFiveContiguousElements = (arr) => {
  let maxSum = -Infinity;
  let currSum;

  for (let i = 0; i <= arr.length - 5; i++) {
    currSum = 0;
    for (let j = i; j < i + 5; j++) currSum += arr[j];
    maxSum = Math.max(maxSum, currSum);
  }
  return maxSum;
};
```
![image](https://user-images.githubusercontent.com/77217430/206093652-6e789012-4b92-4a86-95c0-376e234c5d28.png)


- Sliding window approach
```
const getLargestSumOfFiveConsecutiveElements = (arr) => {
  let currSum = 0;
  for (let i = 0; i <= 4; i++) currSum += arr[i];
  let largestSum = currSum;

  for (let i = 1; i <= arr.length - 5; i++) {
    currSum -= arr[i - 1]; // subtract element to the left of curr window
    currSum += arr[i + 4]; // add last element in curr window
    largestSum = Math.max(largestSum, currSum);
  }
  return largestSum;
};
```
![image](https://user-images.githubusercontent.com/77217430/206093868-6b9dea8f-bb0b-4086-88c7-0806b0927f62.png)

## Related articles
- [How to solve sliding window problems](https://medium.com/outco/how-to-solve-sliding-window-problems-28d67601a66)

## Example questions
- [ Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/solution/)
