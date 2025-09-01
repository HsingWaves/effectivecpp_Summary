### Aliasing problem: px and py point to the same object
If trying to self manage the resources, may delete the resource in advance

Alternativ solution:
```
class Widget{
...
void swap(Widget& rhs);//swap the *this and rhs data
...
};
Widget& Widget::operator=(const Widget& rhs)
{
  Widget temp(rhs); //copy rhs
  swap(temp);       
  return *this;
 }
 
 ```
**This design method sacrifices clarity but brings efficiency** 
