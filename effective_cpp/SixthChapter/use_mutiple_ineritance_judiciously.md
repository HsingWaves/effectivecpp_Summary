There are tow groups: single inheritance is good, multiple is better.
Other advocates multiple inheritance is not worthwhile.
```
class BorrowableItem{
public:
	void checkOut();
	...
}
;
class ElectronicGadget{
private:
	bool checkOut()const;
	...
};

class MP3Player:
	public BorrowableItem,
	public ElectronicGadget{...};

MP3Player mp;
mp.checkOut(); // ambiguity, which ckeckOut is called.

//To avoid this ambiguity
mp.BorrowableItem::checkOut();
```
From the perspetive of correct behaviorL public inheritance should always virtual.
But virtual inheritance should cost more.
Suggestion is that:\
1. Don't use virtual base classes, unless it is necessary. 
2. If using virutal base classes, try to avoid laying data.

```
class IPerson{
public:
	virtual ~IPerson();
	virtual string name() cosnt =0;
	virtual string birthDate() const = 0;
};

shared_ptr<Iperson> makePerson(DatabaseID personIdentifier);
//This  function gets database ID from user
DatabaseID id(askUserForDatbaseId());

shared_ptr<IPerson>pp(makePerson(id));
//create an object support IPerson interface, through it dealing with *pp
...
```
But makePerson how to create an object and return a pointer to refer to it?
```
class PersonInfo{
public:
	explicit PersonInfo(DatabaseID pid);
	virtual ~PersonInfo();
	virtual const char* theName()const;
	virtual const char* theBirthDate()const;
	...
private:
	virtual const char* valueDelimOpen()const;
	virtual const char* valueDelimClose() const;
	...
};

class IPerson{
publc:
	virtual ~IPerson();
	virtual string name() const = 0;
	virtual string birthDate() const = 0;
};

class DatabaseID{...};

class PersonInfo{
public:
	explicit PersonInfo(DatabaseID pid);
	virtual ~PersonInfo();
	virtual const char* theName() const;
	virtual const char* theBirthDate() const;
	virtual const char* valueDelimOpen()const;
	virtual const char* valueDelimClose()const;
	...
};

class CPerson:public IPerson,private PersonInfo{
public:
	explicit CPerson(DatabaseID pid):PersonInfo(pid){}
	virtual string name()const{
	return PersonInfo::theName();
	}
	
	virtual string birthDate()const{
	return PersonInfo::theBirthDate();
	}
private:
	const char* valueDelimOpen()const {return "";}
	const char* valueDelimClose()const{return "";}
};
```
Multiinheritance has its proper use. One of it is that public inherits one 
interface class and private inherits assist implementation class.
