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

 ### multi-source BFS
 - all nodes at same level are considered to be explored **"at the same tick"**
 - example: [Rotting orange](https://leetcode.com/problems/rotting-oranges/)
 

## One Time
```
class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int[][] directions = {{0,1},{1,0},{-1,0},{0,-1}};
        int m = mat.length, n = mat[0].length;
        int[][] res = new int[m][n];
        
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                queue.offer(new int[] {i, j});
                visited[i][j] = true;
                }
            }
            
        }
        
        int cost = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                int r = curr[0];
                int c = curr[1];
                if (mat[r][c] == 1) res[r][c] = cost;
                
                for (int[] direction: directions) {
                    int nextR = r + direction[0];
                    int nextC = c + direction[1];
                    if (nextR >= 0 && nextR < m && nextC >= 0 && nextC < n && !visited[nextR][nextC]) { 
                        queue.offer(new int[] {nextR, nextC});
                        visited[nextR][nextC] = true;
                    }
                }
            }
        cost++;  
        }
        
        return res;
    }
}
```

## Many Times (extracted as a function)
```
class Solution {
    int[][] directions = {{0,1},{1,0},{-1,0},{0,-1}};
    int m; 
    int n;
    
    public int[][] updateMatrix(int[][] mat) {
        
        m = mat.length;
        n = mat[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 1) {
                    mat[i][j] = bfs(new int[] {i, j}, mat);
                }
            }
        }
        
        return mat;
    }
    
    public int bfs(int[] start, int[][] grid) {
 
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        
        queue.offer(start);
        visited[start[0]][start[1]] = true;
        
        int level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                int currR = curr[0];
                int currC = curr[1];
                if (grid[currR][currC] == 0) return level;
                
                for (int[] direction: directions) {
                    int nextR = currR + direction[0];
                    int nextC = currC + direction[1];
                    if (nextR >= 0 && nextR < m && nextC >=0 && nextC < n && !visited[nextR][nextC]) {
                        queue.offer(new int[] {nextR, nextC});
                        visited[nextR][nextC] = true;
                    }
                }
            }
            
            level++;
        }
        
        return level;
    }
}
```
