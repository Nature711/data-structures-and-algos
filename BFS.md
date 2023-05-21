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

 ## multi-source BFS
 - When you perform BFS from a set of points, it means that you consider all those points as starting points and perform BFS simultaneously from all those points. 
 - i.e., in iteration 1, explore all neighbours of all the starting points; in iteration 2, explore all the neighbours of the points you reached in iteration 1, etc.
 - useful when you want to find the shortest path from any of the starting points to a target.
 - the path found this way will be optimal among all paths starting from any of the starting points.
 - On the other hand, when you perform BFS many times, each time from a different starting point, you explore the graph separately from each starting point. This means you do a complete BFS from the first point, then reset everything, do a complete BFS from the second point, etc. 
 - This type of search is useful when you need to find paths from each starting point separately, e.g., when you want to compare the paths or if the graph changes between searches. 
 - However, performing BFS many times is usually more time-consuming than performing BFS from a set of points, because you may end up exploring the same areas of the graph multiple times
 - example questions
      - example: [Rotting orange](https://leetcode.com/problems/rotting-oranges/)
      - [01 Matrix](https://leetcode.com/problems/01-matrix/)
      - [Shortest Bridge](https://leetcode.com/problems/shortest-bridge/)

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
