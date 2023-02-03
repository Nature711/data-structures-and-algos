## Usage
- when unique keys are available for the data we want to store
- when we don't care about the order of data -- map is unordered by default
- key-value semantics -- store items based on a key that can be used to retrieve the item

## Example questions
- [Two Sum](https://leetcode.com/problems/two-sum/)
- [Maximum size subarray sum equals k](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)
  - Compute the prefix sum array where psum[i] is the sum of all the elements from 0 to i.
  - At each index i, the sum of the prefix is psum[i], so we are searching for the index x where psum[x] = psum[i] - k. The subarray [x + 1, i] will be of sum k.
  - **Use a hashmap to get the index x efficiently or to determine that it does not exist.**
