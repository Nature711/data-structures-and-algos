## Controlled iteration
- example: [BST inorder iterator](https://leetcode.com/problems/binary-search-tree-iterator/solution/)
```
class BSTIterator {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode root;
    
    public BSTIterator(TreeNode root) {
        this.root = root;
    }
    public int next() {
        TreeNode leaf = leftmost(); 
        this.root = leaf.right; //point C
        return leaf.val; //point E
    }
    
    public boolean hasNext() {
        return !this.stack.isEmpty() || this.root != null;
    }
    
    public TreeNode leftmost() {
        while (this.root != null) {
            this.stack.push(this.root);
            this.root = this.root.left;
        }
        return stack.pop(); //point A
    }
}
```
- compared with iterative inorder traversal
  - what the call to ```leftmost()``` returns (point A) is exactly what it is at inorder traversal point B
  - point E is the same as point F -- adding a node to the traversed list
  - point C is the same as point D -- ready to traverse right tree
  - the only difference:
    - in controlled iteration, once we find the next node, we immediately return it; also we set the current node pointer to its right subtree (to prepare for the next calls)
    - in inorder traversal, the while loop is like continuously calling next() to find the next node until it traverses the whole tree

![image](https://user-images.githubusercontent.com/77217430/208617671-58fbb0c7-d292-4dc1-afa8-2d93bd9834a6.png)

```
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }        
        curr = stack.pop(); //point B
        res.add(curr.val); //point F
        curr = curr.right; //point D
    }
    return res;
}
```
