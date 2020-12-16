```
class Widget{
public:
  ...
  ~Widget(){...}
}
void doSomething(){
  std::vector<Widget>v;
  ...
}
// v is destroyed here automatically
```
Assumption: There are 10 elements in the v. If an exception throws in first deconstruction, the left 9 elements should 
be also deleted. If **deconstruction function throws exception**, the program will terminate early or perform abnormally

### Attention1: avoid exceptions in deconstruction functions

And special cases:

```
class DBConnection{
public:
  ...
  static DBConnection create();
  
  void close();//close connection;if it fails throw exception 
 };
 
```
**Solution:** Through a class to manage DBConnection, and offer a function to client that can manage the exception

```
class DBConn{
public:
  ...
  void close(){
  db.close();
  closed = true;
  }
  ~DBConn()
  {
  if(!closed){
    try{
    db.close();
    }
    catch(...){
      //make the running records, write down close using failure
      ...
      }
  }
private:
  DBConnection db;
  bool closed;
};

