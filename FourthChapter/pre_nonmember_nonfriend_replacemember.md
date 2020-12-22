 Consider following code
 ```
 class WebBrowser{
 public:
	...
	void clearCache();
	void clearHistory();
	void removeCookies();
};

// some users want to execute all these functions

class WebBrowser{{
public:
	...
	void clearEverything();
	...
};

//of course, it can written in this way:
void clearBrowser(WebBrowser& wb){
	wb.clearCache();
	wb.clearHistory();
	wb.removeCookies();
}
```
which way is better?
According to OOP the first way is better. But it wrong. In contrary,
member function clearEverything wrap worse than non-member function
Execpt that, non member function offers packaging flexibility.
More formally 
```
namespace WebBrowserStuff{
class WebBrowser{....};
void clearBrowser(WebBrowser& wb);
```
A class like WebBrowser may have a lot of convinient functions.
Some relevant to print, some relevant to cookie. Guests may interest in 
some and not in all. Sepeate them, the best way to use different header files
```
//int webbrowser.h file
namespace WebBrowserStuff{
class WebBrowser{...};// kernal functions almost everyone need it
	...
}
```
```
//in webbrowserbookmarks.h file
namespace WebBrowserStuff{
	....//bookmarks relevant
}
```
```
//in webbrowserbookmarks.h
namespace WebBrowserStuff{
	...//cookies relevant
}
...
```
**Not all classes are designed as base classes.**