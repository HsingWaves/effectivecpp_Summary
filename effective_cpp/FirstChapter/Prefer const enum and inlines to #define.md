## This sentence means that prefer compiler than preprocessor
`#define ASP_RATION 1.65` 
This syntax may be ignored when preprocessor may delete it before compiler begins to work.
Using `const double ASP_RATION = 1.65` Instead

# Attention 1:
`const char* const authorName = "sb H"` :The pointer and the constante value. Two things need to be declared.
`const string authorName(sb H)` is more suitable
# Attention 2:Exclusive constant within the scope of a class
```class GamePlayer{
  private:
      static const int NumTurns = 5;
      int scores[NumTurns];
      ....
  public:
      const int GamePlayer::NumTurns; //this statement muss here not in .h file
```
# Attention 3:
``` #define CALL_WITH_MAX(a,b) f((a)>(b)? (a):(b)
using inline than macros

template<typename T>
  inline void callWithMax(const T& a,const T& b){
       f((a)>(b)? (a):(b);
  }
```
