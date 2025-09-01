### public inheritance includes two parts:functions interfaces and implementations
TO distinguish the diffenerces of them, considering about the following code
```
class Shape{
public:
	virtual void draw() const = 0;
	virtual void error(const std::string& msg);
	int objectID() const;
	...
};

class Rectangle:public Shape{...};
class Ellipse:public Shape{...};
```
Shape is an abstract class, its pure virtual function draw makes it abstract.
client cannot initialize a shape class.
**Declaration of pure virtual function is to make derived classes only inheritance
interfaces.**
```
Shape* ps = new Shape; //error
Shape* ps1 = new Rectangle; //ok
ps1->draw();	//ok
Shape* p2 = new Ellipse;
ps2->draw();//ok Ellipse::draw
ps1->Shape::draw();//ok Shape::draw
ps2->Shape::draw();//ok Shape::draw
```
Impure virtual function will offer an implementation code,derived classes can
override it.
```
class Shape{
public:
	virtual void error(const std::string& msg);
	...
}
```
This means that if you don't implement an error function, you can use Shape class
offering version. But this may cause other trouble.
```
class Airport{...};
class Airplane{
public:
	virtual void fly(const& Airport& destination);
	...
}
void Airplane::fly(const Airplane& destination)
{
	...
}

class ModelA:public Airplane{...};
class ModelB:public Airplane{...};
```
The above code is typical OOP example, easy to maintain, share the
same entities.
```
class ModelC:public Airplane{
	...//haven't declared fly function
};

Airport PDX{...};
Airplane* pa = new ModelC;
...
pa->fly(PDX);	// call Airplane::fly
```
This program now tries to use ModelA or ModelB fly to execute ModelC.
ModelC is different fly function, ModelA and ModelB use default. How?
Solution:
```
class Airplane{
public:
	virtual void fly(const Airplane& destination) = 0;//pure virtual
	...
};

void Airplane::fly(const Airplane& destination)
{
	...
}

class ModelA:public Airplane{
public:
	virtual void fly(const Airport& destination)
	{
	Airplane::fly(destination);
	}
	...
};
class ModelB:public Airplane{
public:
	virtual void fly(const Airplane& destination){
	Airplane::fly(destination);
	}
	...
};

class ModelC:public Airplane{
public:
	virtual void fly(const Airplane& destination);
	...
};

void ModelC::fly(const Airport& destination)
{
		...// ModelC Implementation
}
```
*** Attention: If a member function is non-virtual function, it **does** 
mean that **it would not have** different behaviors in derived classes.
Invariance of member function is over its specilization.
