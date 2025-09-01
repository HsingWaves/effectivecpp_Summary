The following code shows an obivious error:
```
class B{
public:
	virtual void f()const;
};

class D:public B{
public:
	virtual void f(){
];
```
Here D wants to redefine B::f, but f in B is a const function.
**Warning:** D::f() hides virtual B::f()
This warning is trying to tell that B::f was not redeclared, rather it was 
covered. 
**Be serious about the warnings from compiler, try to write code without any 
warnings.** 