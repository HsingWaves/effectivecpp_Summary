### OOp world always uses explicit interfaces and runtime polymorphism to solve problem
```
class Widget{
public:
	Widget();
	virtual ~Widget();
	virtual std::size_t size() const;
	virtual void normalize();
	void swap(Widget& other);
	...
};

void doProcessing(Widget& w)
{
	if(w.size() > 10&&w!= someNastyWidget){
		Widget temp(w);
		temp.normalize();
		temp.swap(w);
	}
}
```
The variable is declared as type Widget, so w must support Widget. This interface
can be found in the head file. So it is called explicit interface.
As some member functions of Widget are virtual, w performs **runtime polymorphism**
for these funtions. It means that at the runtime which funtion is called according to
dynamic type of w (see tip 37)
In template world, **implicit interfaces and compile-time polymorphism** come front.
```
template<typename T>
void doProcessing(T& w)
{
	if(w.size() > 10&&w!= someNastyWidget){
		T temp(w);
		temp.normalize();
		temp.swap(w);
	}
}
```
Compile-time polymorphism: All the calls involve w, such as operator> and operator!=
may lead to template instantiated. Different template parameter instantiated may call
different funtions. It is so called compile-time polymorphism.
```
template<typename T>
void doProcessing(T& w)
{
if(w.size() > 10&&w!= someNastyWidget)
	...
```
Does it mean that w should have size() funtion? Or must support operator!=?
Through **operator overloading**, these two restrictions don't need to be satisfied.
