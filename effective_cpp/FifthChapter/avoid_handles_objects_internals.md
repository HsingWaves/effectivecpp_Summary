Look at following code
```
class Point{
public:
	Point(int x, int y);
	...
	void setX(int newVal);
	void setY(int newVal);
	...
};
struct RectData{
	Point ulhc;
	Point lrhc;
};
class Rectangle{
	...
	Point& upperLeft() const{return pData->ulhc;}
	Point& lowerRight() const{return pData->lrhc;}
private:
	std::shared_ptr<RectData>pData;
};

//And the user can modify the internal data
	Point coord1(0,0);
	Point coord2(100,100);
	const Rectangle rec(coord1, coord2);
	
	rec.upperLeft().setX(50); //it is expected, res should unchangable
```
Solution:
```
//just add const before Point&
	const Point& upperLeft(){....}
	....
```
Try to avoid dangling handles as possible

