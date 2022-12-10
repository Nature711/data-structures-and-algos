## Data structure

- commonly implemented using a priority queue: 
  - default: ```PriorityQueue<T> pq = new PriorityQueue<>();```
      - 💡 the default pq for int arranges the elements in ascending order (i.e., min element has highest priority)
      - with customized comparator: ```PriorityQueue<T> pq = new PriorityQueue<>(comparator);```
        - min element has higher priority (default): ```(a, b) -> a - b```
        - max element has higher priority: ```(a, b) -> b - a```
        - smaller first element in pair has higher priority: ```(p1, p2) -> p1.getKey() - p2.getKey()```
        - smaller second element in pair has higher priority: ```(p1, p2) -> p1.getValue() - p2.getValue()```

## Use cases 1

### finding the max / min element while modifying the set on the fly

- problem: every time we want to find the min / max element from a set of numbers, but are modifying the set on the fly
- sorting won't solve the problem since it's just done once at the start; the modification on the fly would break the sorted sequence; the only fix is to sort every time whichi takes O(NlogN)
- solution: min / max heap -- find min / min in O(1) since we just pop from top; for any modification the rebalance can be achieved in O(logN)
- example: [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)

### finding "top K frequent"

- example: [Top K frequent elements](https://leetcode.com/problems/top-k-frequent-elements/s)

