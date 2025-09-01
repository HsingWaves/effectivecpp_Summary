 ### C++ doesn't make the seperation of interface and implementation optimal
 ```
 class Person{
 public:
	Person(const std::string &name,const Date &birthday,
			const Address &addr);
	std::string name()const;
	std::string birthday()const;
	std::string adress() const;
	...
private:
	std::string theName;
	Date theBirthDate;
	Address theAddress;
};
```
The above code can not be compiled. Because compiler doesn't get the dependecies:
```
#include<string>
#include "date.h"
#include "address.h"
```
It lend to compilation dependency. The cascading compliation dependencies may lend 
to indescribable disaster.

### Attention: Head file should only have full and declaration-only forms
```
#include <string>
#include <memory>

class PersonImpl;
class Date;
class Address;

class Person{
public:
	Person(const std::string& name,const Date &birthday,
				const Address &addr);
	std::string name() const;
	std::string birthday() const;
	std::string address() const;
	...
private:
	std::shared_ptr<PersonImpl>pImpl;
};
// Person is divided into two classes, one for declaration and one for implementation
//following is the implementation
#include "Person.h"
#include "PersonImpl.h"

Person::Person(const std::string& name,const Date &birthday,
				const Address &addr):
		pImpl(new PersonImpl(name,birthday,addr)){}
std::string Person::name()const
{
	return pImpl->name();
}
```
Another way is to make Person class become a special abstract base class
```
class Person{
public:
	virtual ~Person();
	virtual std::string name()const = 0;
	virtual std::string birthday() const = 0;
	virtual std::string address() const = 0;
	...
};
```