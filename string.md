## Array idx -- character

- property 1: if ```c``` is some lowercase character, then ```c - 'a'``` is the "displacement of this character from the letter 'a'
e.g., 
```
'a' - 'a' --> 0
'b' - 'a' --> 1
'c' - 'a' --> 2
...
'z' - 'a' --> 25
```

- idea: use a fixed-size array to store info about each character
- the info of character ```c``` is stored in the ```c - 'a'```-th element (i.e., ```arr[c - 'a']```)
- constant space usage

- property 2: each character corresponds to an integer (one-to-one mapping) according to ascii specification
--> when a character is supplied to the array index bracket [], it will automatically be converted to an int
e.g., 
```
(int) 'a' --> 97
(int) 'b' --> 98
...
(int) 'z' --> 122

T[] arr = new T[256];
arr['a'] = xxx; --> arr[97] will be filled with value xxx
arr['z'] = yyy; --> arr[122] will be filled with value yyy

T[] arr2 = new T[26];
arr['a' - 97] = xxx; --> arr[0] will be filled with value xxx
arr['z' - 97] = yyy; --> arr[25] will be filled with value yyy
```

### Applications
- when we want to count occurences of each character in a string and store such info
- intuitive approach: use a hashmap -- key is character, value is its occurences
```
HashMap<Character, Integer> map = new HashMap<>();
for (char c: string.toCharArray) map.put(c, map.get(c, map.getOrDefault(c, 0) + 1);
```
- optimized approach: use a constant size int array -- value at index i represents the occurence of the character ```(char) i```
```
int[] map = new int[26];
for (char c: string.toCharArray) map[c - 'a']++;
```

### Example questions
- [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Space optimization
- count occurences of character in one string, validate if another string contains the same set of charactcers, or with the same number of occurences
- instead of using 2 maps to store the info about 2 strings, we can use only 1 map:
  - 1st iteration: iterate through one string to ***increment*** occurence
  - 2nd iteration: iterate through the other string to ***decrement*** occurence
  - at last: check if all the values in map are zero --> the two string ***cancel off*** each other

### Example questions
- [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
- [Ransom Note](https://leetcode.com/problems/ransom-note/?envType=study-plan&id=data-structure-i)


## String manipulation 

### String vs Char (in Java)
|                    | String      | char |
| ----------- | ----------- | ----------- |
|     representation          |       double quote "a"    | single quote 'a'       |
|     empty value         |        ```String s = " "```    | ```char c = ' '```    |
|     related methods        |      ```s.substring(i, j)```  | ```s.charAt(idx)```  |


### Number in string format
- Conversion: ```Integer.valueOf()```
- Check: pattern matching using regex

```
String nString = "8";
Integer.valueOf(nString); //returns 8

nString = "18";
Integer.valueOf(nString); //returns 18

Pattern pattern = Pattern.compile(".*[^0-9].*");
pattern.matcher(nString).matches(); //returns true;
nString = "abc";
pattern.matcher(nString).matches(); //returns false;

```
- notes on ```Integer.valueOf()```
  - when input string is not an integer --> throw exception
    - e.g., ```Integer.valueOf("abc"); //NumberFormatException```
  - when input is a ```char``` --> returns the corresponding unicode value
    - e.g., ```Integer.valueOf('a'); //returns 97``` 
  - ignores the zero padding in input string 
    - e.g., ```Integer.valueOf('018'); //returns 18```


### Number in Char format
- conversion: ```Character.getNumericValue()``` 
- check: ```Character.isDigit()```

```
String nString = "a1b2"

Character.getNumericValue(nString.charAt(1)); //returns 1

Character.getNumericValue(nString.charAt(0)); //throws exception

Character.isDigit(nString.charAt(0)); //returns false;
```

### common mistake 1
- given a string in the form of: each letter followed by a number representing the occurences of this letter, output the uncompressed string
--> can't simply assume chars are alternating between char & number -- what if a number has more than 1 digit, in which case it's represented by more than 1 char?
- correct approach: once encounter a digit, keep iterating while we're still pointing at a digit, break only after we hit a non-digit
```
StringBuilder numSb = new StringBuilder();
while (i < compressedString.length() && Character.isDigit(compressedString.charAt(i))) {
    //keep appending while current char is still a digit
    numSb.append(compressedString.charAt(i++));
}
int t = Integer.valueOf(numSb.toString());
```
- related questions:
  - [Design Compressed String Iterator](https://leetcode.com/problems/design-compressed-string-iterator/)
  - [Decode String](https://leetcode.com/problems/decode-string/)

### common mistake 2
- StringBuilder is passed / stored by reference, not by copy
  - relevant problem: [Sum root to leaf numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/submissions/)
  - the code below doesn't work: the old variable is not actually storing a copy of the StringBuilder object, but only a reference to it
  - When you modify the currPath variable after storing it in old, any subsequent changes to currPath will also be reflected in old
```
public void backtrack(TreeNode root, StringBuilder currPath) {
        if (root.left == null && root.right == null) {
            int num = Integer.valueOf(currPath.toString());
            nums.add(num);
            return;
        }
        StringBuilder old = currPath;
        
        if (root.left != null) {
            currPath.append(Integer.toString(root.left.val));
            backtrack(root.left, currPath);
            currPath = old;
        }
        if (root.right != null) {
            currPath.append(Integer.toString(root.right.val));
            backtrack(root.right, currPath);
            currPath = old;
        }
    }
```
- fix1: make a copy of the StringBuilder object before storing it in old
  - ```StringBuilder old = new StringBuilder(currPath.toString());```
- fix2: directly remove last char from StringBuilder
  - instead of using ```currPath = old```, use ```currPath.deleteCharAt(currPath.length() - 1)```
