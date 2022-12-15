## Algos

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

### Divide & Conquer

- splits the linked list into 2 equal halves; recursively solve each half
- note: after finding the middle node, we need to ***EXPLICLITY cut the list into 2 halves by setting the first half's last node's next pointer to NULL***

- example: [Sort list](https://github.com/Nature711/my-leetcode-notes/blob/master/0148-sort-list/NOTES.md)

## Fast and slow pointer 

- idea: use 2 two pointers -- fast & slow, to traverse the linked list; fast traverses twice as fast as slow
- implementation
```
while (fast != null && fast.next != null) {
   fast = fast.next.next;
   slow = slow.next;
}
```
- applications:
  - find middle node -- [Middle of the linked list](https://leetcode.com/problems/middle-of-the-linked-list/solution/)
  - cycle detection -- [Linked list cycle](https://github.com/Nature711/my-leetcode-notes/blob/master/Linked-list-cycle.md)

## Dummy node

- treating the dummy head with the invariant that it is always pointing to the current correct answer makes **dealing with edge cases** in linked lists a lot easie
- application
  - [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)
  - [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

### N-th node
- find n-th node from end of list
 ```
 no of nodes in list = 1 = N 

  1
  ^            
 f,s

 while (n-->0) move fast forward

  1  null
  ^   ^
  s   f

 return slow (1)
 ```
- simply keep fast ptr N nodes aways from slow ptr
```
public ListNode NthFromEnd(ListNode head, int n) {

    ListNode slow = head, fast = head; //fast and slow starts at the same node

    while (n-- > 0) fast = fast.next;
    
    //now fast and slow are N nodes apart

    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    //as fast reaches the end, by the fact that fast and slow is kept N nodes apart, now slow must be at the N-th node from the end

    return slow; 
}
```
- remove the n-th node from end of list
```
no of nodes in list = 1 = N 

 d ------> 1
 ^         ^      
 s         f

while (n-->0) move fast forward

 d ------> 1  null 
 ^             ^      
 s             f

to remove 1, set s.next = s.next.next

 d ------->  null 
 ^            ^      
 s            f
          s.next.next
```
  - in order to do so, we need to reference previous node of the N-th node from the end (found in the example above)
  - need to use a dummy node to account for edge case: when the node to be removed is the only node in the list
  - to do so, we simply adjust the slow pointer to start at the previous node of fast (previous node of head --> dummy)
  - i.e., we're keep fast and slow (N+1) nodes apart from each other, so as fast reaches the end, slow is at the (N+1)-th node from the end
    - A: the (N+1)-th node from the end (slow)
    - B: the N-th node from the end
    - C: the (N-1)-th node from the end
    - B = A.next; C = B.next = A.next.next;
  - to remove B, we simply set A's next to be C -- ```slow.next = slow.next.next```
```
public ListNode removeNthFromEnd(ListNode head, int n) {

     ListNode dummy = new ListNode();
     dummy.next = head;
     ListNode slow = dummy, fast = head; //slow starts 1 node before fast

     while (n-- > 0) fast = fast.next;

     while (fast != null) {
         slow = slow.next;
         fast = fast.next;
     }
     
     //as fast reaches the end, slow is at the (N+1)-th node from the end

     slow.next = slow.next.next; //remove the N-th node

     return dummy.next;
 }
```
- find the middle node
 1. middle node is the ceil(n/2)-th node from the start
   ```
   1 --------> 2
   ^            
  f,s
  
   1 --------> 2  null
               ^   ^
               s   f
   return slow (2) as mid
  
   ```
   - if we have 2 nodes, the right one is considered as the mid
   ```
    public ListNode findMiddleFloor(ListNode head) {
         ListNode fast = head, slow = head;
         while (fast != null && fast.next != null) {
             fast = fast.next.next;
             slow = slow.next;
         }
         return slow;
     }
   ```
  
 2. middle node is the floor(n/2)-th node from the start
   - if we have 2 nodes, the left one is considered as the mid
   ```
   d ---------> 1 --------> 2
   ^            ^          
   s            f    
   
   d ---------> 1 --------> 2  null
                ^               ^
                s               f
   return slow (1) as mid
   ```
   - need to use a dummy node, so that slow starts at 1 node before fast
   ```
    public ListNode findMiddleFloor(ListNode head) {
       ListNode dummy = new ListNode();
       dummy.next = head;
       ListNode fast = head, slow = dummy;
       while ...
       return slow;
   }
   ```
   
3. (bonus) remove the middle node (ceiling as mid)
  - if we consider ceiling as mid, then to remove the mid node, we need to refer to the node before the right-biased mid,
    which is essentially the left-biased mid
  - thus we can simply reuse the code for case 2
 ```
 d --------> 1
 ^           ^ 
 to refer    to remove

 d --------> 1  -------> 2 --------> 3 
             ^           ^ 
             to refer    to remove


 d --------> 1  -------> 2 --------> 3 ----------> 4
                         ^           ^ 
                         to refer    to remove
 ```
