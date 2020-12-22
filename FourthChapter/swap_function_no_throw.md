 ### swap is part of stl, become the most part of exception-safe programming
 ```
 //basic swap function 
 namespace std{
	template<typename T>
	void swap(T& a,T& b){
	T temp(a);
	a = b;
	b = temp;
}
```
Only when T support copying.
It involved three objects copying:
a to temp,b to a, temp to a.
But for some types, these copying are not necessary.
These types are that using pimpl(pointer to implementation)
```
class WidgetImpl{
public:
	...			
private:
	int a,b,c;			//may contain a lot of data	
	std::vector<double>v;// copying means that long time
	...
};

class Widget{
public:
	Widget(const Widget& rhs);
	Widget& operator = (const Widget& rhs){	//copy Widget make it
	...										//copy WidgetImpl object	
	*pImpl = *(rhs.pimpl);
	...
	}
	...
priavte:
	WidgetImpl* pImpl;
};
```
When using general mode to swap, it copys not only three Widgets but also
three WidgetImpl
Solution:define a swap function in Widget.
```
class Widget{
public:
	...
	void swap(Widget& other){
	using std::swap;//declaration is necessary here
	swap(pImpl,other.pImpl);
	}
	...
};
namespace std{
	temp<>
	void swap<Widget>(Widget&a, Widget& b){
	a.swap(b);
	}
}
```
But when Widget and WidgetImpl are not classes, rather class template:
```
template<typename T>
class Widget{...};

namespace std{
	template<typename T>
	void swap<Widget<T>>(Widget<T>& a,Widget& b){
			a.swap(b);
	}//error, invalid
}
```
Solution: declare a non-member swap 
```
namespace WidgetStuff{
	...
	template<typename T>
	class Widget{...};
	...
	template<typename T>
	void swap(Widget<T>& a,
			  Widget<T>& b)
	{
		a.swap(b); //here dont belong to std namespace
	}
}
```
### Attention: member swap cannot throw exception-safe

