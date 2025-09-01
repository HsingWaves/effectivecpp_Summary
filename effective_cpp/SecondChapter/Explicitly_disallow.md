## Some objects are unique, it is willing to see the copy operation fail

### Attention 1: To turn down the silent funtions from compiler, these unwilling functions are set to private

### Attention 2: extend from Uncopyable is executable
```
  class HomeForSale: private Uncopyable{
  
  };
// Multiple inheritance is inevitable
  
```
