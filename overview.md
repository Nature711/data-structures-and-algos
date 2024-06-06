# Intro to data structures 

## What & Why
- Motivation: human works with different types of data for different tasks
  - e.g., dictionaries (sorted lists), city maps (geometries), cash books (tabular schema)
- DS is a way to store and organize data in a *computer*, so that it can be *used efficiently*

## Abstract Data Types (ADT)
- ADTs define data and operations (logical view, abstraction) without implementation details
  - e.g., lists, abstractly defined for storing, read, modifying elements
- Implementations provide concrete types with detailed functionality
  - list (ADT) is implemented using array in many programming languages
- Studying data structures involves examining logical views, available operations, and their costs.

# Framework
## Storage method of data structure
1. **Array** -- sequential storage
2. **Linked list** -- chained storage
- All other data structures (e.g., hash table, stack, queue, tree, graph, etc.) are built on top of them -- "structural foundation"
### "Derived" data structures
- queue, stack: implemented using either linked list or array
  - array: need to deal with expansion & contraction
  - linked list: memory overhead (storing node pointers)
- graph: represented with either adjacency list (linked list) or adjacency matrix (2D array)
  - adjacency matrix
    - fast to perform most operations (e.g., checking connectivity between nodes, add / remove an edge) -- O(1)
    - more memory overhead -- O(V^2)
  - adjacency list
    - less memory overhead -- O(E)
    - more costly to perform most operations -- O(V)
- hash table: maps keys to a large array using a hash function
    - collision resolution: separated chaining, linear probing
- tree: implemented using either linked list or array
    - array: "heap" -- complete binary tree
    - linked list: general trees
    - derived trees (for more efficient operations): AVL tree, Red-black tree, B+ tree
### Comparison
#### Array
- stored continuously in memory -- random access via index, O(1)
- fixed size -- memory must be allocated once at creation; problem with expansion & contraction, O(N)
- insertion / deletion in the middle of array -- need to move all data, O(N)
#### Linked list
- not stored continuously in memory -- access an element O(N)
- dynamic size -- insertion / deletion operation O(1)
- more memory overhead to store node pointers

![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/0f610a96-9aba-4436-8500-b5e56d172de3)

## Basic operations on data structures
1. **Traversal**
2. **Access** -- add, delete, query, modify
- purpose of building different data structures: to access as efficiently as possible in different scenarios
### How to traverse & access
1. **Linear**: for/while loop iteration
2. **Non-linear**: recursion
- linked list traversal
```
class ListNode {
    int val;
    ListNode next;
}

void traverse(ListNode head) {
    for (ListNode p = head; p != null; p = p.next) {
        // linear, iterative
    }
}

void traverse(ListNode head) {
    // non-linear, recursive
    traverse(head.next);
}
```
- from linked list to binary to N-binary tree traversal
  - binary tree: essentially a linekd list with 2 "next" pointers
  - N-binary tree: essentially a linekd list with N "next" pointers
```
class TreeNode {
    int val;
    TreeNode left, right;
}

void traverse(TreeNode root) {
    traverse(root.left);
    traverse(root.right);
}

class TreeNodeN {
    int val;
    TreeNode[] children;
}

void traverse(TreeNodeN root) {
    for (TreeNodeN child : root.children)
        traverse(child);
}
```
- from tree to graph traversal: graph is essentially a combination of several trees
  - cycle can be dealt with using `visited` array

## Data structures & Algorithms
- data structures ~ building blocks (tools)
- algorithms ~ performing various tasks using appropriate building blocks
