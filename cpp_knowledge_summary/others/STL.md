STL’s six components = **Containers + Algorithms + Iterators + Functors + Adapters + Allocators**.

Their relationships:

- **Algorithms** access **Containers** through **Iterators**.
- **Functors** customize the behavior of **Algorithms** or **Containers**.
- **Adapters** transform or restrict interfaces.
- **Allocators** handle memory management.



------

## 🌟 STL Six Major Components Explained in Detail

### 1. **Containers**

- Definition: Data structures that hold collections of objects.

- Types:

  - **Sequence containers** → `vector`, `list`, `deque` (elements in order).
  - **Associative containers** → `set`, `map`, `multiset`, `multimap` (ordered by key).
  - **Unordered containers** → `unordered_set`, `unordered_map`, etc. (hash-based).
  - **Container adapters** → `stack`, `queue`, `priority_queue`.

- Purpose: Provide **storage + access rules** for elements.

  ```c++
  // Sequence containers
  std::vector<int> v = {1, 2, 3, 4};     // dynamic array
  std::list<int> l = {1, 2, 3, 4};       // doubly linked list
  std::deque<int> d = {1, 2, 3, 4};      // double-ended queue
  
  // Associative containers
  std::set<int> s = {1, 2, 3};           // unique keys, ordered
  std::map<int, std::string> m = {{1,"A"}, {2,"B"}}; // key-value
  
  // Unordered containers
  std::unordered_set<int> us = {1, 2, 3}; // hash table
  std::unordered_map<int, int> um = {{1,10}, {2,20}};
  
  // Container adapters
  std::stack<int> st;   // LIFO
  std::queue<int> q;    // FIFO
  std::priority_queue<int> pq; // heap
  
  ```

  

------

### 2. **Algorithms**

- Definition: Generic procedures that operate on ranges of elements.

- Examples:

  - Searching: `find`, `binary_search`.
  - Sorting: `sort`, `stable_sort`.
  - Modifying: `copy`, `transform`.
  - Numeric: `accumulate`, `inner_product`.

- Key Point: **Algorithms are independent of containers**; they only require iterators.

  ```c++
  std::vector<int> v = {5, 2, 8, 1, 3};
  
  // Sorting
  std::sort(v.begin(), v.end());                 // ascending
  std::sort(v.begin(), v.end(), std::greater<>()); // descending
  
  // Searching
  auto it = std::find(v.begin(), v.end(), 8);    // linear search
  bool b = std::binary_search(v.begin(), v.end(), 3); // requires sorted
  
  // Modifying
  std::reverse(v.begin(), v.end());              // reverse order
  std::replace(v.begin(), v.end(), 2, 99);       // replace all 2 → 99
  
  // Numeric
  int sum = std::accumulate(v.begin(), v.end(), 0); // sum of all
  
  ```

- 

------

### 3. **Iterators**

- Definition: Generalized pointers that connect algorithms and containers.
- Types:
  - Input iterator (read-only, one-pass).
  - Output iterator (write-only).
  - Forward iterator (multi-pass, read/write).
  - Bidirectional iterator (move both directions).
  - Random access iterator (full pointer-like).
- Purpose: **Decouple containers from algorithms** so the same algorithm works on different containers.

------

### 4. **Functors (Function Objects)**

- Definition: Objects that behave like functions (via `operator()`).

- Examples:

  - `std::less<T>`, `std::greater<T>` for comparisons.
  - Arithmetic functors: `std::plus<T>`, `std::minus<T>`.

- Purpose: **Customize algorithm behavior** (e.g., sort ascending vs. descending).

- Modern C++: Often replaced by **lambdas** (`[](int a, int b){ return a > b; }`).

  ```c++
  std::vector<int> v = {1, 2, 3, 4};
  
  // Basic iteration
  for (auto it = v.begin(); it != v.end(); ++it) {
      std::cout << *it << " ";
  }
  
  // Reverse iteration
  for (auto it = v.rbegin(); it != v.rend(); ++it) {
      std::cout << *it << " ";
  }
  
  // Insert iterator
  std::list<int> lst;
  std::copy(v.begin(), v.end(), std::back_inserter(lst));
  
  // Stream iterators
  std::istream_iterator<int> in(std::cin), eof;
  std::vector<int> v2(in, eof);   // read numbers from stdin
  
  ```

  

------

### 5. **Adapters**

- Definition: Wrappers that **modify or restrict interfaces** of containers, iterators, or functors.

- Types:

  - Container adapters: `stack`, `queue`, `priority_queue`.
  - Iterator adapters: `reverse_iterator`, `insert_iterator`, `istream_iterator`.
  - Function adapters: `std::bind`, `std::not_fn`.

- Purpose: Provide **alternative views** or **different usage styles** of existing components.

  (A) Container adapters

  ```c++
  std::stack<int, std::vector<int>> st; // use vector as backend
  st.push(10); st.push(20); st.top();   // 20
  
  ```

  (B) Iterator adapters

  ```c++
  std::vector<int> v = {1, 2, 3};
  std::vector<int> result;
  std::copy(v.begin(), v.end(), std::back_inserter(result)); // push_back
  std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, " ")); // print
  
  ```

  (C) Functor adapters

  ```c++
  #include <functional>
  #include <algorithm>
  #include <vector>
  using namespace std;
  
  vector<int> v = {1, 5, 10, 20};
  auto f = bind(std::greater<int>(), 10, placeholders::_1); 
  // f(x) = 10 > x
  int cnt = count_if(v.begin(), v.end(), f); // how many < 10
  
  ```

  

------

### 6. **Allocators**

- Definition: Components that abstract memory allocation/deallocation.

- Default: `std::allocator<T>` uses `::operator new` and `::operator delete`.

- Custom: You can design your own allocator (e.g., memory pools, shared memory).

- Purpose: **Control how memory is managed** behind containers.

  ```c++
  #include <memory>
  #include <vector>
  using namespace std;
  
  int main() {
      // Default allocator
      vector<int, std::allocator<int>> v1;
  
      // Custom allocator (demo)
      struct MyAlloc : std::allocator<int> {
          int* allocate(size_t n) {
              cout << "Allocating " << n << " ints\n";
              return std::allocator<int>::allocate(n);
          }
      };
      vector<int, MyAlloc> v2;   // will print allocation info
      v2.push_back(42);
  }
  
  ```

  

------

## 🔗 How They Work Together

- **Algorithms** operate on **Containers** by using **Iterators**.

- **Functors** (or lambdas) adjust **how algorithms behave**.

- **Adapters** reshape or limit interfaces (stack, back_inserter, bind, etc.).

- **Allocators** make sure containers get their memory.

  **Container** = where data lives.

  **Iterator** = how to move through data.

  **Algorithm** = what to do with data.

  **Functor** = customize behavior.

  **Adapter** = reshape the interface.

  **Allocator** = handle memory.

------

✅ Example in action:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> v = {5, 2, 8, 1, 3};          // Container
    sort(v.begin(), v.end(), greater<int>()); // Algorithm + Iterator + Functor

    for (int x : v) cout << x << " ";         // Output: 8 5 3 2 1
}
```

- `vector` → **container**
- `sort` → **algorithm**
- `v.begin()` / `v.end()` → **iterators**
- `greater<int>()` → **functor**
- (Allocator used internally by `vector`)
- (If we used `stack` wrapping `vector`, that would be an **adapter**)

------

