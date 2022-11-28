## Approaches

### Recursive

- base case: list is null
- recursive case: local work + establish connection with subproblem (solved by wishful thinking)

Ex.1, merge two sorted list (l1, l2)
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

Ex.2, reverse linked list
- base case: when there's just 0 or 1 node, return the list itself: ```if (head == null || head.next == null) return head;```
- recursive case: 
  - subproblem: reverse the sublist (the list starting from the next node) ```ListNode res = reverseList(head.next);```
  - establish connection: connect the current node with the reversed sublist
    - tricky part: the subproblem returns the **head** of the sublist; but what we want is to make the **tail** of the sublist point to our current node!
    - how to **grab the tail of the sublist**? -- don't ask the subproblem, the answer is just the next of the current node!
```
p = reverseList(head.next)                   
1 -> (2 <- 3 <- 4 <- 5)
^                    ^
head                 p

head.next.next = head;
1 <-> (2 <- 3 <- 4 <- 5)
^                     ^
head                  p

head.next = null;
1 <- (2 <- 3 <- 4 <- 5)
^                    ^
head                 p
```

### Iterative

- usually make use of ***dummy node***, and return ```dummy.next``` at last
- whenever we're accessing ```node.val```, we must make sure ```node != null```

## Example questions

- [Merge two sorted lists](https://github.com/Nature711/my-leetcode-notes/blob/master/0021-merge-two-sorted-lists/NOTES.md)
- [Reverse linked list](https://github.com/Nature711/my-leetcode-notes/blob/master/0206-reverse-linked-list/NOTES.md)
