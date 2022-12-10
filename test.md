## Download WSL && Ubuntu
1. open ```Windows Powershell``` in administrative mode
2. install wsl and download ubuntu: ```wsl --install -d ubuntu```
3. Run the following commands:
   ```
   sudo apt update
   sudo apt upgrade
   ```

## Configure Git
```
git config --global user.name "test"
git config --global user.email test@email.com
```
- run ```git --list``` 
-> expected output: 
  ```
  user.name=test 
  user.email=test@email.com
  ```
## C++ Compiler
- approximate space required: 202MB
```
sudo apt install build-essential 
g++ --version ## verify installation
```

- Test file: ```hello.cc```
```
#include <iostream>
using namespace std;

int main() 
{
    cout << "Hello world!";
    return 0;
}
```
- compile: ```g++ -o hello hello.cc```
- run: ```./hello``` 
-> expected output: ```Hello world!```

## Python
```
sudo apt install python3
python3 --version
```

- Test file: ```hello.py```
```
print("Hello world")
```
- run: ```python3 hello.py```
--> expected output: ```Hello world!```

## NodeJS && NPM
- approximate space required: 92MB

```
sudo apt install nodejs npm
node -v && npm -v ## verify installation
```
