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

### applications
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

## Example questions
- [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Space optimization
- count occurences of character in one string
- instead of using 2 hashmaps, we 
