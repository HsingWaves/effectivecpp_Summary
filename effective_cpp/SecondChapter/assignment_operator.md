### Chain assignment:Right associative law
 ```
 Widget& operator=(const Widget& rhs)
  {
      ...
      return *this;
  }
  ...
  ```
### Attention: assignment return a reference to *this
