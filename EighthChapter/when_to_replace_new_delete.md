 First of all, why default operator new and delete need to be replaced by :\
 1.To test error.\
 2.To be more efficient.\
 3.To collect statistical data\
 An pseudo operator new is as follows:
 ```
 static const int signature = 0xDEADBEEF;
 typedef unsigned char Byte;
 
 void* operator new(std::size_t size) throw(std::bad_alloc)
 {
	using namespace std;
	//add it, make it can hold two signatures
	size_t realSize = size +2*sizeof(int);
	//call malloc for memory 
	void* pMem = malloc(realSize);
	if(!pMem) throw bad_alloc();
	
	//write signature into memeory first and last paragraph
	*(static_cast<int*>(pMem)) = signature;
	*(reinterpret_cast<int*>(static_cast<Byte*>(pMem)
		+realSize-sizeof(int))) = signature;
	//return pointer, refers to the first position after first signature
	return static_cast<Byte*>(pMem) + sizeof(int);
}
```
It is limited to **alignment,** which is meaningful in C++, as it asks all
operator new pointers should have proper alignment.
The above code returns only a int size pointer. Nobody can ensure its security.
It should return a pointer from malloc.

