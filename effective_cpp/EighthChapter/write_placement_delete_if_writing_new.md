```
Widget* pw = new Widget;
```
Two functions are called, one is for allocating operator new, one for Widget
default function. Assume the former is successful and the latter fails. Client 
has no ability to return memory
```
void* operator new(std::size_t) throw(std::bad_alloc);
```
refers to normal operator delete:
```
void operator delete(void* rawMemory) throw();
void operator delete(void* rawMemory,std::size_t size)throw();
```
Solution for memory overflows:
```
class Widget{
public:
	...
	static void* operator new(std::size_t size, std::ostream& logStream)
		throw(std::bad_alloc);
	static void operator delete(void* pMemory) throw();
	static void operator delete(void* pMemory,std::ostream& logStream)
		throw();
	...
};

//now , there is no overflow:
Widget* pw = new (std::err)Widget;

//but it will call normal delete:
delete pw;
```
The easy to solve this is to build a base class:
```
class StandardNewDeleteForms{
public:
	//normal new/delete
	static void* operator new(std::size_t size) throw(std::bad_alloc)
	{	return ::operator new(size);}
	static void operator delete(void* pMemory) throw()
	{::operator delete(pMemory);}
	
	//placement new/delete
	static void* operator new(std::size_t size, void* ptr)throw()
	{return ::operator new(size,ptr);}
	
	static void operator delete(void* pMemory,void* ptr)throw()
	{return ::operator delete(pMemory,ptr);}
	
	static void* operator new(std::size_t size,const std::nothrow_t& nt)throw()
	{return ::operator new(size,nt);}
	
	static void operator delete(void* pMemory,const std::nothrow_t&)throw()
	{::operator delete(pMemory);}
};
```
For each client who wants self-defined, can use using declaration:
```
class Widget:public StandardNewDeleteForms{
public:
	using StandardNewDeleteForms::operator new;
	using StandardNewDeleteForms::operator delete;
	
	static void* operator new(std::size_t size,
									std::ostream& logStream)
	throw(std::bad_alloc);
	static void operator delete(void* pMemory,
									std::ostream& logStream)
		throw();
	...
};
```
If you decalre placement new and placement delete ,dont' forget to cover
its default version.