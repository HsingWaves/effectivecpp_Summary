### Attention 1: Copy function should ensure that all members variables and all base class components
```
void logCall(const std::string& funcName);//create a log entry
class Customer{
public:
    ...
    Customer(const Customer& rhs);
    Customer& operator=(const Customer& rhs);
    ...
private:
    std::string name;
};
```

```
class PriorityCustomer:public Customer{
public:
   ...
   PriorityCustomer(const PriorityCustomer &rhs);
   PriorityCustomer& operator=(const PriorityCustomer& rhs);
   ...
private:
   int priority;
};

PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs):priority(rhs.priority){
    logCall("PriorityCustomer copy constructor);
}

PriorityCustomer& PriorityCustomer::operator=(const PriorityCustomer& rhs)
{
  logCall("PriorityCustomer copy assignment operator");
  priority = rhs.priority;
  return *this;
  
  //here copy construtor doesn't send the parameter to base class;
}

```
**compiler error**
Solution:

```
PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs):Customer(rhs),priority(rhs.priority){
    logCall("PriorityCustomer copy constructor);
}

PriorityCustomer& PriorityCustomer::operator=(const PriorityCustomer& rhs)
{
  logCall("PriorityCustomer copy assignment operator");
  priority = rhs.priority;
  Customer::operator=(rhs);//this 
  return *this;
}

```
### Attention 2: Dont' try to use one copy funtion to implement another copy function. Put the common mechanism 
### into the third function, which can be called by the both copy functions.

