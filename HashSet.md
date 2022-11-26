## Description
- a collection of items where every item is unique
- ```HashSet<T> cars = new HashSet<>()```

## Common use cases
- when you want to store a set of items and make sure each item is unique


## Operations

| Method  | Description | Example | 
| ------- | ------- | -------- |
| ```boolean add(T ele)```  | add an element of type T to the hashset | ```for (int num: nums) set.add(num);``` |
| ```boolean contains(T ele)```  | returns true if the set contains the specified element  | ```if (set.contains(num)) //do smth```   |
| ```boolean isEmpty()```  | returns true if the hashset is empty | ```if (set.isEmpty()) //do smth``` |
| ```int size()```  | returns the number of elements in the hashset | ```if (set.size() == n) //do smth```  |
| ```Iterator<T> iterator()```  | returns an iterator over the elements in the hashset | ```for (int num: set.iterator()) System.out.println（num);```  |


## Example questions
- [Contains Duplicates](https://github.com/Nature711/my-leetcode-notes/tree/master/0217-contains-duplicate)
