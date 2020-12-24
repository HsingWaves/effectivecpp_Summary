### sometimes class and typename are not seen as equal
```
template<typename C>
void print2nd(const C& container)
{
	if(container.size() >= 2){
	C::const_iterator iter(container.begin());
	++iter;
	int value = *iter;
	std::cout<<value;
	}
}
```
The above is not valid code.C::const_iterator is nested dependent type name,
which is subordinate to a template parameter.
Before C is known, nobody knows what is the type fo C::const_iterator
Solution:
```
template<typename C> //can use typename or class
void print2nd(const c& container) //not allow to use typename
{
	if(container.size() >= 2) {
	//must use typename
	typename C::const_iterator iter(container.begin());//using
		//the typename syntax
	...
	}
}
```
C is not nested dependent. So it cannot be declared typename.
Exception for nested dependent type is that
```
template<typename T>
class Derived:public Base<T>::Nested{// In base class list 
public:								// not allow typename
	explicit Derived(int x)
	:Base<T>::Nested(x)			//mem. init. list not allow
	{							//typename and nested dependent type
		
		//but here soll add typename as base class decorator
		typename Base<T>::Nested temp;
		...
	}
	...
};
// This rule make people angry!
```
The most common case:
```
template<typename IterT>
void workWithIterator(IterT iter)
{
	typename std::iterator_traits<IterT>::value_type temp(*iter);
	...
	//it is variation or complicated form
	typedef typename std::iterator_traits<IterT>::value_type value_type;
	value_type temp(*iter);
}
```
here is just declare this long sentence is a type of IterT.

