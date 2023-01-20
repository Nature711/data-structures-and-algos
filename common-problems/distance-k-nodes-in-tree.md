## [All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

![image](https://user-images.githubusercontent.com/77217430/213609303-575fc266-46dd-4107-ba6a-70238f649404.png)

### Convert tree to (undirected) graph
```
HashMap<TreeNode, List<TreeNode>> graph = new HashMap<>();
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        //flatten tree
        levelOrder(root);
    
        //bfs
        int n = graph.size();
        Set<TreeNode> visited = new HashSet<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(target);
        visited.add(target);
        
        while (!queue.isEmpty() && k-- > 0) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                List<TreeNode> nextLevelNodes = graph.get(currNode);
                if (nextLevelNodes == null) break;
                for (TreeNode next: nextLevelNodes) {
                    if (!visited.contains(next)) queue.offer(next);
                    visited.add(next);
                }
            }
        }
        
        //now the queue contains all nodes at the k-th level, i.e., the nodes that are distance k from target
        List<Integer> res = new ArrayList<>();
        while (!queue.isEmpty()) res.add(queue.poll().val);
        return res;
    }
    
    public void levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                if (currNode.left != null) {
                    queue.offer(currNode.left);
                    TreeNode n1 = currNode, n2 = currNode.left;
                    if (graph.containsKey(n1)) graph.get(n1).add(n2);
                    else {
                        List<TreeNode> list = new ArrayList<>();
                        list.add(n2);
                        graph.put(n1, list);
                    }
                    
                    if (graph.containsKey(n2)) graph.get(n2).add(n1);
                    else {
                        List<TreeNode> list = new ArrayList<>();
                        list.add(n1);
                        graph.put(n2, list);
                    }
                }
                if (currNode.right != null) {
                    queue.offer(currNode.right);
                    TreeNode n1 = currNode, n2 = currNode.right;
                    if (graph.containsKey(n1)) graph.get(n1).add(n2);
                    else {
                        List<TreeNode> list = new ArrayList<>();
                        list.add(n2);
                        graph.put(n1, list);
                    }
                    
                    if (graph.containsKey(n2)) graph.get(n2).add(n1);
                    else {
                        List<TreeNode> list = new ArrayList<>();
                        list.add(n1);
                        graph.put(n2, list);
                    }
                }
            }
                
        }
    }
```

### Annotate parent
- DFS to traverse tree and establish parent-child relationship, stored in hashmap 
  - hashmap.put(curr, source)
    - initialization: hashmap.put(root, null)
- BFS starting from target node
  - next level: left child + right child + **parent** of current node
    - left, right child: curr.left, curr.right
    - parent: hashmap.get(curr) 
```
 Map<TreeNode, TreeNode> parent;
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        parent = new HashMap();
        dfs(root, null);

        Queue<TreeNode> queue = new LinkedList();
        queue.add(null);
        queue.add(target);

        Set<TreeNode> seen = new HashSet();
        seen.add(target);
        seen.add(null);

        int dist = 0;
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                if (dist == K) {
                    List<Integer> ans = new ArrayList();
                    for (TreeNode n: queue)
                        ans.add(n.val);
                    return ans;
                }
                queue.offer(null);
                dist++;
            } else {
                if (!seen.contains(node.left)) {
                    seen.add(node.left);
                    queue.offer(node.left);
                }
                if (!seen.contains(node.right)) {
                    seen.add(node.right);
                    queue.offer(node.right);
                }
                TreeNode par = parent.get(node);
                if (!seen.contains(par)) {
                    seen.add(par);
                    queue.offer(par);
                }
            }
        }

        return new ArrayList<Integer>();
    }

    public void dfs(TreeNode node, TreeNode par) {
        if (node != null) {
            parent.put(node, par);
            dfs(node.left, node);
            dfs(node.right, node);
        }
    }
```
