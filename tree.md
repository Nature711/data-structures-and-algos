# Approaches

## Recursive

## Iterative


# Traversal

## Level order
- from left to right, level by level
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [1 2 3 4 5 6]
- implementation: queue
  - at each level, get the initial queue size (i.e., no of nodes at that level) --> determines how many times we need to poll from queue
  - for each node being polled out, add its left and right child into queue
  - complete traversal when queue is empty 
  - we can safely add the nodes of the next level to the current queue because we know exactly how many times we need to poll in order to traverse all the nodes at the current level (because we record down the queue size at each level before we start to traverse the nodes at that level)
- code:
```
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        
        if (root == null) return res;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> currLevel = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                currLevel.add(currNode.val);
                if (currNode.left != null) queue.offer(currNode.left);
                if (currNode.right != null) queue.offer(currNode.right);
            }
            res.add(currLevel);
        }
        
        return res;
    }
```

## Preoder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [1 2 4 5 3 6]
- implementation: 
``` 
print(root)
dfs(root.left)
dfs(root.right)
```
- code:
```
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public void dfs(TreeNode root) {
        if (root == null) return;
        res.add(root.val);
        dfs(root.left);
        dfs(root.right);
    }
}
```

## Inoder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [4 2 5 1 3 6]
- recursive implementation: 
``` 
dfs(root.left)
print(root)
dfs(root.right)
```
- iterative implementation: 
```
Stack<TreeNode> stack = new Stack<>();
TreeNode curr = root;

while (curr != null || !stack.isEmpty()) {
    while (curr != null) {
        stack.push(curr);
        curr = curr.left;
    }
    TreeNode popped = stack.pop();
    nodes.add(popped.val);
    curr = popped.right;
}
```

## Postorder 
```
    1
   / \
  2   3
 / \   \
4   5   6
```
- order: [4 5 2 6 3 1]
- implementation: 
``` 
dfs(root.left)
dfs(root.right)
print(root)
```

# N-ary tree
- similar to binary tree
- to visit its children, instead of 
