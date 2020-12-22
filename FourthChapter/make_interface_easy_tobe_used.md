### Make an easy to be used correct interface
```
class Date{
	Date(int month, int day, int year};
}
```
It looks like very reasonable. But it lacks restrictions.

```
struct Day{							struct Month{ 					
	explicit Day(int d):val(d){}     explicit Month(int m):val(d){}
	int val;						 int val;	
};                                  }
	
struct Year{
	explicit Year(int y):val(y){}
	int val;
}

class Date{
	Date(const Month month, const Day day, const Year year};
}
	
advanced:

struct Month{
public:
	static Month Jan(){return Month(1);}
	static Month Feb(){return Month(2);}
	...
private:
	explicit Month(int m);
	...
};
Date d(Month::Jan(),Day(30),Year(1995));
```

### shared_ptr let the designer avoid the guest making stack overflow problem and cross DLL problem
```
std::shared_ptr<Investment>createInvestment(){
	return std::shared_ptr<Investment>(new Stock);
}
```
The returned shared_ptr can be tranfered to arbitary DLLs. So in doesn't need to care about when delete is used.
Although the overhead of shared_ptr is twice that of a normal pointer, the effect it brings is worthwhile.