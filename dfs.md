
## DFS

### basic struture
- in main function:
  - determine if it's single or multi source dfs problem
  - call dfs as entry point
- in dfs function:
  - local work
  - for each direction
    - calculate next point
    - check boundary and terminating conditions on next point
    - if satisifed, perform dfs on next point
    - mark next point as visited

## Matrix (4 directions)
```
main(matrix) {
    for (int i = 0; i < matrix.length; i++) {
          for (int j = 0; j < matrx[0].length; j++) {
              if (<some_condition>) dfs(i, j, matrix); //enter and search matrix from index (i,j)
          }
     }
}

dfs(i, j, matrix) {
    if (i < 0 || i == grid.length || j < 0 || j == grid[0].length || <terminating_condition>) return;
    //out of bounds / hitting terminating condition
        
    matrix[i][j] = 0; //mark current grid as visited
    
    //searching each of the 4 directions 
    dfs(i + 1, j, grid);
    dfs(i, j + 1, grid);
    dfs(i - 1, j, grid);
    dfs(i, j - 1, grid);

    return;
}
```

### Examples 
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
  - DFS simplies marks visited grids; returning void
  - Count the number of DFS done
- [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
  - DFS returns the size of area 
  - Find the max area after DFSing all areas

## States marking

```
L ← Empty list that will contain the sorted nodes
while exists nodes without a permanent mark do
    select an unmarked node n
    visit(n)

function visit(node n)
    if n has a permanent mark then
        return
    if n has a temporary mark then
        stop   (graph has at least one cycle)

    mark n with a temporary mark

    for each node m with an edge from n to m do
        visit(m)

    remove temporary mark from n
    mark n with a permanent mark
    add n to head of L
 ```
 
 ### Examples
 - [Course Schedule](https://github.com/Nature711/my-leetcode-notes/blob/master/0207-course-schedule/NOTES.md)
 
 ## Variant: Topological sort
 
 ```
 L ← Empty list that will contain the sorted elements
S ← Set of all nodes with no incoming edge

while S is not empty do
    remove a node n from S
    add n to L
    for each node m with an edge e from n to m do
        remove edge e from the graph
        if m has no other incoming edges then
            insert m into S

if graph has edges then
    return error   (graph has at least one cycle)
else 
    return L   (a topologically sorted order)
```

 ### Examples
 - [Course Schedule II](https://github.com/Nature711/my-leetcode-notes/blob/master/0210-course-schedule-ii/NOTES.md/)

## DFS vs Backtracking


| DFS     | Backtracking |
| ----------- | ----------- |
|    handles an explicit tree  | handles an implicit tree      |
| the state of the node remains same after a path is explored so that it will not be explored again   | need to restore previous state of visited node, by making visited=false, after exploring current path     |
| once you find a desired path to a cell, there's no need to consider it again | need to explore different paths and find all possible solutions
|searching (reachability, etc) | pattern finding | 

### Classic dfs problems
- [Number of Islands](https://leetcode.com/problems/number-of-islands)
- [Path Sum](https://leetcode.com/problems/path-sum)

### Classic backtracking problems
- [Word Search](https://leetcode.com/problems/word-search/) -- very likely to be mistreated as a DFS problem (forgot to restore state after visiting)
- [Subsets](https://leetcode.com/problems/subsets)
- [Permutations](https://leetcode.com/problems/permutations)
- [Path Sum II](https://leetcode.com/problems/path-sum-ii)

- related: [path sum series](https://github.com/Nature711/my-leetcode-notes/blob/master/path-sum-series.md)
