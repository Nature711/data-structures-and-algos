## Overview
- with path compression --> N elements results in tree with height O(logN)
- time complexity for both union & find operation: O(logN)

```
UnionFind uf = new UnionFind(numOfElements)
for each pair of node (p1, p2) in graph
  if (p1, p2 satisfy some conditions) uf.union(p1, p2)

return uf.numOfComponets
```

## Example questions
- [Number of Provinces](https://github.com/Nature711/my-leetcode-notes/blob/master/0547-number-of-provinces/NOTES.md)
- [Most stones removed from same row or column](https://github.com/Nature711/my-leetcode-notes/blob/master/0947-most-stones-removed-with-same-row-or-column/NOTES.md)

## Sample code of Union Find (with path compression)
- note: each element in a UnionFind class has a unique id
  - case 1: the actual id value doesn't matter; we just need it to uniquely identify each elements; what we really care is the numOfComponets
  - case 2: the actual value of each element does matter, then we need to keep track of additional info for that 
### 
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

### Case 2
```
class Solution {
    Map<Integer, Integer> ids = new HashMap<>();
    Map<Integer, Integer> size = new HashMap<>();
    
    public int longestConsecutive(int[] nums) {
        
        for(int num: nums){
            ids.put(num, num);
            size.put(num, 1);
        }
        
        for(int num: nums){
            if(ids.containsKey(num+1)){
                union(num, num+1);
            }
        }
        
        int maxSize = 0;
        for(int curr: size.values()){
            maxSize = Math.max(maxSize, curr);
        }
        
        return maxSize;
    }
    
    public void union(int p1, int p2){
        int p1id = find(p1);
        int p2id = find(p2);
        
        if(p1id==p2id) return;
        
        if(p1id>p2id){
            ids.put(p2id, p1id);
            size.put(p1id, size.get(p2id) + size.get(p1id));
        } else{
            ids.put(p1id, p2id);
            size.put(p2id, size.get(p1id) + size.get(p2id));
        }
    }
    
    public int find(int p){
        int root = p;
        while(root!=ids.get(root)){
            root=ids.get(root);
        }
        
        while(p!=root){
            int next = ids.get(p);
            ids.put(p, root);
            p = next;
        }
        
        return root;
    }
}
```
