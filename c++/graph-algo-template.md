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
## Union Find
### Quick Find 
- array keeping track of the root of each node (instead of direct parent)
- Find O(1)
- Union O(N)
```
class UnionFind {
private: 
    vector<int> roots;
    int num_of_components;
public: 
    UnionFind(int n): num_of_components(n) {
        roots.resize(n);
        for (int i = 0; i < n; i++) roots[i] = i; 
    }

    int find(int x) {
        return roots[x];
    }
    
    void union_sets(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return;
        
        for (int i = 0; i < roots.size(); i++) {
            if (roots[i] == ry) roots[i] = rx;
        }
        num_of_components--;
        
    }
    
    int get_num_of_components() {
        return num_of_components;
    }

    bool is_connected(int x, int y) {
        return find(x) == find(y);
    }
};
```

### Quick Union
- array keeping track of the direct parent of each node
- Find O(N)
- Union O(N)
```
class UnionFind {
private: 
    vector<int> parents;
    int num_of_components;
public: 
    UnionFind(int n): num_of_components(n) {
        parents.resize(n);
        for (int i = 0; i < n; i++) parents[i] = i;
    }
    
    int find(int x) {
        int rx = x;
        while (parents[rx] != rx) rx = parents[rx];

        return rx;
    }
    
    void union_sets(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        parents[py] = px;
        
        num_of_components--;
    }
    
    int get_num_of_components() ...
    
    bool is_connected(int x, int y) ...
};
```

## Quick Union with Rank
- Keep track of rank of each node
- During union, always connect the shorter tree to the taller tree -- to achieved more balanced tree height
- Max Tree Height = logN (instead of N)
- Find O(logN)
- Union O(logN)
```
class UnionFind {
private: 
    vector<int> parents;
    int num_of_components;
    vector<int> ranks;
public: 
    UnionFind(int n): num_of_components(n) {
        parents.resize(n);
        ranks.resize(n);
        for (int i = 0; i < n; i++) {
            parents[i] = i;
            ranks[i] = 1;
        }
    }
    
    int find(int x) ...
    
    void union_sets(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (ranks[px] > ranks[py]) parents[py] = px;
        else if (ranks[px] < ranks[py]) parents[px] = py;
        else {
            //if tree height are the same, randomly connect one to the another and increment the rank of the node being connected to by 1
            parents[py] = px;
            ranks[px]++; 
        }
        
        num_of_components--;
    }
    
    int get_num_of_components() ...
    
    bool is_connected(int x, int y) ...
};
```
### Quick Union with Path Compression
- Update the parent of each node as we traverse along the tree during the find operation
- Find O(logN)
- Union O(logN)
```
class UnionFind {
private: 
    vector<int> parents;
    int num_of_components;
public: 
    UnionFind(int n): num_of_components(n) {
        parents.resize(n);
        for (int i = 0; i < n; i++) parents[i] = i;
    }
    
    int find(int x) {
        if (x == parents[x]) return x;
        parents[x] = find(parents[x]);
        return parents[x];
    }
    
    void union_sets(int x, int y) ...
    
    int get_num_of_components() ...
    
    bool is_connected(int x, int y) ...
};
```

### Quick Union with Path Compression & Rank
- Find: O(a(N))
- Union: O(a(N))
- a is the Inverse Ackermann function -- assume to be a constant
```
class UnionFind {
private: 
    vector<int> parents;
    int num_of_components;
    vector<int> ranks;
public: 
    UnionFind(int n): num_of_components(n) {
        parents.resize(n);
        ranks.resize(n);
        for (int i = 0; i < n; i++) {
            parents[i] = i;
            ranks[i] = 1;
        }
    }
    
    int find(int x) {
        if (x == parents[x]) return x;
        parents[x] = find(parents[x]);
        return parents[x];
    }
    
    void union_sets(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (ranks[px] > ranks[py]) parents[py] = px;
        else if (ranks[px] < ranks[py]) parents[px] = py;
        else {
            parents[py] = px;
            ranks[px]++; 
        }
        
        num_of_components--;
    }
    
    int get_num_of_components() ...
    
    bool is_connected(int x, int y) ...
};
```
- [summary](https://leetcode.com/explore/featured/card/graph/618/disjoint-set/3844/)
