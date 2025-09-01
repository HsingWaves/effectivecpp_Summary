### Not all resources are heap-based
For theses resources auto_ptr and shared_ptr are not suitable for
handling resources. Sometimes it needs build up your own resources
management classes.

### Attention 1: Copying RAII object, it should copy all its resources
Now it has two functions:
```
void lock(Mutex* pm); //lock the Mutex pm points to 
void unlock(Mutex* pm);//unlock 

class Lock{
public:
	explicit Lock(Mutex* pm):mutexPtr(pm)
	{lock(mutexPtr);} // get resources
	
	~Lock(){unlock(mutexPtr);}// release resources
private:
	Mutex *mutexPtr;
};

```	
The client usage meets the RAII:
```
Mutex m;
...
{
Lock m1(&m);
	...
}
```
But what if Lock object is copied?
```
Lock m11(&m);
Lock m12(m11);
```

### Attention 2: Universal RAII class copying behaviors are: 
supreess copying, executing references counting.
supress copying:
```
class Lock:private: Uncopyable{
public:
	...
};
```

executing references counting:
```
class Lock{
public:
	explicit Lock(Mutex* pm):mutexPtr(pm,unlock)
	{	
		lock(mutexPtr.get());
	}
private:
//shared_ptr/mutexPtr deconstructor will auto call deleter if reference equals 0
	std::shared_ptr<Mutex>mutexPtr;
};
```