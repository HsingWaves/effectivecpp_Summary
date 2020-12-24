### Template metaprogramming(TMP) performs template-based C++ during runtime
1. It makes some problems easier.
2. It moves some processes to build time, errors are more easier to be detected.
But the compile time is longer.

TMP starts with factorial during compiling.
It performs recursive template instantiation to implement loop
```
template<unsigned n>
struct Factorial{
	enum{value = n*Factorial<n-1>::value};
};
template<>
struct Factorial<0>{
	enum{value = 1};
};
```
TMP is not mainstream for normal development, but it is necessary for library 
developer.