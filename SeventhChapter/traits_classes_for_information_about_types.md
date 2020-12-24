### STL has total 5 iterator categories.
Input iterator can only move forward. User can only read one time.
istream_iterators is the represent of that.
Output iterator is similar, but only for output:ostream_iterators.
forward iterator is more powerful than above two. Read and write can
execute one more time for multi-pass algorithms.
Bidirectional iterator can move backford and forward. list belongs to it.
Iterators of set, multiset, map and multimap are also in this category.
The most powerful it random access iterator. It can jump to any position.
vector, deque, string offer this iterator.
```
struct input_iterator_tag {};
struct output_iterator_tag {};
struct forward_iterator_tag:public input_iterator_tag {};
struct bidirectional_iterator_tag: public forward_iterator_tag {};
struct random_access_iterator_tag: public bidirectional_iterator_tag {};
```
advance class is hoped to implement random access iterator
```
template<typename IterT,typename DistT>
void advance(IterT iter,DistT d)
{
	if(iter is random access iterator){
		iter +=d;
	}
	else{
		if(d >= 0){while(d--)++iter;}
		else{while(d++) --iter;}
	}
}
```
**traits offers the functionality to get the type during runtime**
Its implementation:
```
template<typename IterT>
struct iterator_traits {
	typedef typename IterT::iterator_catagory iterator_catagory;
	...
};
```
Solution for type judge: overloading
```
template<typename IterT,typename DistT>
void doAdvance(IterT& iter,DistT d,
			std::random_access_iterator_tag)
{
	iter +=d;
}

template<typename IterT, typename DistT>
void doAdvance(IterT& iter,DistT d,
			std::random_access_iterator_tag)
{
	if(d>=0){while (d--) ++iter;}
	else{ while(d++) --iter;
}

template<typename IterT,typename DistT>
void doAdvance(IterT& iter,DistT d,
			std::random_access_iterator_tag)
{	
	if(d<0){throw std::out_of_range("Negative distance");}
	while(d--) ++iter;
}

//implementation
template<typename IterT, typename DistT>
void advance(IterT& iter, DistT d)
{
	doAdvance(
	iter,d,typename
	std::iterator_traits<IterT>::iterator_catagory()
	};
}
```
Summary:
1.Build a group of overloading functions
2.Build a control function or function template(advance)

