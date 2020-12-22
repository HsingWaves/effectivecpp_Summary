 ### pass-by-value is c style code
 ```
 class Person{
 public:
	Person();
	virtual ~Person();
	...
private:
	string name;
	string address;
};
class Student:public Person{
public:
	Student();
	~Student();
	...
private:
	string schoolName;
	string schoolAddress;
};
```
The following code is executed:
```
bool validateStudent(Student s);
Student plato;
bool platoisOk = validateStudent(plato);
```
Student copy construction is called. Each time Student object is created, a Person object is also created.
Totally one Student copying construction, one Person copying construction and four string copy construction.
and six deconstruction.
Solution:
```
bool validateStudent(const Student& s);
//pass by reference
```
**There are no construction and deconstruction call in this way.** So the const is necessary.
**Because pass-by-value object is seen as base object.**
### pass by reference can also avoid slicing problem
Example:
```
class Window{
public:
	...
	string name()const;
	virtual void display()const;
	
};

class WindowWithScrollBars:public Window{
public:
	...
	virtual void display()const;
};

Now a print function is needed and display the window at the same time
false way:
void printNameAndDisplay(Window w){ // the parameter may be slicing wrong!
	cout<<w.name();
	w.display();
}

WindowWithScrollBars wwsb;
printNameAndDisplay(wwsb0);// this time it executes only Window::display() rather than WindowWithScrollBars::display()
```
### But the above rules are not suitable for Built-in type

pass by reference is actually through pass by pointer, whose overhead is bigger than that of pass-by-value.

	