```
#include <tuple>
#include <string>

std::tuple<int, double, std::string> foo() {
    return {42, 3.14, "hello"};
}

int main() {
    auto [x, y, z] = foo(); // structured bindings (C++17)
}

```

**Pros:**

- Easy to return ad-hoc multiple values without defining a new type.
- Works well for small, temporary groupings.
- Can use `std::tie` or structured bindings for unpacking.

**Cons:**

- Semantics can be unclear without names — `{42, 3.14, "hello"}` doesn’t tell you what each value means.
- Can reduce readability in APIs if overused.

```
struct FooResult {
    int code;
    double weight;
    std::string message;
};

FooResult foo() {
    return {42, 3.14, "hello"};
}

int main() {
    FooResult r = foo();
    // r.code, r.weight, r.message
}

```

- **Pros:**
  - Named fields improve clarity and maintainability.
  - Can add methods or invariants later.
  - Self-documenting in API/headers.
- **Cons:**
  - Slightly more boilerplate (need to define a type).
  - May feel heavy for very temporary “just return two things” use cases.

### Why product code favors `struct`

- **Readability & self-documenting APIs.** Named fields beat `get<1>()`. Call sites and headers are clearer, reviews are easier.
- **Future-proofing.** You can add invariants, helper methods, and validation later without changing call sites.
- **Zero cost.** Aggregates optimize just like tuples; NRVO/move elision apply. For non-empty members, there’s no performance penalty vs `tuple`.
- **Tooling & ABI hygiene.** Debuggers show field names; static analyzers/reviewers reason about intent.

### Why `tuple` shows up in metaprogramming

- **Type-level container.** Works great with packs, `std::index_sequence`, `std::apply`, `std::tuple_cat`—all the generic glue.
- **Heterogeneous algorithms.** Perfect for writing “for each element type” utilities, forwarding bundles, and adaptor combinators.
- **EBO tricks.** `std::tuple` exploits empty base optimization to store empty types with zero size—handy in policy/CRTP designs.

### Practical guidance

- **Public API / domain objects:** prefer a `struct` with named fields.

- **Local helpers / internal plumbing / TMP:** `tuple` (or `pair`) is fine.

- **Two named things but you want tuple-like ergonomics:** give your `struct` a tuple interface.

  Mini-example: make a `struct` unpack like a tuple

  ```
  #include <tuple>
  struct Point { int x; int y; };
  
  // enable structured bindings & get<N>(Point)
  namespace std {
    template<> struct tuple_size<Point> : std::integral_constant<size_t, 2> {};
    template<> struct tuple_element<0, Point> { using type = int; };
    template<> struct tuple_element<1, Point> { using type = int; };
  }
  template<size_t I> auto& get(Point& p)       { if constexpr(I==0) return p.x; else return p.y; }
  template<size_t I> const auto& get(const Point& p){ if constexpr(I==0) return p.x; else return p.y; }
  
  int main() {
      Point p{1,2};
      auto [x, y] = p;       // structured bindings
      int xx = get<0>(p);    // tuple-like access
  }
  
  ```

  ### Edge notes

  - `tuple` layout is unspecified; don’t rely on it for ABI. `struct` with standard-layout can be shared more predictably.