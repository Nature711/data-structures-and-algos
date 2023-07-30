## Overview of std::unordered_map
- `std::unordered_map`: c++ container that stores key-value pairs
  
## Common Methods
1. **Declaration:**
   - C++:
     ```cpp
     #include <unordered_map>
     std::unordered_map<int, std::string> myMap;
     ```
   - Java:
     ```java
     import java.util.HashMap;
     HashMap<Integer, String> myMap = new HashMap<>();
     ```

2. **Insertion:**
   - C++:
     ```cpp
     myMap.insert({1, "apple"});
     myMap[2] = "peach"; //alternatively
     ```
   - Java:
     ```java
     myMap.put(1, "apple");
     ```

3. **Access Value using Key:**
   - C++:
     ```cpp
     std::string fruit = myMap[1];
     std::string fruit2 = myMap.get(2);
     ```
   - Java:
     ```java
     String fruit = myMap.get(1);
     ```

4. **Find if Key Exists:**
   - C++:
     ```cpp
     if (myMap.find(1) != myMap.end()) {
         // Key exists
     }
     ```
   - Java:
     ```java
     if (myMap.containsKey(1)) {
         // Key exists
     }
     ```

5. **Removal of Element:**
   - C++:
     ```cpp
     myMap.erase(1);
     ```
   - Java:
     ```java
     myMap.remove(1);
     ```

6. **Size of the Map:**
   - C++:
     ```cpp
     int size = myMap.size();
     ```
   - Java:
     ```java
     int size = myMap.size();
     ```

7. **Iteration through the Map:**
   - C++:
     ```cpp
     for (const auto& pair : myMap) {
         int key = pair.first;
         std::string value = pair.second;
         // Process key-value pair
     }
     ```
   - Java:
     ```java
     for (Map.Entry<Integer, String> entry : myMap.entrySet()) {
         int key = entry.getKey();
         String value = entry.getValue();
         // Process key-value pair
     }
     ```

## Comparing with ```std::map```

| Feature                    | `std::map`                         | `std::unordered_map`             |
|----------------------------|-----------------------------------|----------------------------------|
| Underlying Data Structure  | Balanced Binary Search Tree       | Hash Table                       |
| Elements Sorted            | Sorted by Keys                    | Not Sorted                       |
| Time Complexity (Average)  | Insertion, Deletion, Search: O(log n) | Insertion, Deletion, Search: O(1) |
| Performance                | Good for Small Datasets, Sorted Traversal | Good for Large Datasets, Fast Access |
| Memory Usage               | More Memory Consuming              | Less Memory Consuming             |

