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
- note: need to BFS for multiple times, each time starting from a remaining unvisited node -- this is because the graph may not be fully connected
```
    public boolean isBipartite(int[][] graph) {
        boolean[] isColored = new boolean[graph.length];
        Set<Integer> visited = new HashSet<>();
        for (int n = 0; n < graph.length; n++) {
            if (!visited.contains(n)) {
                Queue<Integer> q = new LinkedList<>();
                q.offer(n);
                visited.add(n);
                while (!q.isEmpty()) {
                    int size = q.size();
                    for (int i = 0; i < size; i++) {
                        int curr = q.poll();
                        for (int nei: graph[curr]) {
                            if (!visited.contains(nei)) {
                                q.offer(nei);
                                visited.add(nei);
                                isColored[nei] = !isColored[curr];
                            } else {
                                if (isColored[nei] == isColored[curr]) return false;
                            }
                        }
                    }
                }
            }
        }
        return true;
    }
```
