# Two pointers

## Technique: fast & slow pointers
- commonly used to modify array in place

### Remove duplicates from sorted array
- example: [1,1,2,2] --> [1,2,_,_]
- idea
  - slow pointer points to the first occurence of a new element encountered
  - fast pointer "probes forward" to detect the next new element
  - as soon as we find a new element (pointed to by fast), exchange it with slow
  - invariant: all elements before slow pointer are unique (and sorted) 
- illustration
```
1. nums[slow] == nums[fast] --> just increment fast
[1, 1, ..., 1, 2, 2..]
 ^          ^
 s          f


2. nums[slow] != nums[fast] 
[1, 1, ..., 1, 2, 2..]
 ^             ^
 s             f

- increment slow to point to the index at which the next new element should be placed
[1, 1, ..., 1, 2, 2..]
    ^          ^
    s          f

- exchange fast & slow -- so that all elements before & inclduing slow are unique
[1, 2, ..., 1, 1, 2..]
    ^          ^
    s          f
```
- code 
```
public int removeDuplicates(int[] nums) {
    int slow = 0, fast = 0;
    while (fast < nums.length) {
        if (nums[slow] != nums[fast]) { // case 2
            slow++;
            nums[slow] = nums[fast];
        }
        fast++; // case 1
    }
    return slow + 1; // number of unique elements = slow index + 1
}
```

### Move zeros
- goal: to move all the zeros in an array to the end
  - to rephrase: to move all the non-zero elements to the front 
- example: [0,1,0,3,12] --> [1,3,12,0,0]
- idea:
  - slow pointer points to the index at which the next non-zero element should be placed
  - fast pointer finds the next non-zero element
  - as soon as we find a new non-zero element (pointed to by fast), exchange it with slow
  - note: it doesn't matter what the element at fast is after the exchange, since the element is already been inserted to the right place (where slow points to) and later may be used to hold other elements
  - invariant: all elements before slow pointer are non-zero
```
public int removeElements(int[] nums, int val) {
    int slow = 0, fast = 0;
    while (fast < nums.length) {
        if (nums[fast] != val]) { 
            slow++;
            nums[slow] = nums[fast]; 
        }
        fast++; 
    }
    return slow + 1; // index at which the first zero should start
}

public void moveZeroes(int[] nums) {
        int p = removeElements(nums, 0);
        for (int i = p; i < nums.length; i++) nums[i] = 0; // fill the remaining slots with 0, effectively "moving" the zeros to the end 
}
```

