## Overview 
- A minimum spanning tree (MST) is a subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight
![image](https://user-images.githubusercontent.com/77217430/211953897-4fcd81ad-41d1-4494-93f5-faf133c44fe7.png)
- [MST explore section](https://leetcode.com/explore/featured/card/graph/621/algorithms-to-construct-minimum-spanning-tree/3884/)

## Properties
- Cut Property: For any cut C of the graph, if the weight of an edge E in the cut-set of C is strictly smaller than the weights of all other edges of the cut-set of C, then this edge belongs to all MSTs of the graph.
--> Motivated algo: Kruskal's Algo
  1. Sort all the edges in non-decreasing order of their weight. 
  2. Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far. If the cycle is not formed, include this edge. Else, discard it. 
  3. Repeat step#2 until there are (V-1) edges in the spanning tree.

- [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

```
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        List<int[]> edges = new ArrayList<>(); //edges is a size-3 array where each element is {distanceBetweenP1P2, p1, p2}
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int[] p1 = points[i];
                int[] p2 = points[j];
                int distance = Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
                edges.add(new int[] {distance, i, j});
            }
        }
        
        Collections.sort(edges, (e1, e2) -> e1[0] - e2[0]); //sort the edges in ascending order of their distance, then start picking from the min cost edge
        
        UnionFind uf = new UnionFind(n);
        int totalCost = 0;
        int numEdges = 0;
        
        for (int[] edge: edges) {
            if (numEdges == n - 1) break;
            int cost = edge[0], p1 = edge[1], p2 = edge[2];
            if (uf.union(p1, p2)) { //union returns true only if p1, p2 are not yet in the same component --> only in this case do we union them (choose this edge)
                numEdges++;
                totalCost += cost;
            }
            //otherwise if p1, p2 are already connected by the current edge, then choosing this edge will return in a cycle which we don't want
        }
        return totalCost;
    }
}

public class UnionFind {
    int[] parents;
    int[] size;
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

    public boolean union(int node1, int node2) {
        //whether union operation is succussful
        int node1Parent = find(node1);
        int node2Parent = find(node2);

        if (node1Parent == node2Parent) return false;

        if (size[node1Parent] > size[node2Parent]) {
            parents[node2Parent] = node1Parent;
            size[node1Parent] += size[node2Parent];
        } else {
            parents[node1Parent] = node2Parent;
            size[node2Parent] += size[node1Parent];
        }
        numOfComponets--;
        return true;
    }
}
```
