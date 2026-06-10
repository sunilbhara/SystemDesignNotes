# Liskov Substitution Principle (LSP) - C#

## Definition

The **Liskov Substitution Principle (LSP)** states:

> Objects of a derived class should be replaceable with objects of the base class without altering the correctness of the program.

In simple words:

> If Class B inherits from Class A, then Class B should be able to replace Class A without breaking the application.

---

# Understanding LSP

If code works with:

```text
Rectangle
```

then it should also work correctly with:

```text
Square
```

if Square inherits Rectangle.

If replacing the parent with the child breaks behavior, LSP is violated.

---

# ❌ Without LSP (Bad Design)

## Rectangle

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }

    public virtual int Height { get; set; }

    public int GetArea()
    {
        return Width * Height;
    }
}
```

---

## Square

```csharp
public class Square : Rectangle
{
    public override int Width
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }

    public override int Height
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }
}
```

---

## Client Code

```csharp
Rectangle rectangle = new Square();

rectangle.Width = 5;
rectangle.Height = 10;

Console.WriteLine(rectangle.GetArea());
```

Expected:

```text
50
```

Actual:

```text
100
```

---

## Why?

The client expects:

```text
Width = 5
Height = 10
Area = 50
```

But Square forces:

```text
Width = Height
```

Result:

```text
10 * 10 = 100
```

The derived class changed the behavior expected from the base class.

LSP is violated.

---

# ✅ Following LSP

Instead of inheritance, use a common abstraction.

---

## Shape

```csharp
public interface IShape
{
    int GetArea();
}
```

---

## Rectangle

```csharp
public class Rectangle : IShape
{
    public int Width { get; set; }

    public int Height { get; set; }

    public int GetArea()
    {
        return Width * Height;
    }
}
```

---

## Square

```csharp
public class Square : IShape
{
    public int Side { get; set; }

    public int GetArea()
    {
        return Side * Side;
    }
}
```

---

## Usage

```csharp
IShape rectangle =
    new Rectangle
    {
        Width = 5,
        Height = 10
    };

IShape square =
    new Square
    {
        Side = 5
    };

Console.WriteLine(rectangle.GetArea());
Console.WriteLine(square.GetArea());
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

Bird contains:

```csharp
public virtual void Fly()
```

Ostrich cannot fly.

To implement Ostrich:

```csharp
throw new NotImplementedException();
```

LSP violated.

---

## Better Design

```text
Bird
   |
   +-- Sparrow

Bird
   |
   +-- Ostrich

IFlyable
   |
   +-- Sparrow
```

Only birds that can fly implement IFlyable.

---

# Interview Questions

## Q1. What is LSP?

**Answer**

> LSP states that derived classes should be substitutable for their base classes without changing the correctness of the program.

---

## Q2. What happens when LSP is violated?

**Answer**

The derived class changes expected behavior, causing bugs and unexpected results.

---

## Q3. What is the most famous LSP example?

**Answer**

Rectangle-Square.

A Square is not always a proper behavioral substitute for Rectangle.

---

## Q4. How can we fix LSP violations?

**Answer**

* Use interfaces
* Use composition
* Create better abstractions
* Avoid incorrect inheritance

---

## Q5. Which SOLID principles are related?

**Answer**

* OCP
* ISP
* DIP

A poor inheritance hierarchy often violates multiple principles.

---

# Common Interview Mistake

❌ Wrong Answer

> LSP means child class should inherit parent class.

Incorrect.

✅ Correct Answer

> LSP means a child class should be able to replace the parent class without changing program behavior.

---

# Quick Revision (30 Seconds)

* LSP = Derived objects must replace base objects safely.
* Child classes must honor parent behavior.
* Rectangle-Square is the classic example.
* Prefer interfaces over incorrect inheritance.
* Violations often indicate bad class design.

---

# C# Interview One-Liner

> LSP states that derived classes must be substitutable for their base classes without affecting program correctness. If replacing a parent object with a child object breaks behavior, LSP is violated.
