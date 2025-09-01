### Statiscally bound is early binding, dynamic bound is later binding
```
class Shape{
public:
	enum ShapeColor{Red,Green,Blue};
	virtual void draw(ShapeColor Color = Red) const = 0;
	...
};
class Rectangle:public Shape{
public:
	virtual void draw(ShapeColor color = Green)const;
	...
}

class Circle:public Shape{
public:
	virtual void draw(ShapeColor color)const;
	...
};

Shape* ps;
Shape* pc = new Circle;
Shape* pr = new Rectangle;

//virtual function is dynamic binding, which virtual function is called,
depending on caller'dynamic type.

pc->draw(Shape::Red);	//call Circle
pr->draw(Shape::Red);	//call Rectangle
```
There code is already taught several times. Strange is that half parameters are
from static type Shape*. Why in C++ does it have such situation? If default parameters
are dynamic binding, compiler should have one method to decide parameters during 
runtime. This mechanism is more slower. C++ compiler make thi choice.