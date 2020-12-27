### In C++ coder should manage the memory manaully
On the contrary, Java and .NET has their internal GC ability.
Memory management includes two terms: allocation and deallocation routines,
namely operator new, operator delete. When operator new is not statisfied,
new handler is called.\
Heap is global modified resources. In STL heap memory is managed by allocator 
objects, not by new and delete. This chapter discuss STL allcator not.
When operator new cannot statisfies a memory allocation request, it will throw
an exception, in the past it would throw NULL pointer. In first, it will call an
error handler function specified by client.
```
namesapce std{
	typedef void (*new_handler)();
	new_handler set_new_handler(new_handler p) throw();
}
```
ne handler is typedef, defining a pointer to function. This function doesn't have
parameter and returns nothing.
```
void outOfMem(){
	//when request of memory doesn't satisfy
	cerr<<"Unable to satisfy request for memory\n";
	abort();
}

int main(){
	set_new_handler(outOfMem);
	int* pBigDataArray = new int[1000000000l];
	...
};
//The program will sends a message and abort.
```
A good designed new-handler should have following characteristics:\
1.make more memory can be used. A big memory is distributed as program executing.
2.install another new-handler.\
3.delete new-handler, namely pass null pointer to set_new_handler.\
4.throw bad_alloc exception, which is not caught by operator new.\
5.doesn't retrun, usually using abort and exit.
```
template<typename T>
class NewHandlerSupport{
public:
	static std::new_handler set_new_handler(std::new_handler p)throw();
	static void* operator new(std::size_t size)throw(std::bad_alloc);
	...
private:
	static std::new_handler currentHandler;
};

template<typename T>
std::new_handler
NewHandlerSupport<T>::set_new_handler(std::new_handler p )throw()
{
	std::new_handler oldHandler = currentHandler;
	currentHandler = p;
	return oldHandler;
}

template<typename T>
void* NewHandlerSupport<T>::operator new(std::size_t size)
throw(std::bad_alloc)
{
	NewHandlerSupport h(std::set_new_handler(currentHandler));
	return ::operator new(size);
}
template<typename T>
std::new_handler NewHandlerSupport<T>::currentHandler = 0;

//With this class template, it is easy for Widget to add 
//set_new_handler

class Widget:public NewHandlerSupport<Widget>{
//doesn't need to declare set_new_handler and operator new anymore
public:
	//static std::new_handler set_new_handler(std::new_handler p)throw();
	//static void* operator new(std::size_t size) throw(std::bad_alloc);
private:

	//static std::new_handler currentHandler;
};
```
	

