# Intro to data structures & algorithms

## Data structures: what & why
- Motivation: human works with different types of data for different tasks
  - e.g., dictionaries (sorted lists), city maps (geometries), cash books (tabular schema)
- DS is a way to store and organize data in a *computer*, so that it can be *used efficiently*

### Abstract Data Types (ADT)
- ADTs define data and operations (logical view, abstraction) without implementation details
  - e.g., lists, abstractly defined for storing, read, modifying elements
- Implementations provide concrete types with detailed functionality
  - list (ADT) is implemented using array in many programming languages
- Studying data structures involves examining logical views, available operations, and their costs.

## Framework for learning data structures
### Storage method of data structure
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

### Basic operations on data structures
1. **Traversal**
2. **Access** -- add, delete, query, modify
- purpose of building different data structures: to access as efficiently as possible in different scenarios
#### How to traverse & access
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

### From data structures to algorithms
- data structures ~ building blocks (tools)
- algorithms ~ performing various tasks using appropriate building blocks

## Essence of algorithm: "exhaustive enumuration"
- some algorithms (e.g., cryptographic & ML algorithms, problems that can be solved using math formula) are programming implementations of mathematical principles -- not the scope of our discussion
- e.g., permutation and combination
  - mathematical approach: using math formula (1 line of code)
  - computational approach ("computer algorithms"): backtracking algorithm
    - abstract the problem into a **multi-branch tree structure** (computational structure), then use the backtracking algorithm to **brute force** it -- exhaustively enumurate all possible answers

### Challenges 
- **No omission" -- otherwise wrong answer
- **No redundancy" -- otherwise slow computation

### Dimension to approach algorithmic problems
1. **How to enumerate** --  to enumerate all possible solutions without omission
2. **How to exhaustively search the problem in a clever way** -- to avoid redundant calculations and consume as few resources as possible to find the answer
- different problems may be difficult in terms of different dimensions 
#### Algorithms that are difficult in terms of "how to exhaustively enumerate"
- recursive problems in general, especially dynamic programming
- core of DP
  - writing a brute force solution (i.e., state transition equation)
  - adding a memo --> become a top-down recursive solution
  - modifying it to become a bottom-up recursive solution
  - optimizations: dimensionality reduction -- problem of "how to enumerate in a clever way"
- main difficulty: finding the "state transition equation" in the first place
  - mathematical induction
#### Algorithms that are difficult in terms of "how to enumerate in a clever way"
- some well-known non-recursive algorithms
- e.g. , Union Find 
  - goal: to find connected components in a graph
  - brute force approach (exhaustive): DFS / BFS
  - Union Find: use an array to simulate tree structure -- O(1)
- e.g., Greedy algorithm
  - to find patterns in the problem so that it can be solved without needing to exhaustively enumerate all solutions

## Common algorithms 
### Array & singly linked list 
#### Two pointers
- fast & slow pointer: linked list cycle
  - brute force: use a `HashSet` to to keep track of the nodes encountered, report cycle if seeing duplicate
    - memory overhead
  - two pointer: "fast" & "slow" pointers -- smart exhaustive way, O(1) space
- binary search: to find an element in a sorted array
- sliding windows: to solve substring related problems
- palidrome related problems
#### Prefix sum 
- to calculate the sum of subarrays, without repeatedly using for-loop to traverse array
#### Difference array
- to add / subtract subarrays, without needing a loop

### Binary tree
- 2 ideas of recursive solutions to binary tree problems: 
1. Traverse the binary tree to get the answer
   - similiar idea as backtracking problem
2. Decomposing the problem to get the answer
   - similiar idea as DP problem
