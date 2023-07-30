# Pointer Basics

## What & Why
- pointer = symbolic representation of addresses
- usages:
  - pass-by-reference
  - iterating over std containers
## How
### Declaring & initializing pointer
```
datatype var_name = val; //declare a variable var_name of type datatype 
datatype* ptr; //declare a pointer variable ptr that pointers to smth. of type datatype
ptr = &var_name; //the address of the variable is assigned to the pointer variable that pointers to the same data type
```
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/9678e048-a64f-4c50-aba3-3449d6f8ac87)

### Deferencing pointer
```
int var = 20;
int* ptr = &var;
cout << "Value at ptr = " << ptr << "\n";  //prints 0x7ffe454c08cc (address of variable var)
cout << "Value at var = " << var << "\n";  //prints 20 (value of variable var)
cout << "Value at *ptr = " << *ptr << "\n";  //prints 20 (deferencing pointer to get the value)
```

### Array & pointers
- An array name contains the address of the first element of the array which acts like a constant pointer.
  - the address stored in the array name can’t be changed
  - if we have an array named val, then val and &val[0] can be used interchangeably
```
  // Declare an array
  int val[3] = { 5, 10, 20 };

  // declare pointer variable
  int* ptr;

  // Assign the address of val[0] to ptr
  // We can use ptr=&val[0];(both are same)
  ptr = val;

  //prints 5 10 20 
  cout << ptr[0] << " " << ptr[1] << " " << ptr[2];
```

### Pointer arithmetic
- Pointer arithmetic is meaningless unless performed on an array
- The reason we associate data type with a pointer is that it knows how many bytes the data is stored in.
- When we increment a pointer, we increase the pointer by the size of the data type to which it points.
```
  for (int i = 0; i < 3; i++) {
      cout << "Value at ptr = " << ptr << "\n";  //prints address
      cout << "Value at *ptr = " << *ptr << "\n";  //prints value

      // Increment pointer ptr by 1 -- move on to the next array element
      ptr++;  
  }
```
![image](https://github.com/Nature711/data-structures-and-algos/assets/77217430/e0550501-7ff2-42b9-881a-66a66ba95907)
