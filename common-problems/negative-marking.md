# Idea
- input properties: all array elements are positive 
- goal: find the missing numbers in single pass, without extra space
- difficulty: 
  - find missing numbers, i.e., the set difference between the "universe" (known) and input array (given)
  - requires us to establish the "universe", then iterating over input, finally see the leftovers
  - how to keep track of the "visited" / "unvisited" without extra space? 
    --> can we do it in input array directly? 
    --> what properties of input array can we leverage?
        - all elements are positive
        - each element has a unique index in the range [0, n-1]
    --> idea:
        - once we encounter a number (i.e., nums[i]), we record it as visited by: MARKING nums[nums[i] - 1] as negative 
        - insight: if a number (e.g., j), is missing from array, then nums[j - 1] will never be marked
        - eventually as we reiterate over the input array, when we see a number (e.g., nums[k]) that's still positive, this means the number (k + 1) must be missing 
        

## Example questions
### [Find all numbers disappearing from array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/submissions/)

- Negating the numbers seen in the array and use the sign of each of the numbers for finding our missing numbers
- Treating numbers in the array as indices and mark corresponding locations in the array as negative

```
public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                //see nums[i] for the first time
                nums[nums[i] - 1] = - Math.abs(nums[nums[i] - 1]); 
                //negative marking
                //take the absolute so that if it's already been marked 
                //(seen by someone else), we want to leave it negative (marked)
            } else {
                //nums[i] itself is negative, meaning it's seen by someone else
                nums[-nums[i] - 1] = - Math.abs(nums[-nums[i] - 1]);
            }
        }
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) res.add(i + 1);
        }
        
        return res;
    }
}
```
