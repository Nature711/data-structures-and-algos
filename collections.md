## Valid Parenthesis

```([{()}])```
- observations: 
  - input stream comes in order
  - want to process a match once it occurs
  - delay evaluation of open parenthesis that hasn't yet been matched (corresponding matching closing parenthesis may occur later)
- data structure used: stack

## Two Sum
- observations: 
  - input stream comes without order 
  - want to find matching (complement) on the fly
- data structure used: hash table 

## Longest Palindrome by Concatenating Two Letter Words
```
 public int longestPalindrome(String[] words) {
        int n = words.length;
        HashMap<String, Integer> map = new HashMap<>();
        int res = 0;
        for (String word: words) {
            if (map.containsKey(word)) {
                //somewhere before we've encountered the reverse of this word
                res += 4;
                int rem = map.get(word) - 1;
                if (rem == 0) map.remove(word);
                else map.put(word, rem);
            } else {
                String rev = word.substring(1,2) + word.substring(0,1);
                map.put(rev, map.getOrDefault(rev, 0) + 1);
            }
        }
        
        for (String word: map.keySet()) {
            if (word.charAt(0) == word.charAt(1)) {
                res += 2;
                break;
            }
        }
        return res;
    }
}
```
