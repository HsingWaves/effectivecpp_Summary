### public inheritance means "is a" relationship
```
class Bird{
	...
};

class FlyingBird:public Bird{
public:
	virtual void fly();
	...
}£»


class Penguin:public Bird{
	...//Penguin cannot fly
};
```
A question is that should class Square public extend class Rectangle?
```
class Rectangle{
public:
	virtual void setHeight(int newHeight);
	virtual void setWidth(int newWidth);
	virtual int height() const;
	virtual int width() const;
	...
};

void makeBigger(Rectangle& r)
{
	int oldHeight = r.height();
	r.setHeight(r.width()+10);
	assert(r.height() == oldHeight);
}

//obiviously the above assert is always true,makeBigger only change r width
// r height dont' change
class Square:public Rectangle {...};
Square s;
...
assert(s.width() == s.height());//pk
makeBigger(s);
assert(s.width() == s.height());//bound to be ok but not

//It is compitible but logically not correct.
```
