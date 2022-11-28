## Approaches

### Recursive

- base case: list is null
- recursive case: local work + establish connection with subproblem (solved by wishful thinking)

e.g., merge two sorted list (l1, l2)
- base case: when one of the lists is null, return the other list
 ```
    if (l1 == null) return l2;
    if (l2 == null) return l1;
 ```
- recursive case: 
  - local work: compare l1.val with l2.val
  - if l1.val < l2.val
    - want to keep node l1
    - subproblem: merge the rest of l1 with l2, solved by ```merge(l1.next, l2)```
    - establish connection: ```l1.next = merge(l1.next, l2)```
    - assume subproblem is solved correctly, then simply return l1
  - if l1.val > l2.val
    - want to keep node l2
    - subproblem: merge the rest of l2 with l1, solved by ```merge(l1, l2.next)```
    - establish connection: ```l2.next = merge(l1, l2.next)```
    - assume subproblem is solved correctly, then simply return l2

### Iterative

- usually make use of ***dummy node***, and return ```dummy.next``` at last
- whenever we're accessing ```node.val```, we must make sure ```node != null```

## Example questions

- [Merge two sorted lists](https://github.com/Nature711/my-leetcode-notes/blob/master/0021-merge-two-sorted-lists/NOTES.md)
