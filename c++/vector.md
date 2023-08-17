## Vector methods

1. `push_back()`: Add an element to the end of vector
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> myVector;

    // Adding elements using push_back()
    myVector.push_back(1);
    myVector.push_back(2);
    myVector.push_back(3);

    //myVector: {1, 2, 3}

    return 0;
}
```

2. `pop_back()`: Remove the element from the end of the vector (i.e., last element)
```cpp
myVector.pop_back();

//myVector: {1, 2}
```

3.  `size()`: Get the size of the vector.
```cpp
myVector.size(); //prints 2
```

4. `resize()`: Change the size of the vector. If the new size is greater, new elements will be default-initialized, and if the new size is smaller, elements at the end will be removed.
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> myVector = {1, 2, 3};

    // Resize the vector to a larger size
    myVector.resize(5);

    //myVector: {1, 2, 3, 0, 0}

    // Resize the vector to a smaller size
    myVector.resize(2);

    /.myVector: {1, 2}

    return 0;
}
```

5. `clear()`: Remove all elements from the vector.
```cpp
myVector.clear();

//myVector: {}
```

6. `emplace_back()`: Construct elements directly in the vector, avoiding extra copies or moves.
```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
    MyClass(int val) : value(val) {}

    int getValue() const {
        return value;
    }

private:
    int value;
};

int main() {
    std::vector<MyClass> myVector;

    // Using emplace_back() to construct elements directly in the vector
    myVector.emplace_back(1);
    myVector.emplace_back(2);
    myVector.emplace_back(3);

    // Print the elements in the vector
    for (const auto& obj : myVector) {
        std::cout << obj.getValue() << " ";
    }

    return 0;
}
```

## 2D vectors
- declares a 2D vector with m rows & n cols, initialize to 0
```
vector<vector<int>> vec(m, vector<int>(n, 0));
```

## Notes
### ```push_back``` vs ```emplace_back```
- ```push_back()```
  -  add elements to the end of the vector
  -  need to provide an object of the vector's element type
  -  may involve unnessasry copying / moving if vector stores class objects
  -  use case: when you have pre-existing objects and want to store copies of them in the vector
  -  e.g.,
    ```cpp
    std::vector<int> myVector;
    int x = 42;
    myVector.push_back(x); // Copies the value of x into the vector
    ```
- ```emplace_back()```
  - construct elements directly in the vector, avoiding unnecessary copying or moving
  - forwards the arguments directly to the constructor of the object stored in the vector --> constructing the object in place within the vector
  - use case: when you want to avoid unnecessary copies or moves and construct objects directly within the vector
  - more efficient when working with objects that have expensive copy / move 
  - e.g.,
    ```cpp
    std::vector<std::string> myVector;
    myVector.emplace_back("Hello"); // Constructs the string "Hello" in place within the vector
    ```

### more on ```push_back```
- When you push a customized class object into a vector using push_back(), the vector stores a copy of that object.
- Modifications made to the object inside the vector will not affect the original copy. 
```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
    MyClass(int val) : value(val) {}

    void setValue(int val) {
        value = val;
    }

    int getValue() const {
        return value;
    }

private:
    int value;
};

int main() {
    std::vector<MyClass> myVector;

    MyClass obj1(10);
    myVector.push_back(obj1);

    // Modify the object inside the vector
    myVector[0].setValue(20);

    // Original object remains unchanged
    std::cout << "Original object value: " << obj1.getValue() << std::endl; // Output: Original object value: 10

    // Object inside the vector has been modified
    std::cout << "Object in the vector value: " << myVector[0].getValue() << std::endl; // Output: Object in the vector value: 20

    return 0;
}
```

- If you want to modify the original object directly --> use pointer to access the elements using iterators
```cpp
int main() {
    std::vector<MyClass*> myVector; // Use pointers to store objects

    MyClass obj1(10);
    myVector.push_back(&obj1); // Store the address of obj1 in the vector

    // Modify the original object using an iterator
    for (auto it = myVector.begin(); it != myVector.end(); ++it) {
        if ((*it)->getValue() == 10) {
            (*it)->setValue(20); // Modify the object through the pointer
            break;
        }
    }

    std::cout << "Modified object value: " << obj1.getValue() << std::endl; // Output: Modified object value: 20

    return 0;
}
```

## Passing vector to function

### Pass by value
- create a copy of the vector
- the new copy is passed to function --> any changes made to the (new) vector in the function do not affect the original vector
```cpp
#include <bits/stdc++.h>
using namespace std;

// The vect here is a copy of vect in main()
void func(vector<int> vect) { vect.push_back(30); }

int main()
{
	vector<int> vect;
	vect.push_back(10);
	vect.push_back(20);

	func(vect);

	// vect remains unchanged after function call
    //prints 10, 20
	for (int i = 0; i < vect.size(); i++)
		cout << vect[i] << " ";

	return 0;
}

```
### Pass by reference
- referenced & original vector has the same address --> change to the original vector affects the new one
```cpp
#include <bits/stdc++.h>
using namespace std;

void func(vector<int>& vect) { vect.push_back(30); }

int main()
{
	vector<int> vect;
	vect.push_back(10);
	vect.push_back(20);

	func(vect);

    //prints 10, 20, 30
	for (int i = 0; i < vect.size(); i++)
		cout << vect[i] << " ";

	return 0;
}
```

## Copy vector
### Iterative method
- use a loop to ```push_back()``` the old vector elements into the new vector
- deep copy
- change in the original vector doesn't affect the new copy
```cpp
// C++ code to demonstrate copy of vector
// by iterative method.
#include<iostream>
#include<vector>
using namespace std;

int main()
{
	// Initializing vector with values
	vector<int> vect1{1, 2, 3, 4};

	// Declaring new vector
	vector<int> vect2;

	// A loop to copy elements of old vector into new vector iteratively
	for (int i=0; i<vect1.size(); i++)
		vect2.push_back(vect1[i]);

	cout << "Old vector elements are : "; //prints 1,2,3,4
	for (int i=0; i<vect1.size(); i++)
		cout << vect1[i] << " ";
	cout << endl;

	cout << "New vector elements are : "; //prints 1,2,3,4
	for (int i=0; i<vect2.size(); i++)
		cout << vect2[i] << " ";
	cout<< endl;

	vect1[0] = 2;

	cout << "The first element of old vector is :"; //prints 2
	cout << vect1[0] << endl; 
	cout << "The first element of new vector is :"; //prints 1
	cout << vect2[0] <<endl;

	return 0;
}
```

### Assingment operator
- same effect as copy constructor
- independent copies --> change in one vector doesn't affect the other
- note: assignment is not possible for arrays
```cpp
// Using assignment operator to copy one vector to other
vect2 = vect1;

vect1[0] = 2;

cout << "The first element of old vector is :"; //prints 2
cout << vect1[0] << endl;
cout << "The first element of new vector is :"; //prints 1
cout << vect2[0] <<endl;
```

### Pass into constructor
- at the time of declaration of vector, passing an old initialized vector copies the elements of the passed vector into the newly declared vector
- deep copy
```cpp
vector<int> vect2(vect1); 

vect1[0] = 2; 

cout << "The first element of old vector is :";  //prints 2
cout << vect1[0] << endl; 
cout << "The first element of new vector is :";  //prints 1
cout << vect2[0] <<endl; 
```
