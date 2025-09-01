### The name rule C++ is name-hiding rule
It is not important that two variables the same name but differnt types.
```
int x;
void someFunc()[
	double x;
	std::cint >>x;
}
//global scope: x
// someFunc scope : x
```
The compiler must calculate a function/variable refering to.
It searchs from low scope to wide scope.
```
class Base{
private:
	int x; 
public:
	virtual void mf1()=0;
	virtual void mf1(int);
	virtual void mf2();
	void mf3();
	void mf3(double);
	...
};

class Derive:public Base{
public:
	virtual void mf1();
	void mf3();
	void mf4();
	...
};
```
Base scope:\
x,mf1(2 functions),mf2(1 function),mf3(2 functions.\
Derived scope:\
mf1(1 function),mf3(1 function),mf4(1 function).\
Base::mf1 and Base::mf3 are not already derived.
```
Derive d;
int x;
...
d.mf1();
d.mf1(x);//error
d.mf2();
d.mf3();
d.mf3(x);//error, Derived::mf3 covered the Base::mf3
```
Solution: use **using**
```
class Base{
private:
	int x; 
public:
	virtual void mf1()=0;
	virtual void mf1(int);
	virtual void mf2();
	void mf3();
	void mf3(double);
	...
};

class Derive:public Base{
public:
	using Base::mf1;
	using Base::mf3;
	virtual void mf1();
	void mf3();
	void mf4();
	...
};
d.mf1(x);//is ok, it calls Base::mf1.
```
Sometimes not all functions from base classes are to be extended.
This though violate the "is-a" principle.
Assum that Derived private extends Base, and it only wants to extend
the mf1() without parameter. 
Solution for it is an easy **forwarding function**
```
class Base{
private:
	int x; 
public:
	virtual void mf1()=0;
	virtual void mf1(int);
	...
};
class Derived:private Base{
public:
	virtual void mf1()
	{Base::mf1();}// implicit converse to inline see terms 30
	...
};
...
Derived d;
int x;
d.mf1();//ok call Derived::mf1
d.mf1(x);//error
```