## HashMap
- [my explanation](https://leetcode.com/problems/longest-consecutive-sequence/discuss/3175984/Java-solution-using-HashMap-(with-detailed-explanation)

## My approach

```
public int longestConsecutive(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for (int num: nums) set.add(num);
    int maxLen = 0;

    for (int num: nums) {
        if (!set.contains(num)) continue;
        int base = num;
        int curr = base, currLen = 1;
        while (set.contains(++curr)) {
            currLen++;
            set.remove(curr);
        }
        curr = base;
        while (set.contains(--curr)) {
            currLen++;
            set.remove(curr);
        }
        maxLen = Math.max(maxLen, currLen);
    }

    return maxLen;
}
```

### Time complexity: O(n)
- HashSet to store all the elements -- O(n)
- For each element, checks both sides of the element to find the longest consecutive sequence that contains the element -- O(m), where m is the length of the longest consecutive sequence containing the element.
-- Since **each element in nums is only processed once**, the overall time complexity is O(n).
