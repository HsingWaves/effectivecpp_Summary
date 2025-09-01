A small modification of code in tip 24
```
template<typename T>
class Rational{
public:
	Rational(const T& numerator = 0,
			const T& denominator =1);
	const T numerator() const;
	const T denominator() const;
	...
};
template<typename T>
const Rational<T>operator* (const Rational<T> &lhs,
							const Rational<T> &rhs)
{...}
// now the code is changed to template, but it is invalid
```
Solution:
```
template<typename T>
class Rational{
public:
...
friend const Rational operator*(const Rational& lhs,
								const Rational& rhs)
{
	return Rational(lhs.numerator()	*rhs.numerator(),
					lhs.denominator() *rhs.denominator());
}
};

```
Usage of friend is irrelevant to its tradional usage: to get the non-public 
elements.
Make friend function call assist function is a interesting topic:
```
template<typename T> class Rational;
template<typename T>
const Rational<T> doMultiply(const Rational<T> &lhs,
							const Rational<T> &rhs);
template<typename T>
class Rational{
public:
	...
friend
	const Rational<T>operator*(const Rational<T> &lhs,
							const Rational<T> &rhs)
	{return doMultiply(lhs,rhs);}
	...
};
```