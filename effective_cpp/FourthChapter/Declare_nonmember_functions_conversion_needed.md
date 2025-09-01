 ### implicit casting may be terriable idea, but it has exception
 The most common case is to build value types.
 It seems rational to do the conversion.
 ```
 class Rational{
 public:
	Rational(int numberator = 0,
			 int denominator = 1);//no explicit, allow int-to-Rational
	int numberator() const;
	int denominator() const;
private:
	...
};

//for operator*
class Rational{
public:
	...
	const Rational operator*(const Rational* rhs)const;
};

//following test case
Rational oneEight(1,8);
Rational oneHalf(1,2);
Rational result = oneHalf * oneEight; //great
result = result * oneEight;//great

//but by mixed computing
result = oneHalf*2; //pass
result = 2*oneHalf; //error!

result = oneHalf.operator*(2);//pass
result = 2.operator*(oneHalf);//error
```
An implicit type conversion happens here
```
result = oneHalf * 2; //is smiliar as following

const Rational temp(2);
result = oneHalf *temp;
```

Solution: let opeartor* become a non-member function
```
class Rational{
	...
};
// now this become a non-member function
const Rational operator*(const Rational& lhs,const Rational& rhs)
{
	return Rational(lhs.numerator()* rhs.numberator(),
					lhs.denominator()*rhs.denominator());
}
Rational oneFourth(1,4);
Rational result;
result = oneFourth*2;//pass
result = 2*oneFourth;//pass
```