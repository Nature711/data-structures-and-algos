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


while (carry is not ZERO)

iteration 1
```
update sum 
  101             
^ 011
-----
  110   <- sum 
  
update carry
  101             
& 011
-----
  001   <- carry
 0010   <- actual carry
```
iteration 2
```
update sum 
  0110             
^ 0010
------
  0100   <- sum 
  
update carry
  0110             
& 0010
------
  0010   <- carry
  0100   <- actual carry
```
iteration 3
```
update sum 
  0100             
^ 0100
------
  0000   <- sum 
  
update carry
  0100             
& 0100
------
  0100   <- carry
  1000   <- actual carry
```
iteration 4
```
update sum 
  0000             
^ 1000
------
  1000   <- sum 
  
update carry
  0000             
& 1000
------
  1000   <- carry
  0000   <- actual carry == 0
```

```
import java.math.BigInteger;
class Solution {
  public String addBinary(String a, String b) {
    BigInteger x = new BigInteger(a, 2);
    BigInteger y = new BigInteger(b, 2);
    BigInteger zero = new BigInteger("0", 2);
    BigInteger carry, answer;
    while (y.compareTo(zero) != 0) {
      answer = x.xor(y);
      carry = x.and(y).shiftLeft(1);
      x = answer;
      y = carry;
    }
    return x.toString(2);
  }
}
```
