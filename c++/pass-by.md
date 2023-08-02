## Pass by pointer

- the memory location (address) of the variables is passed
```cpp
#include <iostream>
using namespace std;

void swap(int *x, int *y)  //function signature: expects pointer as parameter
{
	int z = *x;
	*x = *y;
	*y = z;
}

int main()
{
	int a = 45, b = 35;
	cout << "Before Swap\n";
	cout << "a = " << a << " b = " << b << "\n"; //prints a = 45, b = 35

	swap(&a, &b); //pass by pointer (i.e., pass the address of the variable to function)

	cout << "After Swap with pass by pointer\n";
	cout << "a = " << a << " b = " << b << "\n"; //prints a = 35, b = 45
}
```

## Pass by reference 
- allows a function to modify a variable without having to create a copy of it
- have to declare reference variables
- the memory location of the passed variable and parameter is the same --> any change to the parameter reflects in the variable as well
```cpp
#include <iostream>
using namespace std;
void swap(int& x, int& y)  //function signature: expects reference as parameters
{
	int z = x;
	x = y;
	y = z;
}

int main()
{
	int a = 45, b = 35;
	cout << "Before Swap\n";
	cout << "a = " << a << " b = " << b << "\n"; //prints a = 45, b = 35
 
	swap(a, b); //p

	cout << "After Swap with pass by reference\n";
	cout << "a = " << a << " b = " << b << "\n"; //prints a = 35, b = 45
}
```

## Comparison

| Parameter             | Pass by Pointer                | Pass by Reference             |
|-----------------------|--------------------------------|-------------------------------|
| Passing Arguments     | Pass the address of arguments  | Pass the arguments directly   |
| Accessing Values      | Use dereferencing operator *   | Use the reference name        |
| Reassignment          | Parameters can be reassigned   | Parameters cannot be reassigned |
| Allowed Values        | Can contain a NULL value       | Cannot contain a NULL value   |
| Use cases             | When "reseating" is needed     | Preferred when "reseating" is not needed |

- notes:
   - reseating = changing the parameter to refer to a different object)
   - use references when you can, use pointers when you have to

