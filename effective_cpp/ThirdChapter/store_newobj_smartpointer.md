 ### There are functions showing priority
 
 ```
 int priority();
 void processWidget(std::shared_ptr<Widget>pw,int priority);
 
 ......
 //now considering calling it
 processWidget(new Widget(),priority());//compiling error
 
 //the correct format
 processWidget(std::shared_ptr<Widget>(new Widget(),priority());//pass
 
 ```
 But the above codes may lead to stack overflow
 1. execute new Widget
 2. call shared_ptr construction function
 3. call priority
 4. execute new Widget
 5. call shared_ptr function
 
 The order of C++ compiler is resilient. Above processes may be simplified to:
 1. execute new Widget
 2. call priority
 3. call shared_ptr function
 Solution: **seperate them**
 ```
 std::shared_ptr<Widget>pw(new Widget);
 processWidget(pw,priority());
 ```