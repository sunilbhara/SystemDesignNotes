# Liskov Substitution Principle (LSP) - C++

## Definition

The **Liskov Substitution Principle (LSP)** states:

> Objects of a derived class should be replaceable with objects of the base class without changing the correctness of the program.

In simple words:

> A derived class should behave like its base class from the client's perspective.

---

# Understanding LSP

If a function works with:

```cpp
Rectangle*
```

it should also work correctly with:

```cpp
Square*
```

if Square inherits Rectangle.

If behavior changes unexpectedly, LSP is violated.

---

# ❌ Without LSP (Bad Design)

## Rectangle

```cpp
class Rectangle
{
protected:
    int width;
    int height;

public:
    virtual void setWidth(int w)
    {
        width = w;
    }

    virtual void setHeight(int h)
    {
        height = h;
    }

    int getArea()
    {
        return width * height;
    }
};
```

---

## Square

```cpp
class Square : public Rectangle
{
public:
    void setWidth(int w) override
    {
        width = height = w;
    }

    void setHeight(int h) override
    {
        width = height = h;
    }
};
```

---

## Client Code

```cpp
Rectangle* shape = new Square();

shape->setWidth(5);
shape->setHeight(10);

cout << shape->getArea();
```

Expected:

```text
50
```

Actual:

```text
100
```

LSP is violated.

---

# Why?

The client expects:

```text
Width = 5
Height = 10
```

But Square forces:

```text
Width = Height
```

The derived class changes the behavior expected from Rectangle.

---

# ✅ Following LSP

Use a common abstraction.

---

## Shape

```cpp
class Shape
{
public:
    virtual int getArea() = 0;

    virtual ~Shape() {}
};
```

---

## Rectangle

```cpp
class Rectangle : public Shape
{
private:
    int width;
    int height;

public:
    Rectangle(int w, int h)
    {
        width = w;
        height = h;
    }

    int getArea() override
    {
        return width * height;
    }
};
```

---

## Square

```cpp
class Square : public Shape
{
private:
    int side;

public:
    Square(int s)
    {
        side = s;
    }

    int getArea() override
    {
        return side * side;
    }
};
```

---

## Usage

```cpp
Shape* rectangle =
    new Rectangle(5, 10);

Shape* square =
    new Square(5);

cout << rectangle->getArea() << endl;
cout << square->getArea() << endl;
```

Output:

```text
50
25
```

No unexpected behavior.

LSP is satisfied.

---

# Real World Example

## Bad Design

```text
Bird
  |
  +-- Sparrow
  |
  +-- Ostrich
```

Bird defines:

```cpp
virtual void fly()
```

Ostrich cannot fly.

Violation:

```cpp
throw runtime_error("Cannot Fly");
```

---

## Better Design

```text
Bird
   |
   +-- Sparrow

Bird
   |
   +-- Ostrich

Flyable
   |
   +-- Sparrow
```

Only flying birds implement Flyable.

---

# Interview Questions

## Q1. What is LSP?

**Answer**

> LSP states that derived classes should be usable in place of base classes without changing program correctness.

---

## Q2. What is the classic LSP example?

**Answer**

Rectangle-Square.

---

## Q3. What are signs of LSP violation?

**Answer**

* Throwing exceptions in overridden methods
* Empty implementations
* Unexpected behavior changes
* Excessive type checking

---

## Q4. How do we avoid LSP violations?

**Answer**

* Better abstractions
* Composition over inheritance
* Interfaces
* Proper inheritance hierarchies

---

## Q5. Why is LSP important?

**Answer**

It ensures polymorphism works correctly and keeps systems extensible and maintainable.

---

# Common Interview Mistake

❌ Wrong Answer

> LSP is about inheritance.

Incomplete.

✅ Correct Answer

> LSP is about behavioral compatibility between parent and child classes.

---

# Quick Revision (30 Seconds)

* LSP = Child should safely replace Parent.
* Focus on behavior, not inheritance.
* Rectangle-Square is the classic example.
* Use interfaces when inheritance is inappropriate.
* Violations break polymorphism.

---

# C++ Interview One-Liner

> LSP states that derived classes should be completely substitutable for their base classes. If replacing a base object with a derived object changes expected behavior, the inheritance hierarchy is incorrect.
