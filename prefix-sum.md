## Description
- running sum of an array up until a given index
- allows us to calculate the sum of any slice of the array very quickly

## Implementations
```
   public int[] prefixSum(int[] nums) {
        int n = nums.length;
        int[] prefixSums = new int[n + 1];
        prefixSums[0] = 0;
        for (int i = 1; i < n + 1; i++) prefixSums[i] = prefixSums[i - 1] + nums[i - 1]; //calculate the i-th number in the running sum from the (i-1)-th number
        return prefixSums;
    }
    
    // prefixSums[j + 1] - prefixSums[i] == sum of subarray[i...j] (inclusive on both ends)
```

## Common occurrences
1. when you want to find the sum of a subarray 
2.


## Example questions
- [Find pivot index](https://github.com/Nature711/my-leetcode-notes/tree/master/724-find-pivot-index)
