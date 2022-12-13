## Use case: anything related to prefix:
- longest common prefix
- prefix search

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
}

class Node {
    char c;
    Node[] children = new Node[27];
    boolean isTerminating = false;
    
    public Node() {this.c = 'R';}
    public Node(char c) {this.c = c;}
}

## Example questions
- [Implementing trie](https://leetcode.com/problems/implement-trie-prefix-tree/)
- [Longest common prefix](https://leetcode.com/problems/longest-common-prefix/solution/)
- [Word Search II](https:)//leetcode.com/problems/word-search-ii/)
