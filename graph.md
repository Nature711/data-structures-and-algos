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

### template: dfs in matrix (4 directions)

```
//global variables
- input matrix: int[][] globalMatrix;
- visited array: boolean[][] visited;
- directions: int[][] directions = {{0,1},{1,0},{-1,0},{0,-1}};

public void main() {
    //initialize global variables
    dfs(i, j, ...); //assume single source dfs
}

public void dfs(int i, int j,...) {
    
    <local work, e.g., matrix[i][j] = ...>
    
    //dfs from each of the 4 neighbors
    for (int[] direction: directions) {
        int nextR = r + direction[0];
        int nextC = c + direction[1];
        if (nextR >= 0 && nextR < m && nextC >= 0 && nextC < n || visited[nextR][nextC] || !<terminating conditions...>) {
          dfs(nextR, nextC);
          visited[nextR][nextC];
        }
     }
}
```

## BFS 

### basic struture
- in main function:
  - determine if it's single or multi source dfs problem
  - initialize a queue
  - add entry point into queue to kick-start the bfs search
- while queue is not empty: 
   - determine size of queue -- how many points we have for the current level
   - do for ```size``` iterations: 
      - poll an element from queue
      - local work
      - for each direction
        - calculate next point
        - check boundary and terminating conditions on next point
        - if satisifed, add next point to queue
        - mark next point as visited

```
public int[][] bfs(int[][] mat) {
        int[][] directions = {{0,1},{1,0},{-1,0},{0,-1}};
        int m = mat.length, n = mat[0].length;
        int[][] res = new int[m][n];
        
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {i, j}); //assume single source bfs
        
        int level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                int r = curr[0];
                int c = curr[1];
                <local work, e.g., mat[r][c] = ...>
                
                for (int[] direction: directions) {
                    int nextR = r + direction[0];
                    int nextC = c + direction[1];
                    if (nextR >= 0 && nextR < m && nextC >= 0 && nextC < n && !visited[nextR][nextC]) { 
                        queue.offer(new int[] {nextR, nextC});
                        visited[nextR][nextC] = true;
                    }
                }
            }
        level++;  
        }
 
    }
 ```
 
 ### note 1
 - in search function / iteration
  - local work
  - for each direction
    - calculate next point
    - check boundary and terminating conditions on next point
    - if satisifed, perform search on next point
    - mark next point as visited (depending on implementions -- for some questions, the "local work" at each step is effectively marking a point as visited)
 
 ### note 2
 - Binary tree level-order traveral is almost exactly the same as BFS, except that we don't need a ```visited``` array to keep track of whether a node is visited or not, since the "neighbors" of a node can only be its left (and) right child which is one level "below" it, and it's never possible to go back 
 
