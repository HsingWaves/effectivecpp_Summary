## Compiler can sliently create default constructor, copy constructor, copy assignment = operator,and deconstructor

### Attention: reference cannot change to another object
```
  std::string newDog("Psoh");
  std::string oldDog("Stach");
  NameObject<int>p(newDog, 2);
  
  NameObejct<int>s(oldDog,36);
  
  p =s;//compiler error
```

