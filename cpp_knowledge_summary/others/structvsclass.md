### 1. In **C**

- There are only **`struct`** (no `class`).
- A `struct` in C is just a collection of fields (no methods, no inheritance, no access control).
- Example:

```
struct Point {
    int x;
    int y;
};
```

That’s it. Functions are written separately and operate on `struct Point*`.

------

### 2. In **C++**

### MISRA C++ (2008) Rule 3-9-1 (a.k.a. C.2 guidance in some docs)

It states essentially:

- **Use `struct` only for passive objects with public data** (like PODs).

- **Use `class` when data abstraction is intended** (i.e. when you need invariants, encapsulation, or methods).

  

Both `struct` and `class` exist — but the only difference is the **default access control**:

- In `struct` → members are **public** by default.
- In `class` → members are **private** by default.

Otherwise, they are the same: both can have constructors, methods, inheritance, templates, etc.

```
struct A {
    int x;        // public by default
};

class B {
    int x;        // private by default
};
```

------

### 3. How to choose between `struct` and `class`

A common style guideline:

- Use **`struct`** for *plain data aggregates* (PODs) or when members can be changed independently, without strong invariants.
- Use **`class`** when you want to enforce *invariants*, encapsulation, or provide a controlled interface.

------

### 4. Your rule of thumb

> “if class has const → use class; if member can change independently → use struct”

That’s actually a nice intuition:

- **`class`**: when the object has internal rules (invariants) and you don’t want outside code to break them. You usually provide only const getters, controlled setters, or functions that enforce invariants.
- **`struct`**: when the object is just a bag of values, and each field is logically independent (like a coordinate pair, RGB color, config options, etc.).

------

✅ Summary:

- In **C**: only `struct`, simple aggregate.
- In **C++**: `struct` and `class` differ only by default access.
- **Guideline**:
  - Use `struct` for *plain data aggregates*.
  - Use `class` for *encapsulation and invariants*.

### Case 1: **`struct` for plain data (members independent)**

Here the members are just values that can vary independently. No rules tie them together:

```
struct Point {
    double x;
    double y;
};

// Client code
Point p{3.0, 4.0};
p.x = 10.0;   // allowed
p.y = -5.0;   // allowed
```

👉 Any code can change `x` or `y` freely. That’s fine because a point doesn’t have invariants (nothing breaks if `x` is negative).

------

### Case 2: **`class` when invariants matter**

Suppose we represent a **circle**. We want to ensure:

- `radius >= 0` always.
- Center coordinates can change freely, but radius must never be negative.

We use a `class` to enforce this invariant:

```
class Circle {
    double radius;   // keep private
    double cx, cy;   // center

public:
    Circle(double r, double x, double y)
        : radius(r), cx(x), cy(y) 
    {
        if (r < 0) throw std::invalid_argument("radius must be >= 0");
    }

    // getter
    double getRadius() const { return radius; }

    // controlled setter
    void setRadius(double r) {
        if (r < 0) throw std::invalid_argument("radius must be >= 0");
        radius = r;
    }

    // center can move independently
    void move(double dx, double dy) {
        cx += dx;
        cy += dy;
    }
};
```

Usage:

```
Circle c(5.0, 0.0, 0.0);
c.setRadius(10.0);  // ok
c.setRadius(-2.0);  // throws exception, invariant protected
```

👉 Here we **must** use `class` because invariants matter. `struct` would allow breaking the rules by direct field access.

------

### ✅ Rule of thumb restated

- **Use `struct`**: when data members are “just values” and changing them independently is fine.
- **Use `class`**: when members have relationships/constraints and you need to **guard invariants**.