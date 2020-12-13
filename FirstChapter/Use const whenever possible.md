## const is useful keyword
const can declare function/variable/parameter/pointer/block scope/object...
if const on the **left side of the * ** the **variable** is const
```void f1(const Widget* pw);
   void f2(Widget const * pw);
are the same. f1 and f2 get a pointer,which **points to a constant**
```
if const on the **right side of the * ** the **pointer** is const

### Attention 1: const member function
1. Make class interface easy to be understood, which function can change content of object
2. Make "operating const" possible

### Atttention 2: logical constness and bitwise constness
```class CTextBlock{
public:
  ....
  std::size_t length() const;
private:
  char* pText;
  std::size_t textLength;
  bool lengthIsValid;
  };
 
std::size_t CTexBlock::length() const{
  if(!lengthIsValid){
  textLenght = std::strlen(pText); // **error**
  lengthIsValid = true;           //**error const member function cannot assign value to variable**
  }
  return textLength;
}
```
But if using **mutable** keyword
 ``` mutable std::size_t textLength;
     mutable bool lengthIsValid;
```
There is no error.   
### Attention3: In const and non-const member funciton avoiding duplicate
In operator[] perform **boudns checking, log access data, verify data integrity**
```
class TextBlock{
public:
...
const char& operator[](std::size_t position) const{
      ....//boudns checking
      ....//log access data
      ....//verify data integrity
      return text[position]
}

const char& operator[](std::size_t position) {
      return 
      **const_cast<char&>(static_cast<const TextBlock&>(*this)[position]);** //using casting 2 times here:1. show that perform const operator[], 
      //*this is TextBlock& type cast to const TextBlock& 2.remove the const
                                                                           
}



