### The so-called smart pointers are objects behaving like pointers
The real pointer make a good job for impilicit conversion:
```
class Top{...};
class Middle:public Top{...};
class Bottom:public Middle{...};
Top* pt1 = new Middle;	//Middle* to Top*
Top* pt2 = new Bottom;	//Bottom* to Top*
const Top* pct2 = pt1;	//Top* to const Top*
```
It is complicated to execute conversion by smart pointer
```
template<typename T>
class SmartPtr{
	public:
	explicit SmartPtr(T* realPtr);
	...
};

SmartPtr<Top> pt1 = SmartPtr<Middle>(new Middle);
SmartPtr<Top> pt2 = SmartPtr<Bottom>(new Bottom);
SmartPtr<const Top>pct2 = pt2;
```
The compiler sees SmartPtr<Middle> and SmartPtr<Top> as different types, they 
don't have based-derived relationship.

```
template<typename T>
class SmartPtr{
public:
	template<typename U>
	SmartPtr(const SmartPtr<U>& other);
	...
};
```
The is called generalized copy constructor.
**If member templates are used for generalized copy or generalized assignment,
normal copying construtor and copy assignment are also needed.**

```
template<class T>
class shared_ptr{
public:
	shared_ptr(shared_ptr const& r);
	template<class Y>
	shared_ptr(shared_ptr<Y> const &r);
	template<class Y>
	shared_ptr& operator = (shared_ptr<Y> const& r)
	...
};
```
