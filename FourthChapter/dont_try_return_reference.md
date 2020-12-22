 ### sometimes designer may make the mistakes: everthing pass-by-reference even the objects dont' exist
 ```
 class Rational{
	Rational(int numerator =0,
			 int denominator = 1};
	...
private:
	int n,d;
	friend
	const Rational operator *(const Rational& lhs,
							  const Rational& rhs);
};
//canonot expect the object is existing before it is created:
Rational a(1,2);
Rational b(2,3);
Rational c = a*b;
```
bad code sytle :
```
const Rational& operator*(const Rational& lhs,
						  const Rational& rhs)
{
	Rational result(lhs.n* rhs.n,lhs.d*rhs.d);
	return result;
}
//in this way call the construction function and a reference to result,which is local and outdated!!!
```

Head-based object is created by **new**:
```
const Rational& operator*(const Rational& lhs,
						  const Rational& rhs)
{
	Rational result =new Rational(lhs.n* rhs.n,lhs.d*rhs.d);
	return *result;
}
//The cost is also a call of construction function call
```
A new question is that who executes **delete** ?
```
Rational w,x,y,z;
w = x*y*z;
```
Which may lend to memory overflow. For it is difficult to get the hidden reference pointer.

### Attention: Don't try to use static reference, which may lend to more troubles.

The solution is that when new object is needed and return object:
```
inline const Rational operator*(const Rational&lhs, const Rational& rhs){
	return Rational(lhs.n*rhs.n,lhs.d*rhs.d);
}
```