### Composition is type relationship
As an object has its type object:
```
class Address{...};
class PhoneNumber {...};
class Person{
public:
	...
private:
	string name;
	Address address;
	PhoneNumber voiceNumber;
	PhoneNumber faxNumber;
};
```
Composition has other meanings which represent the same thing:\
layering, containment, aggregation, embedding.\
It is complicated to disdinguish "is-a" and "is-implemented-in
terms-of" to present class with unduplicated objects.
Reuse is good thing, but man has to make a choice.
```
template<class T>
class Set{
public:
	bool member(const T& item) const;
	void insert(const T& item);
	void remove(const T& item);
	size_t size() const;
private:
	std::list<T>rep;
};
```
set is through balanced search implemented. Easy for lookup,insert and remove, no 
duplicated elements. But it costs three pointers extra space by its implementation.
