 Looking at example
```
std::shared_ptr<Investment>pInv(createInvestment());

int daysHeld(const Investment* pi);//return investing days

int days = daysHeld(pInv);//error beacause pInv is std::shared_ptr<Investment>
//not the Investment* type

int days = daysHeld(pInv.get());//great, make the raw pointer to daysHeld
```

### attention 1: APIs usually ask raw resources, so RAII class should offer path to it

### attention 2: Explicit conversion safe, Implicit conversion convenient
```
class Font{
public:	
	...
	operator FontHandle() const //Implicit conversion
	FontHandle get() const{return f}; //explicit conversion
	{return f;}
	...
};
```