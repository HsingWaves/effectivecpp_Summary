**Imperative languages**

Emphasize "process": telling the computer what to do, step by step.

You can start by laying out the main flow, then refine it, slowly adding details later (just like writing a script by outlining it first and then filling in the lines).

They are very forgiving: you can write half of the logic, run it through a portion, see the results, and then gradually refine it.

**Functional languages**

Emphasize "relationships": what are the inputs, what are the outputs, and how functions can be combined.

A function's signature is a contract. You must first think through:

What data does this function accept?

What data does it return?

How does it compose with other functions?

If you don't think through these things, you can't define your first function because the types, parameters, and return values are all undecided.

Why is this so?

Imperative programming allows for "state" as an intermediate buffer. You can use variables to store things and then deal with them later once you have figured it out.

Functional programming strives to eliminate side effects and requires you to use function composition to close logic loops. Therefore, it forces you to clearly understand "data flow" and "transformation relationships."

From another perspective:

Imperative programming is like writing a diary, writing whatever comes to mind.

Functional programming is like writing a mathematical formula. If you don't first define the symbols and relationships, the formula won't work.

```c++
#include <vector>
#include <iostream>
using namespace std;

int main() {
    vector<int> nums = {3, 12, 7, 15, 9};

    int sum = 0;
    for (int x : nums) {
        if (x > 10) {     // 临时想到加个条件
            int sq = x * x; // 这里又想到先平方
            sum += sq;      // 再加到结果里
        }
    }

    cout << sum << endl;
}

```

```c++
#include <vector>
#include <iostream>
#include <numeric>   // accumulate
#include <algorithm> // transform, copy_if

using namespace std;

int main() {
    vector<int> nums = {3, 12, 7, 15, 9};

    // 数据流：nums → 过滤大于10 → 平方 → 求和
    int sum = accumulate(nums.begin(), nums.end(), 0,
        [](int acc, int x) {
            return x > 10 ? acc + x * x : acc;
        }
    );

    cout << sum << endl;
}

```

C++20 Ranges:

```C++
#include <vector>
#include <iostream>
#include <ranges>
#include <numeric>

int main() {
    std::vector<int> nums = {3, 12, 7, 15, 9};

    // ：nums → filter >10 → quadrat → sum
    auto view = nums 
              | std::views::filter([](int x) { return x > 10; })
              | std::views::transform([](int x) { return x * x; });

    int sum = std::accumulate(view.begin(), view.end(), 0);

    std::cout << sum << std::endl;
}

```

**Imperativ**

```c++
#include <vector>
#include <iostream>
using namespace std;

int main() {
    vector<int> nums = {3, 12, 7, 15, 9, 20};

    int evenSum = 0, oddSum = 0;

    for (int x : nums) {
        if (x > 10) {
            int sq = x * x;
            if (sq % 2 == 0)
                evenSum += sq;
            else
                oddSum += sq;
        }
    }

    cout << "Even sum = " << evenSum << endl;
    cout << "Odd sum  = " << oddSum << endl;
}

```

**Functional**

```c++
#include <vector>
#include <iostream>
#include <ranges>
#include <numeric>

int main() {
    std::vector<int> nums = {3, 12, 7, 15, 9, 20};

    // 数据流：过滤 >10 → 平方
    auto view = nums
              | std::views::filter([](int x) { return x > 10; })
              | std::views::transform([](int x) { return x * x; });

    // 收敛阶段：按奇偶分组求和
    int evenSum = std::accumulate(view.begin(), view.end(), 0,
        [](int acc, int x) { return x % 2 == 0 ? acc + x : acc; });
    int oddSum  = std::accumulate(view.begin(), view.end(), 0,
        [](int acc, int x) { return x % 2 == 1 ? acc + x : acc; });

    std::cout << "Even sum = " << evenSum << '\n';
    std::cout << "Odd sum  = " << oddSum  << '\n';
}

```

```c++
#include <vector>
#include <iostream>
#include <ranges>
#include <algorithm>

int main() {
    std::vector<int> nums = {3, 12, 7, 15, 9, 20};

    auto view = nums
              | std::views::filter([](int x){ return x % 2 == 0; })
              | std::views::transform([](int x){ return x * x * x; });

    std::vector<int> result(view.begin(), view.end());
    std::ranges::sort(result, std::greater<>{});

    for (int x : result)
        std::cout << x << " ";
}

```

Count length of words

```C++

#include <string>
#include <vector>
#include <iostream>
#include <ranges>
#include <numeric>

int main() {
    std::vector<std::string> words = {"apple", "banana", "pear", "peach"};

    int totalLen = std::accumulate(words.begin(), words.end(), 0,
        [](int acc, const std::string& s) {
            return acc + (int)s.size();
        });

    std::cout << "Total length = " << totalLen << std::endl;
}

```

Pipeline

```C++
#include <iostream>
#include <ranges>

int main() {
    // 生成 0..∞ → 过滤偶数 → 平方 → 取前 5 个
    auto view = std::views::iota(0)
              | std::views::filter([](int x){ return x % 2 == 0; })
              | std::views::transform([](int x){ return x * x; })
              | std::views::take(5);

    for (int x : view)
        std::cout << x << " ";
}

```

Template:

1) **Filter → Map → Reduce**

   ```
   auto view = xs 
             | std::views::filter(Pred)              // 过滤
             | std::views::transform(Map);           // 映射
   
   auto ans = std::accumulate(view.begin(), view.end(), Init, Reduce); // 归约
   auto v = std::views::iota(Start)                // 生成无限序列
          | std::views::transform(Map)
          | std::views::filter(Pred)
          | std::views::take(N);                   // 取前 N 个
   ```

   

2)  Generate → Transform → Take

   ```c++
   auto flat = container_of_containers | std::views::join;
   
   auto flat = matrix | std::views::join;
   int s = std::accumulate(flat.begin(), flat.end(), 0);
   
   ```

3. FlatMap / Join

   ```
   auto flat = container_of_containers | std::views::join;
   
   auto flat = matrix | std::views::join;
   int s = std::accumulate(flat.begin(), flat.end(), 0);分组聚合
   ```

4.  Group and aggerate

   ```
   #include <unordered_map>
   
   template<class K, class V>
   auto group_sum(const std::vector<V>& xs, auto KeyOf, auto ValOf) {
       std::unordered_map<K, V> g;
       for (auto&& x : xs) g[KeyOf(x)] += ValOf(x);
       return g;
   }
   
   auto g = group_sum<int,int>(xs,
       [](int x){ return (x*x)%2; },   // key: 0=偶,1=奇
       [](int x){ return x*x; }        // val: 平方
   );
   int even = g[0], odd = g[1];
   ```

5. **Option and Maybe (std::optional)**

   ```
   std::optional<T> find_if_opt(const auto& xs, auto Pred) {
       auto it = std::ranges::find_if(xs, Pred);
       if (it == xs.end()) return std::nullopt;
       return *it;
   }
   
   double avg = std::accumulate(xs.begin(), xs.end(), 0.0)/xs.size();
   auto o = find_if_opt(xs, [avg](int x){ return x > avg; });
   if (o) std::cout << *o;
   
   ```

6.  **Either/Result (std::variant)**

   ```
   using Result = std::variant<int, std::string>; // Ok=int, Err=string
   Result parse_nonneg(std::string s){
       int v = std::stoi(s);
       return v>=0 ? Result{v} : Result{std::string{"neg"}};
   }
   
   ```

7. Point-Free

   ```
   auto square   = [](int x){ return x*x; };
   auto is_large = [](int x){ return x>10; };
   
   auto pipe = xs
     | std::views::filter(is_large)
     | std::views::transform(square);
   
   ```

   