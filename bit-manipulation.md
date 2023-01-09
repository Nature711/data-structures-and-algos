## Techniques

| Technique  | Code |
| ------------- | ------------- |
| Test k-th bit is set  | ```(num & (1 << k)) != 0```  |
| Set k-th bit  | ```num \|= (1 << k)```  |
| Turn off k-th bit  | ```num &= ~(1 << k)```  |
| Toggle the k-th bit  | ```num ^= (1 << k)```  |
| Multiply by 2^k  | ```num << k```  |
| Divide by 2^k  | ```num >> k```  |
| Check if a number is a power of 2  | ```(num & num - 1) == 0 or (num & (-num)) == num```  |
| Swapping two variables  | ```num1 ^= num2; num2 ^= num1; num1 ^= num2```  |

## Useful properties
![image](https://user-images.githubusercontent.com/77217430/211230817-7e6b1941-4cd2-4288-b433-29e5c289229d.png)

## Counting the number of 1s bit
```
public static int hammingWeight(int n) {
    int count = 0;
    for (int k = 0; k < 32; k++) {
        if ((n & (1 << k)) != 0) count++;
    }
    return count;
}
```
## [Single Number Series](https://github.com/Nature711/my-leetcode-notes/blob/master/single-number-bitwise.md)

