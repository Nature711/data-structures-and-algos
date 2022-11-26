## Maximum subarray

- challenge: to figure out when a negative number is "worth" keeping in a subarray
- idea: at index i, we can either
  - add the current element to the previous subarray sum, forming a new subarray of sum = dp[i-1] + nums[i], or
  - throw away the previous subarray and start a new subarray from the current element, with sum = nums[i]
- goal: to find the subarray with max sum
  - any subarray whose sum is positive is worth keeping
  - if the previous subarray sum is negative, we'd better discard it and start a new subarray from the current element -- doesn't matter whether our current element is positive or negative
- [Kadane's algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm)
- [visualization](https://leetcode.com/problems/maximum-subarray/solution/)
