Here program is to transfer information to different companies
```
class ComanyA{
public:
	...
	void sendClearText(const std::string &msg);
	void sendEncrypted(const std::string &msg);
	...
};
class CompanyB{
public:
	...
	void sendCleartext(const std::string &msg);
	void sendEncrypted(const std::string &msg);
	...
};
...
class MsgInfo{...};

template<typename Comany>
class MsgSender{
public:
	...
	void sendClear(const MsgInfo& info)
	{
		std::string msg;
		
		Comany c;
		c.sendClear(msg);
	}
	void sendSecrect(const MsgInfo& info)
	{...}
};

template<typename Comany>
class LoggingMsgSender:public MsgSender<Comany>{
public:
	...
	void sendClearMsg(const MsgInfo& info)
	{
		sendClear(info); //call base funtion, not compitible
	}
	...
};
```
A method must found that not entering templatized base classes to oberserve
```
template<typename Comany>
class LoggingMsgSender:public MsgSender<Comany>{
public:
	...
	void sendClearMsg(const MsgInfo& info)
	{
		this->sendClear(info); //valid,assump sendClead is exended
	}
	...
};
//----------------------------------
//tips 33
template<typename Comany>
class LoggingMsgSender:public MsgSender<Comany>{
public:
	using MsgSender<Comany>::sendClear;//tell compiler sendClear is in base class
	...
	void sendClearMsg(const MsgInfo& info)
	{
		sendClear(info); //valid,assump sendClear is exended
	}
	...
};
//------------------------------------
//is not recommended because it calls virtual function
template<typename Comany>
class LoggingMsgSender:public MsgSender<Comany>{
public:
	...
	void sendClearMsg(const MsgInfo& info)
	{
		MsgSender<Comany>::sendClear(info); //valid,assump sendClear is exended
	}
	...
};
```

 