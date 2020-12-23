 Assump a class is for GUI background
 ```
 class PrettyMenu{
public:
	...
	void changeBackground(std::istream& imgSrc);
	...
private:
	Mutex mutex;
	Image* bgImage;
	int imageChanges;
};
// a possible implementation
void PrettyMenu::changeBackground(std::istream& imgSrc){
	lock(&mutex);
	delete bgImage;
	++imageChanges;
	bgImage = new Image(imgSrc);
	unlock(&mutex);
}
```
For the perspective of exception secure, this implementation is terriable

**1. No resources leak:**If new Image(imgSrc)throw exception, the mutex is lock forever
**2. No data damge:** if new Image(imgSrc) throws exception means bgImage points to 
a deleted object.

Solution according to tip 13 and 14:
```
void PrettyMenu::changeBackground(std::istream& imgSrc){
	lock ml(&mutex); // get the lock ensure it will be released later 
	delete bgImage;
	++imageChanges;
	bgImage = new Image(imgsrc);
}
```
Exception-safe functions offer the following three Guarantee
1.Even exception is threw, other varaibles stay. No data or objects damage.
2.Exception is threw, program will be back to place before calling.
3.promise no-throw

The optimal solution for above code
```
class PrettyMenu{
	...
	std::shared_ptr<Image>bgImage;
	...
};
void PrettyMenu::changeBackground(std::istream& imgSrc)
{
	Lock ml(&mutex);
	bgImage.reset(new Image(ImageSrc));
	
	++imageChanges;
}
```