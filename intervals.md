# Overlapping intervals
## Strategies
1. Sort intervals/pairs in increasing order of the start position.
2. Scan the sorted intervals, and maintain an "active set" for overlapping intervals. At most times, we do not need to use an explicit set to store them. Instead, we just need to maintain several key parameters, e.g. the number of overlapping intervals (count), the minimum ending point among all overlapping intervals (minEnd).
3. If the interval that we are currently checking overlaps with the active set, which can be characterized by cur.start > minEnd, we need to renew those key parameters or change some states.
4. If the current interval does not overlap with the active set, we just drop current active set, record some parameters, and create a new active set that contains the current interval.

## Examples
### [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

- Idea: 
1. Sort the intervals by starting point
2. Scan the sorted intervals and maintaining an active set -- a set of intervals which can be burst with a single arrow
   - to make sure this, we maintain the ```minEnd``` of the active set
   - whenever we encounter a new interval ```inv```, check:
     - if inv[0] <= minEnd, this means ```inv``` can be added to our active set, and update the minEnd of active set to be min(inv[1], minEnd) -- by applying an arrow to this updated minEnd, we're sure to shot all the balloons within the active set
     - if inv[0] > minEnd, we have to end our current active set by applying an arrow to burst all ballons within, then update the active set to inv
     
![image](https://user-images.githubusercontent.com/77217430/218090687-fa931df3-da71-488b-85a9-e9bf589628d8.png)
![image](https://user-images.githubusercontent.com/77217430/218090921-e797b8e6-b120-48bf-b53d-ba1d623f51e9.png)

```
   public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (p1, p2) -> Integer.compare(p1[0], p2[0])); //sort the 
        int count = 1;
        int minEnd = points[0][1];
        for (int[] inv: points) {
            if (inv[0] <= minEnd) {
                minEnd = Math.min(minEnd, inv[1]);
            } else {
                count++;
                minEnd = inv[1];
            }
        }
        return count;
    }
```

### Non-overlapping intervals()
- if ```invB``` comes after ```invA``` in the sorted intervals, and ```invB``` overlaps with ```invA``` (i.e., invB[0] < invA[1]), we need to remove one of them
- which one to remove? the one with the greater ending point -- which is likely to overlap with more intervals in the future 
- therefore the one remained is the one with smaller ending point, i.e., ```maxEnd``` is updated to ```min(maxEnd, intervals[i][1]);```
```
   public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (i1, i2) -> Integer.compare(i1[0], i2[0]));
        int maxEnd = intervals[0][1];
        int removeCount = 0;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < maxEnd) {
                removeCount++;
                maxEnd = Math.min(maxEnd, intervals[i][1]);
            } else {
                maxEnd = intervals[i][1];
            }
        }
        return removeCount;
    }
```


## Insert interval
```
public int[][] insert(int[][] intervals, int[] newInterval) {

    List<int[]> res = new ArrayList<>();
    int i = 0, n = intervals.length;

    while (i < n && intervals[i][1] < newInterval[0]) res.add(intervals[i++]);

    while (i < n && newInterval[1] >= intervals[i][0]) {
        newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
        newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
        i++;
    }
    res.add(newInterval);

    while (i < n) res.add(intervals[i++]);

    int[][] result = new int[res.size()][2];
    for (int j = 0; j < res.size(); j++) result[j] = res.get(j);
    return result;
}
```

## Merge intervals
```
public int[][] merge(int[][] intervals) {
        
        Arrays.sort(intervals, (i1, i2) -> i1[0] - i2[0]);
        
        List<int[]> res = new ArrayList<>();
        int[] newInterval = intervals[0];
        int n = intervals.length;
        
        for (int i = 1; i <= n; i++) {
            while (i < n && intervals[i][0] <= newInterval[1]) {
                newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
                i++;
            }
            res.add(newInterval);
            if (i < n) newInterval = intervals[i];
        }
        
        int[][] result = new int[res.size()][2];
        for (int i = 0; i < res.size(); i++) result[i] = res.get(i);
        return result;
    }
}
```
