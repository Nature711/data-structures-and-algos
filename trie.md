## Use case: anything related to prefix
- longest common prefix
- prefix search

![image](https://user-images.githubusercontent.com/77217430/207367177-ec275aec-7427-4b79-8c50-cd7e15de45b1.png)

## Implementation
```
class Trie {
    
    Node root;

    public Trie() { this.root = new Node(); }
    
    public void insert(String word) {
        Node currNode = root;
        for (int i = 0; i < word.length(); i++) {
            if (currNode.children[word.charAt(i) - 'a'] == null) {
                //current child is empty, create it
                currNode.children[word.charAt(i) - 'a'] = new Node(word.charAt(i));
            } //otherwise use the existing child
            currNode = currNode.children[word.charAt(i) - 'a']; //traverse down the child
            if (i == word.length() - 1) currNode.isTerminating = true; //current node is the end of SOME word in dictionary
        }
    }
    
    public boolean search(String word) {
        Node currNode = root;
        for (int i = 0; i < word.length(); i++) {
            if (currNode.children[word.charAt(i) - 'a'] == null) return false;
            currNode = currNode.children[word.charAt(i) - 'a'];
            if (i == word.length() - 1 && !currNode.isTerminating) return false; //search target is only a substring of some dictionary word, not exact match!
        }
        return true;
    }
    
    public boolean startsWith(String prefix) {
        Node currNode = root;
        for (int i = 0; i < prefix.length(); i++) {
            if (currNode.children[prefix.charAt(i) - 'a'] == null) return false;
            currNode = currNode.children[prefix.charAt(i) - 'a'];
        }
        return true;
    }
    
   public String findLCP() {
        StringBuilder sb = new StringBuilder();
        Node currNode = root;
        while (currNode != null) {
            int count = 0, next = -1, flag = 0;
            char c = 'R';
            for (int i = 0; i < 26; i++) {
                if (currNode.children[i] != null) {
                    next = i;
                    c = (char) (i + 97);
                    count++;
                    if (currNode.children[i].isTerminating) flag = 1;
                }
                if (count > 1) return sb.toString();
            }

            if (count == 0) return sb.toString();
            else sb.append(c);
            if (flag == 1) break;
            currNode = currNode.children[next];
        }
        return sb.toString();
    }
}

class Node {
    char c;
    Node[] children = new Node[27];
    boolean isTerminating = false;
    
    public Node() {this.c = 'R';}
    public Node(char c) {this.c = c;}
}
```
![image](https://user-images.githubusercontent.com/77217430/207367288-0260f26b-b131-43f4-9223-c1622b84c494.png)
![image](https://user-images.githubusercontent.com/77217430/207367318-923147f8-89fe-4596-a266-4253cdc6a6ef.png)

## Example questions
- [Implementing trie](https://leetcode.com/problems/implement-trie-prefix-tree/)
- [Longest common prefix](https://leetcode.com/problems/longest-common-prefix/solution/)
- [Word Search II](https://leetcode.com/problems/word-search-ii/)
