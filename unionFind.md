## Overview
- with path compression --> N elements results in tree with height O(logN)
- time complexity for both union & find operation: O(logN)

```
UnionFind uf = new UnionFind(numOfElements)
for each pair of node (p1, p2) in graph
  if (p1, p2 satisfy some conditions) uf.union(p1, p2)

return uf.numOfComponets
```
## Sample class for Union Find (with path compression)
```
public class UnionFind {
    private int[] parents;
    private int[] size;
    int numOfComponets = 0;

    public UnionFind(int n) {
        parents = new int[n];
        size = new int[n];
        numOfComponets = n;
        for (int i = 0; i < parents.length; i++) {
            parents[i] = i;
            size[i] = 1;
        }
    }

    public int find(int cur) {
        int root = cur;
        while (root != parents[root]) {
            root = parents[root];
        }
        // Path Compression
        while (cur != root) {
            int preParent = parents[cur];
            parents[cur] = root;
            cur = preParent;
        }
        return root;
    }

    public int findComponentSize(int cur) {
        int parent = find(cur);
        return size[parent];
    }

    public void union(int node1, int node2) {
        int node1Parent = find(node1);
        int node2Parent = find(node2);

        if (node1Parent == node2Parent)
            return;

        if (size[node1Parent] > size[node2Parent]) {
            parents[node2Parent] = node1Parent;
            size[node1Parent] += size[node2Parent];
        } else {
            parents[node1Parent] = node2Parent;
            size[node2Parent] += size[node1Parent];
        }
        numOfComponets--;
    }
}
```
