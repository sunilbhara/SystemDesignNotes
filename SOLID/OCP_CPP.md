# Open-Closed Principle (OCP) - C++

## Definition

The **Open-Closed Principle (OCP)** states:

> Software entities (classes, modules, functions) should be **open for extension but closed for modification**.

### What does this mean?

* **Open for Extension** → New functionality can be added.
* **Closed for Modification** → Existing code should not be changed.

The goal is to add new behavior by creating new classes instead of modifying existing, tested code.

---

# ❌ Without OCP (Bad Design)

Suppose we have a discount calculator.

```cpp
#include <iostream>
using namespace std;

class DiscountCalculator
{
public:
    double calculateDiscount(string customerType, double amount)
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
};
```

## Usage

```cpp
int main()
{
    DiscountCalculator calculator;

    cout << calculator.calculateDiscount("Regular", 1000)
         << endl;

    cout << calculator.calculateDiscount("Premium", 1000)
         << endl;
}
```

---

## Problem

Business says:

* Add Gold Customer
* Add Platinum Customer
* Add Diamond Customer

We must keep modifying the same class.

```cpp
else if(customerType == "Gold")
{
    return amount * 0.15;
}
```

### Issues

* Existing code changes repeatedly.
* Higher risk of bugs.
* Requires retesting.
* Violates OCP.

---

# ✅ With OCP (Good Design)

Instead of large if-else chains, create an abstraction.

---

## Step 1: Create Abstract Base Class

```cpp
class DiscountStrategy
{
public:
    virtual double calculate(double amount) = 0;

    virtual ~DiscountStrategy() {}
};
```

### Explanation

```cpp
virtual double calculate(double amount) = 0;
```

This is called a **Pure Virtual Function**.

A class containing at least one pure virtual function becomes an **Abstract Class**.

Objects cannot be created directly from abstract classes.

```cpp
DiscountStrategy obj; // Error
```

The class only defines a contract.

---

## Step 2: Create Implementations

### Regular Customer

```cpp
class RegularDiscount : public DiscountStrategy
{
public:
    double calculate(double amount) override
    {
        return amount * 0.05;
    }
};
```

### Premium Customer

```cpp
class PremiumDiscount : public DiscountStrategy
{
public:
    double calculate(double amount) override
    {
        return amount * 0.10;
    }
};
```

---

## Step 3: Use Abstraction

```cpp
class DiscountCalculator
{
public:
    double calculateDiscount(
        DiscountStrategy* strategy,
        double amount)
    {
        return strategy->calculate(amount);
    }
};
```

Notice:

`DiscountCalculator` does not know about:

* RegularDiscount
* PremiumDiscount
* GoldDiscount

It only knows:

```cpp
DiscountStrategy
```

This is called **Programming to an Interface (Abstraction)**.

---

## Usage

```cpp
int main()
{
    DiscountCalculator calculator;

    RegularDiscount regular;
    PremiumDiscount premium;

    cout << calculator.calculateDiscount(
                &regular,
                1000)
         << endl;

    cout << calculator.calculateDiscount(
                &premium,
                1000)
         << endl;
}
```

Output:

```text
50
100
```

---

# New Requirement

Business says:

> Add Gold Customer with 15% discount.

Create a new class.

```cpp
class GoldDiscount : public DiscountStrategy
{
public:
    double calculate(double amount) override
    {
        return amount * 0.15;
    }
};
```

Usage:

```cpp
GoldDiscount gold;

cout << calculator.calculateDiscount(
            &gold,
            1000)
     << endl;
```

Output:

```text
150
```

---

# What Changed?

Nothing.

No modifications required in:

* DiscountCalculator
* RegularDiscount
* PremiumDiscount

We only added a new class.

This follows OCP.

---

# How Runtime Polymorphism Works

```cpp
DiscountStrategy* strategy =
    new PremiumDiscount();
```

At compile time:

```text
strategy -> DiscountStrategy*
```

At runtime:

```text
strategy -> PremiumDiscount Object
```

When this executes:

```cpp
strategy->calculate(1000);
```

C++ looks up the correct implementation using the **Virtual Table (vtable)**.

It automatically calls:

```cpp
PremiumDiscount::calculate()
```

This is called **Runtime Polymorphism**.

---

# Why virtual Keyword is Important

Without virtual:

```cpp
class DiscountStrategy
{
public:
    double calculate(double amount)
    {
        return 0;
    }
};
```

The base implementation would always be called.

With virtual:

```cpp
virtual double calculate(double amount) = 0;
```

C++ resolves the function dynamically at runtime.

This is what enables OCP.

---

# Why override Keyword is Recommended

```cpp
class PremiumDiscount : public DiscountStrategy
{
public:
    double calculate(double amount) override
    {
        return amount * 0.10;
    }
};
```

Benefits:

* Compiler checks correctness.
* Prevents accidental signature mismatches.
* Makes code easier to read.

Always use `override` when overriding virtual functions.

---

# Why Virtual Destructor?

```cpp
virtual ~DiscountStrategy() {}
```

Consider:

```cpp
DiscountStrategy* strategy =
    new PremiumDiscount();

delete strategy;
```

Without a virtual destructor:

```cpp
~PremiumDiscount()
```

may not execute correctly.

Always provide a virtual destructor in polymorphic base classes.

---

# Real World Example: Notification System

## Without OCP

```cpp
class NotificationService
{
public:
    void send(string type, string message)
    {
        if(type == "Email")
        {
            cout << "Email Sent: "
                 << message << endl;
        }
        else if(type == "SMS")
        {
            cout << "SMS Sent: "
                 << message << endl;
        }
    }
};
```

Problem:

Adding WhatsApp notifications requires modifying the same class.

---

## With OCP

### Abstract Base Class

```cpp
class Notification
{
public:
    virtual void send(string message) = 0;

    virtual ~Notification() {}
};
```

### Email

```cpp
class EmailNotification : public Notification
{
public:
    void send(string message) override
    {
        cout << "Email Sent: "
             << message << endl;
    }
};
```

### SMS

```cpp
class SmsNotification : public Notification
{
public:
    void send(string message) override
    {
        cout << "SMS Sent: "
             << message << endl;
    }
};
```

### Service

```cpp
class NotificationService
{
public:
    void sendNotification(
        Notification* notification,
        string message)
    {
        notification->send(message);
    }
};
```

### Usage

```cpp
NotificationService service;

EmailNotification email;
SmsNotification sms;

service.sendNotification(
    &email,
    "Hello");

service.sendNotification(
    &sms,
    "Hello");
```

### Add WhatsApp

```cpp
class WhatsAppNotification : public Notification
{
public:
    void send(string message) override
    {
        cout << "WhatsApp Sent: "
             << message << endl;
    }
};
```

No existing code changes required.

---

# Interview Questions & Answers

## Q1. What is Open-Closed Principle?

**Answer:**

> Open-Closed Principle states that software entities should be open for extension but closed for modification. New functionality should be added through new classes instead of modifying existing tested code.

---

## Q2. How do we achieve OCP in C++?

**Answer:**

Using:

* Abstract Classes
* Pure Virtual Functions
* Runtime Polymorphism
* Dynamic Binding

---

## Q3. What is a Pure Virtual Function?

**Answer:**

A pure virtual function is declared as:

```cpp
virtual void func() = 0;
```

It forces derived classes to provide their own implementation.

---

## Q4. Why is virtual required?

**Answer:**

Without virtual, function calls are resolved at compile time.

With virtual, function calls are resolved at runtime, enabling polymorphism and OCP.

---

## Q5. Why use a virtual destructor?

**Answer:**

To ensure proper cleanup when deleting derived objects through base class pointers.

```cpp
virtual ~Base() {}
```

---

## Q6. Which Design Pattern commonly implements OCP?

**Answer:**

**Strategy Pattern**

Example:

```text
DiscountStrategy
      |
      +-- RegularDiscount
      +-- PremiumDiscount
      +-- GoldDiscount
```

New strategies can be added without changing existing code.

---

# Common Interview Mistake

❌ Wrong Answer:

> OCP means we should never modify classes.

Incorrect.

✅ Correct Answer:

> OCP means existing classes should be stable and new behavior should be added through extension using abstractions and polymorphism rather than modifying existing code.

---

# Quick Revision (30 Seconds)

* OCP = Open for Extension, Closed for Modification.
* Avoid large if-else chains.
* Use Abstract Classes.
* Use Pure Virtual Functions.
* Use Runtime Polymorphism.
* Use Virtual Destructors.
* Strategy Pattern commonly implements OCP.
* Add new behavior by creating new derived classes.
* Existing code remains unchanged.

---

# C++ Interview One-Liner

> In C++, Open-Closed Principle is achieved using abstract base classes and virtual functions. New functionality is added through derived classes while existing code remains unchanged, making the system extensible and maintainable.
