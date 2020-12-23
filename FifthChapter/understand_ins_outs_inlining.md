### The idea behind inline is for every calling of this function replace function itself
inline may cause code inflation and paging and low down instruction cache hit rate
```
class Person{
public:
	...
	int age()const {return theAge;} //an implicit inline apply
	...
private:
	int theAge;
};
```

### Attention: restrict the inlining in small and frequent used functions
### Attention: don't always declare function inline if function templates in the head file, d