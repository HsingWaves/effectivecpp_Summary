 ```
 class B{
 pulic:
	void mf()
	...
}

class D:public B{...}
D x;
B* pB = &x;
pb->mf();

D* pD = &x;
pD->mf();
```
The above mf() are different. Because non-virtual functions are **statically
bound**. On the contrary, virtual functions are **dynamic bound**.
According to terms 32, public inheritance is "is-a" relationship.
Remember never redefine inherited non-virtual functions!