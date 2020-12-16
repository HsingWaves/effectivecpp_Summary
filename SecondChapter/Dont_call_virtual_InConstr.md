### Attention1: Do not call virtual functions during construction and destruction
Reason: Because it will not fall to the derived class

```
class Transaction{
public:
  Transaction{};
  virtual void logTransaction() const = 0;
  ...
};

Transaction::Transaction()
{
  ...
  logTransaction(); //this virtual function in constructor
}

class BuyTransaction:public Transaction{
public:
    virtual void logTransaction()const;
    ...
 };
 
class SellTransaction: public Transaction{
    virtual void logTransaction() const;
    ...
};
```
now executing:
BuyTransaction b;// the logTransaction here executed is the version of Transaction not the version of BuyTransaction

**The reason for that is C++ disallow the part which is not already initialized**

The same reason for the deconstruction.
**Attention for intialization function contains the virtual function, which is not detected by compiler.**
Solution: define the function non-virtual
**void logTransaction(const std::string& logInfo)const;**
