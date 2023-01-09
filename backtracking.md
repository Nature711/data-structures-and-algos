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
  
  ## Combination Sum
  ```
  List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
         backtrack(candidates, new ArrayList<>(), 0, target, 0);
        return res;
    }
    
    public void backtrack(int[] candidates, List<Integer> currCombi, int currSum, int target, int low) {      
        if (currSum == target) {
            res.add(new ArrayList<>(currCombi));
            return;
        }
        for (int i = low; i < candidates.length; i++) {        
            int newSum = currSum + candidates[i];
            if (newSum <= target) {
                currCombi.add(candidates[i]);
                backtrack(candidates, currCombi, newSum, target, i);
                currCombi.remove(currCombi.size() - 1);
            }
        }
    }
  ```
  
  ## Subsets
  ### Subsets formed from array with distinct elements
  - use a hashset to store the current subset --> automatically eliminates duplicates
  ```
   List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        for (int size = 0; size <= nums.length; size++) backtrack(nums, new HashSet<>(), size, 0);
        return res;
    }
    public void backtrack(int[] nums, HashSet<Integer> currSet, int size, int start) {
        if (currSet.size() == size) {
            List<Integer> subset = new ArrayList<>();
            for (int n: currSet) subset.add(n);
            res.add(subset);
            return;
        }
        
        for (int i = start; i < nums.length; i++) {
            currSet.add(nums[i]);
            backtrack(nums, currSet, size, i + 1);
            currSet.remove(nums[i]);
        }
    }
  ```
  ### Subsets formed from array that may contain duplicate elements
  - sort the input array first
  - continue when: ```(i > start && nums[i] == nums[i - 1])``` -- this is not the first time we see this element; this element is a duplicate of the previous 
  ```
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        for (int size = 0; size <= nums.length; size++) backtrack(nums, new ArrayList<>(), size, 0);
        return res;
    }
    public void backtrack(int[] nums, List<Integer> currSet, int size, int start) {
        if (currSet.size() == size) {
            res.add(new ArrayList<>(currSet));
            return;
        }
        
        for (int i = start; i < nums.length; i++) {
            if (i > start && nums[i] == nums[i - 1]) continue;
            currSet.add(nums[i]);
            backtrack(nums, currSet, size, i + 1);
            currSet.remove(currSet.size() - 1);
        }
    }
  ```
