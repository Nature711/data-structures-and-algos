## HashMap vs LinkedHashMap

- LinkedHashMap maintains insertion order
```
public class LinkedHashMapExample {
    public static void main(String[] args) {
        // Create a LinkedHashMap with default parameters
        LinkedHashMap<String, Integer> linkedHashMap = new LinkedHashMap<>();

        // Add elements 
        linkedHashMap.put("bear", 1);
        linkedHashMap.put("apple", 2);

        // Iterating over map
        for (Map.Entry<String, Integer> entry : linkedHashMap.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + ": " + value);
        }
        //prints {bear=1}, then {apple=2}
    }
}
```
- insertion order is not affected if a key is reinserted
```
    // Add elements 
    linkedHashMap.put("bear", 1);
    linkedHashMap.put("apple", 2);
    linkedHashMap.put("bear", 3);
    ...
    //will print {bear=3}, then {apple=2}
```
- ```removeEldestEntry``` method can be overwritten to impose a policy for removing stale mappings automatically when new mappings are added to the map
```
int MAX_CAPACITY = 10;
LinkedHashMap<Integer, Integer> dic = new LinkedHashMap<>(5, 0.75f, true) {
    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > MAX_CAPACITY;
    }
};
```
