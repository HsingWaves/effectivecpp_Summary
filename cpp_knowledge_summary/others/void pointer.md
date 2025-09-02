

------

### 1. 在 C 语言里 (`void*` 是万能指针)

- 在 C 中，`void*` 被定义为一种通用指针类型。

- **可以自动、双向转换**：

  ```c
  int* pi;
  void* pv = pi;   // int* -> void* 自动转换
  pi = pv;         // void* -> int* 自动转换
  ```

- 这是 C 语言的特性，标准保证 `void*` 可以无损表示任何对象指针。

------

### 2. 在 C++ 里 (`void*` 更严格)

- 在 C++ 中，规则改了：

  - **允许**任意对象指针自动转换为 `void*`（单向）。

  - **禁止**`void*` 自动转换为其他类型指针，需要显式转换：

    ```cpp
    int* pi;
    void* pv = pi;          // OK，自动转换
    pi = pv;                // ❌ 编译错误
    pi = (int*)pv;          // ✅ 需要显式转换
    pi = static_cast<int*>(pv); // ✅ 推荐
    ```

- 这样做的原因是：C++ 引入了强类型系统（type safety），不想让 `void*` 像 C 那样随便“回跳”成其他指针，避免野指针或类型错误。

------

### 3. 小结对比

| 特性             | C            | C++                      |
| ---------------- | ------------ | ------------------------ |
| 对象指针 → void* | 自动 (OK)    | 自动 (OK)                |
| void* → 对象指针 | 自动 (OK)    | ❌ 需要显式 cast          |
| 原因             | 通用指针设计 | 强类型安全，减少潜在错误 |

------



- **C = bidirectional**
- **C++ = only object pointer → void\***

------

**the “universal pointer” (`void\*`)**

------

## 1. What is `void*`

- `void*` is a special pointer type: a **pointer to an incomplete type**.
- It can hold the address of any object type, but it **does not know the object’s size or type**.
- Therefore:
  - You **cannot dereference** a `void*` directly (`*pv` is illegal).
  - You **cannot do pointer arithmetic** on it (`pv++` is illegal).
  - You can only store/transfer it, and later **cast it back** to the correct type.

```c
void* p;
int x = 42;
p = &x;  // p points to an int
```

------

## 2. In C (true “universal pointer”)

In C, `void*` is a **generic object pointer**:

- Any object pointer can **implicitly** convert to `void*`.
- A `void*` can **implicitly** convert back to any object pointer.

```c
int  a = 10;
char c = 'A';

void* pv;

pv = &a;        // int* -> void*
int* pa = pv;   // void* -> int* (automatic in C)

pv = &c;        
char* pc = pv;  // void* -> char* (automatic in C)
```

Examples:

- `malloc()` returns `void*`, and you assign it to the type you need:

  ```c
  int* arr = malloc(10 * sizeof(int));
  ```

- `qsort()` and `bsearch()` take `void*` to allow **generic functions**.

------

## 3. In C++ (stricter rules)

C++ enforces stronger type safety:

- **Object pointer → `void\*`** ✅ allowed automatically.

- **`void\*` → object pointer** ❌ not automatic, requires an explicit cast:

  ```cpp
  int x = 10;
  void* pv = &x;      // OK
  int* pi = pv;       // ❌ Error
  int* pi = (int*)pv; // ✅ explicit cast
  int* pj = static_cast<int*>(pv); // ✅ preferred in C++
  ```

Reason: prevent mistakes where a `void*` might be reinterpreted incorrectly.

------

## 4. Typical Uses of `void*`

1. **Generic memory allocation**

   ```c
   int* arr = (int*)malloc(100 * sizeof(int)); // in C++
   ```

2. **Generic library functions**

   ```c
   int cmp(const void* a, const void* b) {
       return *(int*)a - *(int*)b;
   }
   ```

3. **System APIs (POSIX threads, WinAPI, etc.)**

   ```c
   void* thread_func(void* arg) {
       int* p = (int*)arg;
       printf("%d\n", *p);
       return NULL;
   }
   ```

4. **Bridging C and C++ code**

   - Used when C++ needs to interoperate with legacy C libraries.

------

## 5. Limitations

- Cannot dereference (`*pv`) directly.
- Cannot perform pointer arithmetic (`pv + 1`).
- Cannot store function pointers (different type system).

------

## ✅ Summary

- **C**: `void*` is a *true universal pointer* — conversions are **bidirectional and implicit**.
- **C++**: still universal, but conversions back require **explicit casting**.
- `void*` is useful for **memory allocation, generic libraries, and system APIs**.
- In modern C++, you typically use **templates** or safer abstractions (`std::vector`, `std::any`, `std::variant`) instead of raw `void*`.

------

Exactly — in **modern C++** you don’t often see `void*`.

------

## 1. Why `void*` was common in C

- C had no **templates** or **generics**.
- `void*` was the only way to write *generic* code (e.g. `malloc`, `qsort`, `bsearch`).
- It acted as the “universal object pointer.”

------

## 2. Why C++ avoids `void*`

C++ provides **safer, type-aware alternatives**:

- **Templates**
   Instead of a generic `void*`, you just use templates:

  ```cpp
  template <typename T>
  T* allocate(size_t n) {
      return new T[n];
  }
  ```

- **Containers**
   Instead of handling `void*` arrays, you use STL containers:

  ```cpp
  std::vector<int> v(100);
  ```

- **Type-erased utilities**
   C++17/20 introduced safer alternatives:

  - `std::any` – can hold any type, with type checking.
  - `std::variant` – tagged union with compile-time type safety.
  - `std::function` – for function pointers instead of `void*`.

------

## 3. Where `void*` still appears in C++

- **Legacy C APIs** (POSIX threads, WinAPI, etc.)

  ```cpp
  void* thread_func(void* arg) {
      int* p = static_cast<int*>(arg);
      return nullptr;
  }
  ```

- **Low-level systems code** (e.g., device drivers, memory pools).

- **Interfacing with `malloc`** or C libraries.

But if you’re writing *modern C++ code only*, you rarely need it.

------

## ✅ Takeaway

- In **C**, `void*` is *everywhere* (universal pointer, generic APIs).
- In **modern C++**, it’s **uncommon** — usually replaced by **templates**, **STL containers**, or **type-erased utilities**.
- You’ll mostly see `void*` only when interacting with **C code, OS APIs, or very low-level libraries**.

------

