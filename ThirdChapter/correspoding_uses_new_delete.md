### Array is a group of objects

```
string* stringPtr1 = new string;
string* stringPtr2 = new string[100];
...
delete stringPtr1;	//delete an object
delete [] stringPtr2;//delete an array, compiler should be told
```
If using delete for array object, some of objects may be not deleted.
Considering the following codes:
```
typedef std::string AddressLines[4];

std::string* pal = new AddressLines;//new AddressLines is just like new string[4]
	...
delete[] pal;
```