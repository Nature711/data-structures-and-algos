## Properties 

- LIFO -- last in first out

## Use cases
### processing from outside to inwards
- mirroring the recurisve structure of some problems
- delay evaluation of outer layer; evaluate immediately when hitting "base case"
e.g. 
- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/solution/)
- [Decode String](https://github.com/Nature711/my-leetcode-notes/edit/master/0394-decode-string/NOTES.md)

### "undo" some operation
- where the operation to be undone is the "most recent" one -- pop from stack
e.g.
- [Backspace String Comparison](https://github.com/Nature711/my-leetcode-notes/blob/master/0844-backspace-string-compare/NOTES.md)

### the idea of recursing into it
- [Flatten a multilevel doubly linked list](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)
```
public Node flatten(Node head) {
        if (head == null) return head;
        Node dummy = new Node();
        Node curr = dummy, tmp;
        //invariant: all the nodes up to and including curr have their child been flatten (i.e., at a single level)
        Stack<Node> stack = new Stack<>();
        //stack to store the nodes to be processed -- LIFO
        stack.push(head);
        while (!stack.isEmpty()) {
            tmp = stack.pop(); 
            if (tmp.next != null) stack.push(tmp.next); //add node's next first to delay its evaluation -- only after the tmp node's children have been processed
            if (tmp.child != null) stack.push(tmp.child); //add node's child later which will be processed immediately in the next iteration
            curr.next = tmp;
            tmp.prev = curr;
            tmp.child = null;
            curr = tmp;
        }
        dummy.next.prev = null;
        return dummy.next;
    }
```
