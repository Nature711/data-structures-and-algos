## Permutations

- use a hashset to keep track of which elements are used (record their indices)
```
 List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, new ArrayList<>(), new HashSet<>());
        return res;
    }
    
    
    public void backtrack(int[] nums, List<Integer> currPerm, HashSet<Integer> used) {
        if (currPerm.size() == nums.length) {
            res.add(new ArrayList<>(currPerm));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (used.contains(i)) continue;
            currPerm.add(nums[i]);
            used.add(i);
            backtrack(nums, currPerm, used);
            currPerm.remove(currPerm.size() - 1);
            used.remove(i);
        }
    }
```

- exploiting the fact that all numbers in array are unique, we can elimiate the used of hashset to keep track of which elements are used, but instead simply check if the current permutation list contains the number or not
```
List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, new ArrayList<>());
        return res;
    }
    
    
    public void backtrack(int[] nums, List<Integer> currPerm) {
        if (currPerm.size() == nums.length) {
            res.add(new ArrayList<>(currPerm));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (currPerm.contains(nums[i])) continue;
            currPerm.add(nums[i]);
            backtrack(nums, currPerm);
            currPerm.remove(currPerm.size() - 1);
        }
    }
  ```
