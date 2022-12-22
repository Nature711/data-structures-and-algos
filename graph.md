## Single source vs Multi source

- single source: ```search(i, j)```
- multi source: 
```
for (int i = 0; i < m; i++){
    for (int j = 0; j < n; j++) {
        if (<some condition...>) search(i, j);
    }
}
```

 ## Notes
 ### basic structure
 - in search function / iteration
  - local work
  - for each direction
    - calculate next point
    - check boundary and terminating conditions on next point
    - if satisifed, perform search on next point
    - mark next point as visited (depending on implementions -- for some questions, the "local work" at each step is effectively marking a point as visited)
 ### relation to tree traversal
 - Binary tree level-order traveral is almost exactly the same as BFS, except that we don't need a ```visited``` array to keep track of whether a node is visited or not, since the "neighbors" of a node can only be its left (and) right child which is one level "below" it, and it's never possible to go back 
 
