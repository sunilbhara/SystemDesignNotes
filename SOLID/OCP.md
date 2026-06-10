# Open-Closed Principle (OCP) - SOLID

## Definition

The **Open-Closed Principle (OCP)** states:

> Software entities (classes, modules, functions) should be **open for extension but closed for modification**.

### What does this mean?

* **Open for Extension** → We should be able to add new functionality.
* **Closed for Modification** → Existing, tested code should not need to be changed.

The goal is to add new behavior by creating new classes rather than modifying existing ones.

---

# ❌ Without OCP (Bad Design)

Suppose we have a discount calculator.

```csharp
public class DiscountCalculator
{
    public double CalculateDiscount(string customerType, double amount)
    {
        if (customerType == "Regular")
        {
            return amount * 0.05;
        }
        else if (customerType == "Premium")
        {
            return amount * 0.10;
        }

        return 0;
    }
}
```

## Usage

```csharp
DiscountCalculator calculator = new DiscountCalculator();

double regularDiscount =
    calculator.CalculateDiscount("Regular", 1000);

double premiumDiscount =
    calculator.CalculateDiscount("Premium", 1000);
```

---

## Problem

Now business says:

* Add Gold Customer
* Add Platinum Customer
* Add Diamond Customer

We must keep modifying the same class.

```csharp
else if (customerType == "Gold")
{
    return amount * 0.15;
}
```

### Issues

* Existing code changes frequently.
* High chance of introducing bugs.
* Every change requires retesting.
* Violates Open-Closed Principle.

---

# ✅ With OCP (Good Design)

Instead of using if-else conditions, create an abstraction.

---

## Step 1: Create Interface

```csharp
public interface IDiscountStrategy
{
    double CalculateDiscount(double amount);
}
```

This interface defines a contract:

> Every discount type must know how to calculate its discount.

---

## Step 2: Create Implementations

### Regular Customer

```csharp
public class RegularDiscount : IDiscountStrategy
{
    public double CalculateDiscount(double amount)
    {
        return amount * 0.05;
    }
}
```

### Premium Customer

```csharp
public class PremiumDiscount : IDiscountStrategy
{
    public double CalculateDiscount(double amount)
    {
        return amount * 0.10;
    }
}
```

---

## Step 3: Use the Interface

```csharp
public class DiscountCalculator
{
    public double Calculate(
        IDiscountStrategy strategy,
        double amount)
    {
        return strategy.CalculateDiscount(amount);
    }
}
```

Notice that `DiscountCalculator` does not know about:

* RegularDiscount
* PremiumDiscount
* GoldDiscount

It only depends on:

```csharp
IDiscountStrategy
```

---

## Usage

```csharp
DiscountCalculator calculator =
    new DiscountCalculator();

double regularDiscount =
    calculator.Calculate(
        new RegularDiscount(),
        1000);

double premiumDiscount =
    calculator.Calculate(
        new PremiumDiscount(),
        1000);
```

---

# New Requirement

Business says:

> Add Gold Customer with 15% discount.

Create a new class.

```csharp
public class GoldDiscount : IDiscountStrategy
{
    public double CalculateDiscount(double amount)
    {
        return amount * 0.15;
    }
}
```

Usage:

```csharp
double goldDiscount =
    calculator.Calculate(
        new GoldDiscount(),
        1000);
```

### What changed?

Nothing.

No modifications were required in:

* DiscountCalculator
* RegularDiscount
* PremiumDiscount

We simply extended the system.

This follows OCP.

---

# How Polymorphism Helps OCP

```csharp
IDiscountStrategy strategy =
    new PremiumDiscount();
```

At compile time:

```text
strategy → IDiscountStrategy
```

At runtime:

```text
strategy → PremiumDiscount
```

When this executes:

```csharp
strategy.CalculateDiscount(1000);
```

C# automatically calls:

```csharp
PremiumDiscount.CalculateDiscount()
```

This is called **Runtime Polymorphism**.

Polymorphism allows us to add new implementations without changing existing code.

---

# Real World Example: Notification System

## Without OCP

```csharp
public class NotificationService
{
    public void Send(string type, string message)
    {
        if(type == "Email")
        {
            Console.WriteLine($"Email Sent: {message}");
        }
        else if(type == "SMS")
        {
            Console.WriteLine($"SMS Sent: {message}");
        }
    }
}
```

Problem:

Adding WhatsApp or Push Notifications requires modifying this class.

---

## With OCP

### Interface

```csharp
public interface INotification
{
    void Send(string message);
}
```

### Email

```csharp
public class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"Email Sent: {message}");
    }
}
```

### SMS

```csharp
public class SmsNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS Sent: {message}");
    }
}
```

### Service

```csharp
public class NotificationService
{
    public void SendNotification(
        INotification notification,
        string message)
    {
        notification.Send(message);
    }
}
```

### Usage

```csharp
NotificationService service =
    new NotificationService();

service.SendNotification(
    new EmailNotification(),
    "Hello");

service.SendNotification(
    new SmsNotification(),
    "Hello");
```

### Add WhatsApp

```csharp
public class WhatsAppNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"WhatsApp Sent: {message}");
    }
}
```

No existing code changes required.

---

# Interview Questions & Answers

## Q1. What is Open-Closed Principle?

**Answer:**

> Open-Closed Principle states that software entities should be open for extension but closed for modification. New functionality should be added through new classes or implementations without modifying existing tested code.

---

## Q2. How do we achieve OCP in C#?

**Answer:**

We achieve OCP using:

* Interfaces
* Abstract Classes
* Runtime Polymorphism
* Dependency Injection

---

## Q3. Why is OCP important?

**Answer:**

Benefits:

* Reduces bugs in existing code.
* Improves maintainability.
* Improves scalability.
* Encourages loose coupling.
* Easier to test and extend.

---

## Q4. Which C# concepts are commonly used for OCP?

**Answer:**

* Interfaces
* Abstract Classes
* Polymorphism
* Dependency Injection
* Strategy Pattern

---

## Q5. Which Design Pattern is most closely related to OCP?

**Answer:**

The **Strategy Pattern** is one of the most common implementations of OCP.

Example:

```text
IDiscountStrategy
    |
    +-- RegularDiscount
    +-- PremiumDiscount
    +-- GoldDiscount
```

New strategies can be added without modifying existing code.

---

# Common Interview Mistake

❌ Wrong Answer:

> OCP means classes should not be modified.

This is incomplete.

✅ Correct Answer:

> OCP means classes should be open for extension but closed for modification. New behavior should be introduced through abstractions and polymorphism rather than modifying existing code.

---

# Quick Revision (30 Seconds)

* OCP = Open for Extension, Closed for Modification.
* Add functionality through new classes.
* Avoid large if-else chains.
* Use Interfaces or Abstract Classes.
* Achieve using Runtime Polymorphism.
* Strategy Pattern is a common implementation.
* Benefits: Maintainability, Scalability, Low Coupling, Easy Testing.
