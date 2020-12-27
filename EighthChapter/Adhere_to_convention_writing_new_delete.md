### When writing operator new and operator delete, which rules should be followed?
You cannot support in Base::operator new[] each element is sizeof(Base).
About operator delete is only one thing to memorize:delete null pointer forever
secure.
```
class Base{
public:
	static void* operator new(std::size_t size)throw (std::bad_alloc);
	static void operator delete(void* rawMemory, std::size_t size)throw();
	...
};

void Base::operator delete(void* rawMemory,std::size_t size)throw()
{
	if(rawMemory == 0)return;
	if(size!=sizeof(Base)){
		::operator delete(rawMemory);
		return;
	}
	return;
}
```
In operator new it should have an unending loop, distributing memory inside.
If it is not satisfied, it should call new-handler.