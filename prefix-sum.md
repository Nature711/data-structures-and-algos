## Description
- running sum of an array up until a given index
- allows us to calculate the sum of any slice of the array very quickly

![image](https://user-images.githubusercontent.com/77217430/204070061-3b8bedfd-6f7f-4946-8574-2cefed108685.png)

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
![image](https://user-images.githubusercontent.com/77217430/204070074-57c7ca19-c2af-4127-8e65-d8da71eb4fea.png)

## Common occurrences
1. when you want to find the sum of a subarray 
2.


## Example questions
- [Find pivot index](https://github.com/Nature711/my-leetcode-notes/tree/master/724-find-pivot-index)
