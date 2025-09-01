**When the control flow reach current variable, 
it will generate cost by variable with construction and deconstruction functions**
But man may think they cannot define an unused variable
```
std::string encryptPassword(const std::string &password){
	using namespace std;
	string encrypted;
	if(password.length()<MinimumPasswordLength){
		throw logic_error("password is too short");
	}
	...
	return encrypted;
}
//When exception is throwing, encrypted is not used.
//just declare the encrypted after the judgement
```
But this way is not optimal, because it uses default construction.(According to 
**tip 4**, default+pass-value cost more resources than assign construction)
```
std::string encryptPassword(const std::string& password){
	...
	std::string encrypted(password);

	encrypt(encrypted);
	return encrypted;
}
```
### Attention: What if by loop?
```
//Example 1
Widget w;
for(int =0;i<n;++i){
	w = xxx(i);
	...
}

//Example 2
for(int i =0;i<n;++i){
	Widget w;//depend on i
	...
}
```
Example1: 1 construction + 1 deconstruction + n pass-value
Example2: n construction + n deconstruction
For classes the cost of pass-value lower than con+decon, A is more efficient.
Or if the code is in performance-sensitive part. Otherwise, B.
