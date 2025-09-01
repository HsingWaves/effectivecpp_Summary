```cpp
#include <any>
#include <cassert>
#include <functional>
#include <iostream>
#include <map>
#include <mutex>
#include <optional>
#include <stdexcept>
#include <type_traits>
#include <vector>

// ---------- rank 检查策略 ----------
struct nullcheck {};

struct rank_data {
  static std::vector<int>& stack() { static thread_local std::vector<int> s; return s; }
};

struct rank_check_nothrow {
  explicit rank_check_nothrow(int rank) {
    auto& s = rank_data::stack();
    if (!s.empty() && s.back() > rank) {
      std::cerr << "[RankCheck] violation: last=" << s.back() << " > current=" << rank << "\n";
      assert(!"Invalid locking rank order!");
    }
    s.push_back(rank);
  }
  ~rank_check_nothrow() { rank_data::stack().pop_back(); }
};

// ---------- locking policy ----------
struct try_apply_failed : std::runtime_error {
  try_apply_failed(): std::runtime_error("try_apply failed") {}
};

struct stl_locking_policy {
  using mutex_type = std::mutex;
  struct lock_type {
    explicit lock_type(mutex_type& m) : lk(m) {}
    lock_type(mutex_type& m, std::try_to_lock_t) : lk(m, std::try_to_lock) {
      if (!lk.owns_lock()) throw try_apply_failed();
    }
    std::unique_lock<mutex_type> lk;
  };
  struct scoped_unlock {
    explicit scoped_unlock(lock_type& l) : ref(l), unlocked(false) { ref.lk.unlock(); unlocked = true; }
    ~scoped_unlock() { if (unlocked) ref.lk.lock(); }
    lock_type& ref; bool unlocked;
  };
};

// ---------- concepts ----------
template<class T> concept NullCheck    = std::is_same_v<T, nullcheck>;
template<class T> concept RankChecked  = (!NullCheck<T>);

// ---------- shared_resource（concepts 版） ----------
template<class T, int Rank = 0,
         class LockingPolicy = stl_locking_policy,
         class RankCheckPolicy = nullcheck>
class shared_resource {
public:
  using Accessor      = std::function<void(T&)>;
  using ConstAccessor = std::function<void(const T&)>;
  using Mutex         = typename LockingPolicy::mutex_type;
  using Lock          = typename LockingPolicy::lock_type;

  // ---- 构造：无 rank 检查 ----
  shared_resource() requires NullCheck<RankCheckPolicy> {}
  explicit shared_resource(const T& v) requires NullCheck<RankCheckPolicy> : value_(v) {}
  explicit shared_resource(typename LockingPolicy::mutex_type m) requires NullCheck<RankCheckPolicy> : mtx_(std::move(m)) {}
  shared_resource(const T& v, typename LockingPolicy::mutex_type m) requires NullCheck<RankCheckPolicy> : value_(v), mtx_(std::move(m)) {}

  // ---- 构造：有 rank 检查（需要 group，占位 int 即可）----
  explicit shared_resource(int /*group*/) requires RankChecked<RankCheckPolicy> {}
  shared_resource(int /*group*/, const T& v) requires RankChecked<RankCheckPolicy> : value_(v) {}
  shared_resource(int /*group*/, typename LockingPolicy::mutex_type m) requires RankChecked<RankCheckPolicy> : mtx_(std::move(m)) {}
  shared_resource(int /*group*/, const T& v, typename LockingPolicy::mutex_type m) requires RankChecked<RankCheckPolicy> : value_(v), mtx_(std::move(m)) {}

  // ---- apply：无 rank 检查 ----
  void apply(Accessor fn) requires NullCheck<RankCheckPolicy>       { Lock l(mtx_); fn(value_); }
  void apply(ConstAccessor fn) const requires NullCheck<RankCheckPolicy> { Lock l(mtx_); fn(value_); }
  bool try_apply(Accessor fn) requires NullCheck<RankCheckPolicy> {
    try { Lock l(mtx_, std::try_to_lock); fn(value_); return true; }
    catch(...) { return false; }
  }
  bool try_apply(ConstAccessor fn) const requires NullCheck<RankCheckPolicy> {
    try { Lock l(mtx_, std::try_to_lock); fn(value_); return true; }
    catch(...) { return false; }
  }

  // ---- apply：有 rank 检查 ----
  void apply(Accessor fn) requires RankChecked<RankCheckPolicy> {
    RankCheckPolicy guard(Rank);
    Lock l(mtx_); fn(value_);
  }
  void apply(ConstAccessor fn) const requires RankChecked<RankCheckPolicy> {
    RankCheckPolicy guard(Rank);
    Lock l(mtx_); fn(value_);
  }
  bool try_apply(Accessor fn) requires RankChecked<RankCheckPolicy> {
    RankCheckPolicy guard(Rank);
    try { Lock l(mtx_, std::try_to_lock); fn(value_); return true; }
    catch(...) { return false; }
  }
  bool try_apply(ConstAccessor fn) const requires RankChecked<RankCheckPolicy> {
    RankCheckPolicy guard(Rank);
    try { Lock l(mtx_, std::try_to_lock); fn(value_); return true; }
    catch(...) { return false; }
  }

  // 读一份拷贝（演示）
  T copy() const { std::optional<T> out; apply([&](const T& v){ out = v; }); return *out; }

private:
  mutable Mutex mtx_{};
  T value_{};
};

// ---------- 一个极简 PropSet / Cache 用于演示 ----------
namespace prop { enum id { correction_collar=1, mcc_ri=2 }; }

struct CDevPropSet {
  struct property { int type; std::any value; };
  using Map = std::map<int, property>;
  enum { DOUBLE=1, PROPSET=2 };

  void Set(int id, double v){ m_[id] = {DOUBLE, v}; }
  double Get(int id, double dflt) const {
    auto it = m_.find(id);
    if (it==m_.end() || it->second.type!=DOUBLE) return dflt;
    return std::any_cast<double>(it->second.value);
  }
  CDevPropSet* GetSub(int id){
    auto it = m_.find(id);
    if (it==m_.end() || it->second.type!=PROPSET){
      m_[id] = {PROPSET, CDevPropSet{}};
      it = m_.find(id);
    }
    return std::any_cast<CDevPropSet>(&it->second.value);
  }
private:
  Map m_;
};

template<class T>
struct CSmartCache {
  void Update(const CDevPropSet& s){ snap_ = s; }
  const CDevPropSet& Snapshot() const { return snap_; }
private:
  CDevPropSet snap_;
};

// ---------- 两个资源，分配 rank ----------
using SRCache = shared_resource<CSmartCache<int>, 5, stl_locking_policy, rank_check_nothrow>;
using SRProps = shared_resource<CDevPropSet,       6, stl_locking_policy, rank_check_nothrow>;

int main() {
  // 有 rank 检查（需要传 group，占位 0 即可）
  SRCache cache(0);
  SRProps props(0);

  // 写 RI（rank 6）
  props.apply([&](CDevPropSet& p){
    auto* cc = p.GetSub(prop::correction_collar);
    cc->Set(prop::mcc_ri, 1.52);
  });

  // 嵌套锁顺序：5 -> 6（正确）
  cache.apply([&](CSmartCache<int>& c){
    props.apply([&](const CDevPropSet& p){
      c.Update(p);
    });
  });

  // 读 RI
  double ri = 0.0;
  props.apply([&](const CDevPropSet& p){
    auto* cc = const_cast<CDevPropSet&>(p).GetSub(prop::correction_collar);
    ri = cc->Get(prop::mcc_ri, 1.45);
  });
  std::cout << "RI=" << ri << "\n";

  // try_apply
  bool ok = props.try_apply([&](CDevPropSet& p){
    auto* cc = p.GetSub(prop::correction_collar);
    cc->Set(prop::mcc_ri, 1.50);
  });
  std::cout << "try_apply ok=" << std::boolalpha << ok << "\n";

  // 故意的错误锁序（6 -> 5）会触发断言（解开注释体验）
  // props.apply([&](CDevPropSet&){
  //   cache.apply([&](CSmartCache<int>&){});
  // });

  std::cout << "RI(final)="
            << props.copy().GetSub(prop::correction_collar)->Get(prop::mcc_ri, 0.0)
            << "\n";
}

```

C++20 引入 **concepts**，就是为了替代像 SFINAE 那种“写起来拗口、报错难读”的技巧。我们用你 `shared_resource` 里的例子，来对比一下好处。

------

## 1. 语义直白，读起来像自然语言

SFINAE 版本：

```cpp
template<typename U = rank_check_policy,
         typename std::enable_if_t<std::is_same_v<U, nullcheck>, void*> = nullptr>
shared_resource() {}
```

**concepts 版本**：

```cpp
shared_resource() requires std::is_same_v<RankCheckPolicy, nullcheck> {}
```

👉 语义清晰：**“只有当 RankCheckPolicy 是 nullcheck 时，这个构造函数才存在”**。
 读代码的人一眼就能懂。

------

## 2. 错误信息更友好

- **SFINAE**：报错会是类似

  ```
  no matching function for call to ...
  note: candidate template ignored: substitution failure
  ```

  新人很难看懂。

- **concepts**：报错会直接说

  ```
  constraints not satisfied
  the required constraint 'RankChecked<RankCheckPolicy>' was not satisfied
  ```

  报错点 + 失败的条件一目了然。

------

## 3. 接口层面更安全

SFINAE 是“看似有这个函数，但替换失败就消失”，对调用者来说不透明。
 concepts 是 **显式的约束**：调用端如果不满足约束，编译器直接告诉你“这个函数不存在，因为约束条件不成立”。
 这避免了**意外调用到错误重载**的情况。

------

## 4. 可组合性强

Concepts 可以组合、继承：

```cpp
template<typename T>
concept RankChecked = !std::is_same_v<T, nullcheck>;

template<typename T>
concept ValidPolicy = std::is_same_v<T, nullcheck> || RankChecked<T>;
```

你可以在函数里写：

```cpp
void foo() requires ValidPolicy<RankCheckPolicy>;
```

这样就能表达“必须是 nullcheck 或 rank_checked 之一”，而不用嵌套好几层 `enable_if`。

------

## 5. 配合 `if constexpr` 更干净

SFINAE 常用来做“根据类型走不同实现”。
 但 concepts 可以直接放在函数签名上，或者结合 `if constexpr` 在函数体里写：

```cpp
template<typename P>
void bar() {
    if constexpr (std::is_same_v<P, nullcheck>) {
        // 无 rank 逻辑
    } else {
        // 有 rank 逻辑
    }
}
```

比写一堆 `enable_if_t` 重载更清楚。

------

## 6. 现代 C++ 社区推荐

- **Boost**、**标准库**里已经用 concepts 替换掉很多旧的 SFINAE。
- 对新同事（特别是不熟模板的人）来说 concepts 更好解释：“这函数只对整数开放” vs “enable_if 推导失败”。

------

## 小结

**SFINAE**：偏底层技巧，功能强大但写法晦涩，报错难懂。
 **concepts**：语义直观、错误信息清晰、约束显式，能直接表达“这个函数对哪些类型有效”。

> 换句话说：**SFINAE 是编译器的小聪明，concepts 是语言级的规范**。

------

