Some basic parts in C++98:
1.STL, including containers(vector,string,map),iterators, algorithm(find,sort,
transform),function objects(less,greater),containers adapter(stack,priority_queue),
function object adapters(mem_fun,not1).\
2.Iostreams:buffer functions,international I/O,cin,cout,cerr,clog.\
3.International support,multiple active locals. Just like wchar_r and wstring.\
4.Digit deals,complex template and valarray.\
5.Exception hierarchy:base class exception, derived classes logic_error and 
runtime_error.\

TR1 new components(now they are already in standard library):\
1.smart pointer: std::shared_ptr and std::weak_ptr, terms 13 clarifies it detailed.\
2.std::bind: it uses for bindlst and bind2nd. It can cooperate with const and 
non-const member functions.\
3.Hashtable:set multiset,map,multimap.\
4.Regular expression.\
5.tuples: pair template product. pair can hold two objects and tuple can hold 
any objects.\
6. array, mem_fn, reference_wrapper, random number.\
7.Type traits, result_of.\