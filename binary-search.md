## Keywords
- sorted array

## Key principles
- shrink the search space after each iteration
- cannot exclude potential answers during shrinking

## Templates
### 1. find exact value
- loop condition: ```low <= high```
- shrinking: ```low = mid + 1```, ```high = mid - 1```
- return ```-1 (i.e., not found)```, otherwise must return the index during loop
```
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return -1;
    }
```

### 2. find first occurance
- loop condition: ```low < high```
- shrinking: ```low = mid + 1```, ```high = mid```
  1. when ```nums[mid] < target```, we're sure the answer we want to find must be strictly to the right of mid (excluding mid), so set ```low = mid + 1```
  2. otherwise, i.e., when ```nums[mid] >= target```, the answer is to the left of mid but may also be mid itself, so set ```high = mid```
  3. because of #2, we must adjust the while loop condition to ```while (low < high)``` -- reason below
- computing mid: when there're only 2 elements, we want the left one -- use ```mid = low + (high - low) / 2```
```
   public int searchFirst(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < target) low = mid + 1;
            else high = mid;
        }
        //at this point, low == high
        return nums[mid] == low ? low : -1;
    }
```

- example:
    - [First Bad Version](https://leetcode.com/problems/first-bad-version/)

### 3. find last occurance
- loop condition: ```low < high```
- shrinking: ```low = mid```, ```high = mid - 1```
  1. when ```nums[mid] > target```, we're sure the answer we want to find must be strictly to the left of mid (excluding mid), so set ```high = mid - 1```
  2. otherwise, i.e., when ```nums[mid] <= target```, the answer is to the right of mid but may also be mid itself, so set ```left = mid```
  3. because of #2, we must adjust the while loop condition to ```while (low < high)``` -- reason below
- computing mid: when there're only 2 elements, we want the right one -- use ```mid = low + (high - low + 1) / 2```
```
   public int searchFirst(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low + 1) / 2;
            if (nums[mid] > target) high = mid - 1;
            else left = mid;
        }
        // at this point, low == high
        return nums[mid] == low ? low : -1;
    }
```

4. find closest
- loop condition: ```while (low < high - 1)``` -- make sure we're left with exactly 2 elements after exiting loop, then we can compare which one is closer to target
```
   public int searchFirst(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low < high - 1) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < target) high = mid;
            else left = mid;
        }
        //at this point, low == high - 1
        if (nums[high] < target) return high;
        else if (nums[low] > target) return low;
        else return target - nums[low] < nums[high] - target ? low : high;
    }
```

### Notes for computing ```mid```
- version 1: ```mid = low + (high - low) / 2```
  - when there're even number of elements, mid will point to the element closer to the left
    - specifically, when there're only 2 elements, mid will always point to the left one
    - this means if we have:
      - ```high = mid``` during shrinking, and
      - while loop condition ```while (low <= high)```
    - will face an infinite loop -- ```low == high == mid``` always holds
    - to avoid so, we set the while loop condition to ```while (low < high)```, and return ```low``` after exiting
```
1   2   3   4
    ^
   mid
   
 1   2  
 ^
mid
```
  
- version 2: ```mid = low + (high - low + 1) / 2```
  - when there're even number of elements, mid will point to the element closer to the right
    - specifically, when there're only 2 elements, mid will always point to the right one
    - this means if we have:
      - ```low = mid``` during shrinking, and
      - while loop condition ```while (low <= high)```
    - will face an infinite loop -- ```low == high == mid``` always holds
    - to avoid so, we set the while loop condition to ```while (low < high)```, and return ```low``` after exiting
```
1   2   3   4
        ^
       mid
   
 1   2  
     ^
    mid
```

## Binary search in rotated sorted array

Formula: If a sorted array is shifted, if you take the middle, always one side will be sorted. Take the recursion according to that rule.

1. take the middle and compare with target, if matches return.
2. if middle is bigger than left side, it means left is sorted
- 2a if [left] < target < [middle] then do recursion with left, middle - 1 (right)
- 2b left side is sorted, but target not in here, search on right side middle + 1 (left), right
3. if middle is less than right side, it means right is sorted
- 3a if [middle] < target < [right] then do recursion with middle + 1 (left), right
- 3b right side is sorted, but target not in here, search on left side left, middle -1 (right)

```
public int searchRotated(int[] nums, int target) {
    int low = 0, high = nums.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) return mid;
        if (nums[low] <= nums[mid]) {
            //left side sorted
            if (target >= nums[low] && target < nums[mid]) {
                //target within left sorted side
                high = mid - 1;
            } else low = mid + 1;
        } else {
            //right side sorted
            if (target > nums[mid] && target <= nums[high]) {
                //target within left sorted side
                low = mid + 1; 
            } else high = mid - 1;
        }
    }

    return -1;
}
```
