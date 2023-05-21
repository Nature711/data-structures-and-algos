## DFS
### Stack
```
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1);

        for (int start = 0; start < n; ++start) {
            if (color[start] == -1) {
                Stack<Integer> stack = new Stack();
                stack.push(start);
                color[start] = 0;

                while (!stack.empty()) {
                    Integer node = stack.pop();
                    for (int nei: graph[node]) {
                        if (color[nei] == -1) {
                            stack.push(nei);
                            color[nei] = color[node] ^ 1;
                        } else if (color[nei] == color[node]) {
                            return false;
                        }
                    }
                }
            }
        }

        return true;
    }
}
```
### Recursion
```
Set<Integer> visited = new HashSet<>();
    boolean[] isColored;
    int[][] Graph;
    public boolean isBipartite(int[][] graph) {
        Graph = graph;
        isColored = new boolean[graph.length];
        for (int i = 0; i < graph.length; i++) {
            if (!visited.contains(i)) 
                if (!dfs(i)) return false;
        }
        return true;
    }
    
    public boolean dfs(int node) {
        visited.add(node);
        for (int neighbor: Graph[node]) {
            if (!visited.contains(neighbor)) {
                isColored[neighbor] = !isColored[node];
                if (!dfs(neighbor)) return false;
            } else {
                if (isColored[neighbor] == isColored[node]) return false;
            }
        }
        return true;
    }
```
## BFS
### Queue
```
    public boolean isBipartite(int[][] graph) {
        //BFS
        // 0(not meet), 1(black), 2(white)
        int[] visited = new int[graph.length];
        
        for (int i = 0; i < graph.length; i++) {
            if (graph[i].length != 0 && visited[i] == 0) {
                visited[i] = 1;
                Queue<Integer> q = new LinkedList<>();
                q.offer(i);
                while(! q.isEmpty()) {
                    int current = q.poll();
                    for (int c: graph[current]) {
                            if (visited[c] == 0) {
                                visited[c] = (visited[current] == 1) ? 2 : 1;
                                q.offer(c);
                            } else {
                                if (visited[c] == visited[current]) return false;
                            }
                    }
                }                        
            }
        }
        
        return true;
    }
```
