 Consider the following code:
 ```
 class GameCharacter{
 public:
	virtual int healthValue()const;
	...
};
```
healthValue is not declared as pure virtual, it implies that it should 
have a default implementation.
Sometimes it becomes its weakness.
```
//Non virtual interface
class GameCharacter{
public:
	int healthValue() const{
	{	
		...
		int retVal = doHealthValue();
		...
		return retVal;
	}
	...
private:
	virtual int doHealthValue()const{
		...
	}
};
//This stream advocate that all virtual functions should be private.
//It is so called Template Method, this non-virtual funtion is called wrapper.
```
Sometimes some derived class in its virtual function implementation muss call
its base class function. virtual should be protected or public in this kontext.
This NVI is now suitable.
A replacement is called **Strategy model**.
```
class GameCharacter;

int defaultHealthCalc(const GameCharacter& gc);
class GameCharacter{
public:
	typedef int (*HealthCalcFunc)(const GameCharacter&);
	explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc)
	:healthFunc(hcf)
	{}
	int healthValue()const
	{	return healthFunc(*this);}
	...
private:
	HealthCalcFunc healthFunc;
};

//This model is more resilient
claas EvilBadGuy:public GameCharacter{
public:
	explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc)
	:GameCharacter(hcf)
	{...}
	...
};
int loseHealthQuickly(const GameCharacter&);
int loseHealthSlowly(const GameCharacter&);

EvilBadGuy ebg1(loseHealthQuickly);
EvilBadGuy ebg2(loseHealthSlowly);
```
Other alternatives:
1.trl::function replaces virtual function. 
2.classical strategy: replaces an inherited virtual function
by a virtual function in other inherited system. 

