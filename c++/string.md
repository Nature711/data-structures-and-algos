## Using ```char*``` (char array) vs ```std::string``` 
| Feature                      | char*                     | string                                      |
|------------------------------|---------------------------|---------------------------------------------|
| Type                         | C-style character array   | C++ standard library string class         |
| Memory Management            | Manual memory management | Automatic memory management (RAII)         |
| Null-termination             | Requires explicit '\0'    | Automatically null-terminated            |
| Mutability                   | Mutable                   | Mutable                                     |
| Length Calculation           | Manual (```strlen()``` function)  | Automatic (size() method)                  |
| Dynamic Resizing            | Manual (realloc function) | Automatic (handled by the string class)   |
| String Manipulation Methods | C-style functions         | Rich set of member functions               |
| Compatibility                | Lower-level, C-compatible | Modern C++ (preferred for C++ codebase)    |

Example:

```cpp
#include <iostream>
#include <string>
int main() {
    // Using char*
    char* charStr = new char[6];
    strcpy(charStr, "Hello");
    std::cout << "char* string: " << charStr << std::endl;
    delete[] charStr;  //manual deallocation

    // Using string
    std::string cppStr = "Hello";
    cppStr += " World";
    std::cout << "string: " << cppStr << std::endl; 
    //automatic RAII
    return 0;
}
```

## string methods
```
#include <iostream>
#include <string>

int main() {
    // Creating strings
    std::string str1 = "Hello";
    std::string str2 = " World";

    // Concatenation
    std::string result = str1 + str2;
    std::cout << "Concatenated string: " << result << std::endl;

    // Length of the string
    std::cout << "Length of str1: " << str1.length() << std::endl;

    // Accessing characters
    char firstChar = str1[0];
    std::cout << "First character of str1: " << firstChar << std::endl;

    // Substring
    std::string substr = str1.substr(1, 3);
    std::cout << "Substring of str1: " << substr << std::endl;

    // Finding a character or substring
    size_t foundIndex = str1.find("l");
    if (foundIndex != std::string::npos) {
        std::cout << "'l' found at index: " << foundIndex << std::endl;
    }

    // Comparing strings
    if (str1 == str2) {
        std::cout << "str1 is equal to str2" << std::endl;
    } else {
        std::cout << "str1 is not equal to str2" << std::endl;
    }

    // Converting to C-style string (const char*)
    const char* cString = str1.c_str();
    std::cout << "C-style string: " << cString << std::endl;

    return 0;
}
```