 ### casting in C++ is unsafe.
C++ offers four new_type casting:
```
 const_cast<T>(expression)
 dynamic_cast<T>(expression)
 reinterpret_cast<T>(expression)
 static_cast<T>(expression)
```
const_cast: cast away(remove) constness or volatility.
```
    int i = 3;                 // i is not declared const
    const int& rci = i; 
    const_cast<int&>(rci) = 4; // OK: modifies i
	//i=4;
	//if const int i = 3; i will not change
```
```
	const int j = 3; // j is declared const, const is only the int
	int* pj = const_cast<int*>(&j);
	*pj = 4;
    std::cout << *pj;//4
	std::cout<< j; //j=3
```
dynamic_cast: execute **safe downcasting**, cost huge
reinterpret_cast: execute low casting, actual actions depend on compiler
static_cast: push implicit conversions. For example: non-const to const
void* to typed. But it cannot execute const to non-const.

The new casting is welcomed for they are easy to be recognized.
```	
class Base{...};
class Derived:public Base{...};
Derived d; //
Base* pb = &d; //the pb and &d are the same, an objects can have two address
//variables, it is special in C++ and not advocated.
```
Some code is interesting by casting
```
class Window{
public:	
	virtual void onResize(){...}
	...
};
class SpecialWindow: public Window{
public:
	virtual void onResize(){
	static_cast<Window>(*this).onResize();// not allowed here
	
	}
	...
};
```
Solution:
```
class SpecialWindow: public Window{
public:
	virtual void onResize(){
	Window::onResize();// not allowed here
	
	}
	...
};

*** Three tips for that**
1. Try to avoid casting.
2. Hide the casting behind one function.
3. prefer new_type casting than direct using()


 