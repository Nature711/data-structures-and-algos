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
## Add Binary

- when we perform the bitwise AND operation between two binary digits, we get a result of 1 only if both digits are 1. In this case, we need to carry a 1 to the next column. Shifting the result of the bitwise AND operation to the left by 1 bit is equivalent to multiplying it by 2, which effectively moves the carry to the next column.

update sum
```
  101
+ 011
-----
  110   <- sum without carry (using bitwise XOR)
```
update carry
```
  101
+ 011
-----
  001   <- carry without sum (using bitwise AND)
  
  010   <- then shift left by 1 bit
```
while (carry is not ZERO)
update sum
```
  110
+ 010
-----
  100   <- sum xor carry
```
update carry
```
  110
+ 010
-----
  010   <- temp and carry
  
  100   <- left shift by 1 bit
```
```
import java.math.BigInteger;
class Solution {
    public String addBinary(String a, String b) {
        
        BigInteger aa = new BigInteger(a, 2);
        BigInteger bb = new BigInteger(b, 2);
        BigInteger sum = aa.xor(bb);
        BigInteger carry = aa.and(bb).shiftLeft(1);
        
        while (!carry.equals(BigInteger.ZERO)) {
            BigInteger temp = sum;
            sum = sum.xor(carry);
            carry = temp.and(carry).shiftLeft(1);
        }
        
        return sum.toString(2);
    }
}
```
