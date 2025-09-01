The following code uses private inheritance:
```
class Person{...};
class Student:private Person{...};
void eat(const Person &p);
void study(const Student& s);

Person p;
Student s;

eat(p);
eat(s);		//error
```
It is obivious that inheritance does not mean "is-a" relationship.
Private inheritance means **implemented-in-terms-of**. It is similar 
with java interface. Compared to composition in last terms, which is
better? **Try to use composition as possible.** Only when protected members
,virtual functions or space relation involved.
```
class Timer{
public:
	explicit Time(int tickFrequency);
	virtual void onTick() const;
	...
}
// Widget is not timer but want to inherit it.
class Widget:private Timer{
private:
	virtual void onTick()const;
	...
};
//But private inheritance is not necessary
class Widget{
private:
	class WidgetTimer:public Timer{
	public:
		virtual void onTick() const;
		...
	};
	WidgetTimer timer;
	...
};
```
An aggressive situation is that class dealing with doesn't contains any data.
```
class Empt{};

class HoldAnInt{
private:
	int x;
	Empt e;
};
//sizeof(HoldAnInt)>size(int)

class HoldAnInt:private Empt{
private:
	int x;
};
```
This is so called **empty base optimization(EBO).** empty classes are not 
real classes. They neever possess non-staic members, but contain typedefs,
enums, static or non-virtual functions.

