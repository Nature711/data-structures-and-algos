## Template
- [Question](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
```
  public int strStr(String haystack, String needle) {
        
        if (haystack.length() < needle.length()) return -1;
        
        int patternHash = 0;
        int windowSize = needle.length();
        for (int i = 0; i < windowSize; i++) {
            patternHash += (int) Math.pow(2, windowSize - i - 1) * (int) needle.charAt(i);
        }
        
        int currWindowHash = 0;
        for (int i = 0; i < windowSize; i++) {
            currWindowHash += (int) Math.pow(2, windowSize - i - 1) * (int) haystack.charAt(i);
        }
        if (currWindowHash == patternHash) return 0;
        
        for (int base = 1; base < haystack.length() - windowSize + 1; base++) {
            currWindowHash = 2 * (currWindowHash - (int) Math.pow(2, windowSize - 1) * (int) haystack.charAt(base - 1)) + haystack.charAt(base + windowSize - 1);
            if (currWindowHash == patternHash) return base;
        }
        
        return -1;
        
    }
```
