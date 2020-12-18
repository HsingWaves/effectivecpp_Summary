```
class Investment {...};

Investment* createInvestment();//factory function

void f()
{
	Investment* pInv = createInvestment();
	...
	delete pInv;
	
}
```
It looks everything fine here. But if there is return syntax in ...
such as **goto, continue.** How to avoid it?

### Using RAII objects.(Resources Acquisition Is Initialization)
They get the resources in construction and release them in deconstruction.
**The most frequent using classes are shred_ptr and auto_ptr.** The former is
more straightforward and copying makes latter point to null.
Example of using auto_ptr
```
void f()
{	
	//call factory function and use pInv 
	std::auto_ptr<Investment> pInv(createInvestment());
	...
	//through auto_ptr's deconstructor to delete object automatically
}
```
### Attention 1: Don't let auto_ptr point to more than one objects
To avoid this, auto_ptr has a special charateristic:
When copy_constructor or copy assignment to copy auto_ptr,it becomes **null**.
The copying pointer becomes only one owner of this resource.
```
std::auto_ptr<Investment> pInv(createInvestment());
// now pInv becomes null
std::auto_ptr<Investment>pInv2(pInv);

//now pInv2 becomes null
pInv = pInv2;
```
Solution:RCSP but it is not optimal:
**two unused objects point to each other, it looks like they are being used.**
```
void f()
{
	...
	std::trl::shared_ptr<Investment>
	pInv1(createInvestment());
	//trl is used not anymore direct using std::shared_ptr
	//they all point to the same object
	std::shared_ptr<Investment>pInv2(pInv1);
	pInv1 = pInv2;
	...
}
```
### Attention 2: auto_ptr and shared_ptr delete not delete[]
```
//they can be built but bad codes
std::auto_ptr<std::string>aps(new std::string[10]);
std::shared_ptr<int>spi(new int[1024]);
```

