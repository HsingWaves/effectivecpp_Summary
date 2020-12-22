### It is necessary to finely divide the scope of values
```
class AccessLevels{
public:
	...
	int getReadOnly()const{return readOnly;}
	void setReadWrite(int value){readWrite = value;}
	int getReadWrite()const{return readWrite;}
	void setWriteOnly(int value){writeOnly = value;}
private:
	int noAccess;	//not accessiable
	int readOnly;	//read only
	int readWrite;	//read and write
	int writeOnly;	//write only
};

```

When member values hide in behind the function interfaces, resilient for "make everything possible".
From the perspective of wrap:
there are only two authorities: **private and public**