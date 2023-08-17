## DFS
```
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        if (source == destination) return true;
        unordered_map<int, vector<int>> graph;
        unordered_set<int> visited;
        stack<int> stack;
        stack.push(source);
        visited.insert(source);
        for (vector<int>& edge: edges) {
            if (graph.find(edge[0]) == graph.end()) graph[edge[0]] = {edge[1]};
            else graph[edge[0]].push_back(edge[1]);
            if (graph.find(edge[1]) == graph.end()) graph[edge[1]] = {edge[0]};
            else graph[edge[1]].push_back(edge[0]);
        }
        
        while (!stack.empty()) {
            int node = stack.top();
            stack.pop();
            vector<int>& neighbors = graph[node];
            for (int neighbor: neighbors) {
                if (neighbor == destination) return true;
                if (visited.find(neighbor) == visited.end()) {
                    visited.insert(neighbor);
                    stack.push(neighbor);
                }
            }
        }
        
        
        return false;
    }
};
```

## BFS
```
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        if (source == destination) return true;
        unordered_map<int, vector<int>> graph;
        unordered_set<int> visited;
        queue<int> queue;
        queue.push(source);
        visited.insert(source);
        for (vector<int>& edge: edges) {
            if (graph.find(edge[0]) == graph.end()) graph[edge[0]] = {edge[1]};
            else graph[edge[0]].push_back(edge[1]);
            if (graph.find(edge[1]) == graph.end()) graph[edge[1]] = {edge[0]};
            else graph[edge[1]].push_back(edge[0]);
        }
        
        while (!queue.empty()) {
            int node = queue.front();
            queue.pop();
            vector<int>& neighbors = graph[node];
            for (int neighbor: neighbors) {
                if (neighbor == destination) return true;
                if (visited.find(neighbor) == visited.end()) {
                    visited.insert(neighbor);
                    queue.push(neighbor);
                }
            }
        }
        
        return false;
    }
};
```
