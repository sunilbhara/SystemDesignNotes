# Strategy Design Pattern

## Definition

The **Strategy Design Pattern** is a **Behavioral Design Pattern** that defines a family of algorithms, encapsulates each algorithm in a separate class, and makes them interchangeable at runtime.

Instead of using large `if-else` or `switch` statements, the behavior is delegated to a strategy object.

### Intent

> Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

---

# Why Do We Need It?

Consider a payment system:

```csharp
if (paymentType == "CreditCard")
{
    // credit card logic
}
else if (paymentType == "PayPal")
{
    // paypal logic
}
else if (paymentType == "UPI")
{
    // upi logic
}
```

### Problems

* Violates Open/Closed Principle
* Difficult to maintain
* Difficult to extend
* Large conditional statements
* Tight coupling between client and algorithms

### Solution

Move each algorithm into a separate strategy class and let the client choose the strategy dynamically.

---

# Structure

```text
                    +------------------+
                    |     Strategy     |
                    +------------------+
                             ^
                             |
          ----------------------------------------
          |                  |                  |
          |                  |                  |
+----------------+ +----------------+ +----------------+
| Concrete A     | | Concrete B     | | Concrete C     |
+----------------+ +----------------+ +----------------+

                             ^
                             |
                     +---------------+
                     |    Context    |
                     +---------------+
```

---

# Components

## 1. Strategy

Common interface for all algorithms.

```csharp
public interface IPaymentStrategy
{
    void Pay(decimal amount);
}
```

---

## 2. Concrete Strategies

Actual implementations of algorithms.

```csharp
public class CreditCardPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card");
    }
}
```

```csharp
public class PayPalPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using PayPal");
    }
}
```

```csharp
public class UpiPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using UPI");
    }
}
```

---

## 3. Context

Maintains a reference to a strategy object.

```csharp
public class PaymentProcessor
{
    private IPaymentStrategy _strategy;

    public PaymentProcessor(IPaymentStrategy strategy)
    {
        _strategy = strategy;
    }

    public void SetStrategy(IPaymentStrategy strategy)
    {
        _strategy = strategy;
    }

    public void ProcessPayment(decimal amount)
    {
        _strategy.Pay(amount);
    }
}
```

---

## 4. Client

Chooses the strategy.

```csharp
var processor =
    new PaymentProcessor(new CreditCardPayment());

processor.ProcessPayment(100);

processor.SetStrategy(new PayPalPayment());
processor.ProcessPayment(200);

processor.SetStrategy(new UpiPayment());
processor.ProcessPayment(300);
```

### Output

```text
Paid 100 using Credit Card
Paid 200 using PayPal
Paid 300 using UPI
```

---

# Complete C++ Example

## Strategy Interface

```cpp
class PaymentStrategy
{
public:
    virtual void pay(double amount) = 0;
    virtual ~PaymentStrategy() = default;
};
```

---

## Concrete Strategies

```cpp
class CreditCardPayment : public PaymentStrategy
{
public:
    void pay(double amount) override
    {
        std::cout << "Paid " << amount
                  << " using Credit Card\n";
    }
};
```

```cpp
class PayPalPayment : public PaymentStrategy
{
public:
    void pay(double amount) override
    {
        std::cout << "Paid " << amount
                  << " using PayPal\n";
    }
};
```

```cpp
class UpiPayment : public PaymentStrategy
{
public:
    void pay(double amount) override
    {
        std::cout << "Paid " << amount
                  << " using UPI\n";
    }
};
```

---

## Context

```cpp
class PaymentProcessor
{
private:
    std::unique_ptr<PaymentStrategy> strategy;

public:
    PaymentProcessor(
        std::unique_ptr<PaymentStrategy> s)
        : strategy(std::move(s))
    {
    }

    void setStrategy(
        std::unique_ptr<PaymentStrategy> s)
    {
        strategy = std::move(s);
    }

    void processPayment(double amount)
    {
        strategy->pay(amount);
    }
};
```

---

## Client

```cpp
int main()
{
    PaymentProcessor processor(
        std::make_unique<CreditCardPayment>());

    processor.processPayment(100);

    processor.setStrategy(
        std::make_unique<PayPalPayment>());

    processor.processPayment(200);

    processor.setStrategy(
        std::make_unique<UpiPayment>());

    processor.processPayment(300);

    return 0;
}
```

---

# Real World Examples

## Payment Processing

```text
Credit Card
PayPal
UPI
Net Banking
```

---

## Route Planning

```text
Car Route
Bike Route
Walking Route
Public Transport Route
```

---

## Compression

```text
ZIP
RAR
7Z
GZIP
```

---

## Sorting

```text
Quick Sort
Merge Sort
Heap Sort
```

---

## Tax Calculation

```text
India Tax Rule
US Tax Rule
UK Tax Rule
```

---

# Advantages

### Open/Closed Principle

New strategies can be added without modifying existing code.

### Runtime Flexibility

Algorithms can be changed dynamically.

### Cleaner Code

Removes large conditional statements.

### Better Testing

Each strategy can be tested independently.

### Reusability

Strategies can be reused in different contexts.

---

# Disadvantages

### More Classes

Every algorithm usually requires a new class.

### Client Awareness

Client must know which strategy to select.

### Increased Object Creation

More objects may be created compared to a simple conditional solution.

---

# Strategy vs State Pattern

| Feature                 | Strategy         | State                  |
| ----------------------- | ---------------- | ---------------------- |
| Purpose                 | Select algorithm | Represent object state |
| Client chooses behavior | Yes              | Usually No             |
| Runtime switching       | Yes              | Yes                    |
| Focus                   | Algorithms       | State transitions      |
| Example                 | Payment methods  | ATM states             |

### Easy Memory Trick

```text
Strategy = HOW to do something

State = WHAT condition the object is in
```

---

# Strategy vs Template Method

| Strategy                     | Template Method                  |
| ---------------------------- | -------------------------------- |
| Uses Composition             | Uses Inheritance                 |
| Runtime flexibility          | Compile-time flexibility         |
| Change algorithm dynamically | Fixed algorithm structure        |
| Preferred in modern design   | Older inheritance-based approach |

### Easy Memory Trick

```text
Strategy = HAS-A relationship

Template Method = IS-A relationship
```

---

# Modern C# Strategy Using Delegates

```csharp
public class PaymentProcessor
{
    private Action<decimal> paymentStrategy;

    public PaymentProcessor(Action<decimal> strategy)
    {
        paymentStrategy = strategy;
    }

    public void ProcessPayment(decimal amount)
    {
        paymentStrategy(amount);
    }
}
```

Usage:

```csharp
var processor = new PaymentProcessor(
    amount => Console.WriteLine($"UPI Payment {amount}")
);

processor.ProcessPayment(100);
```

---

# Modern C++ Strategy Using std::function

```cpp
#include <functional>

class PaymentProcessor
{
private:
    std::function<void(double)> strategy;

public:
    PaymentProcessor(std::function<void(double)> s)
        : strategy(s)
    {
    }

    void processPayment(double amount)
    {
        strategy(amount);
    }
};
```

Usage:

```cpp
PaymentProcessor processor(
    [](double amount)
    {
        std::cout << "UPI Payment "
                  << amount << "\n";
    });

processor.processPayment(100);
```

---

# UML Diagram

```text
+------------------------+
|      Context           |
+------------------------+
| - strategy             |
+------------------------+
| + execute()            |
+------------------------+
            |
            v
+------------------------+
|      Strategy          |
+------------------------+
| + algorithm()          |
+------------------------+
            ^
            |
    -------------------
    |                 |
    v                 v
+----------+    +----------+
|StrategyA |    |StrategyB |
+----------+    +----------+
```

---

# Interview Questions

## Q1. What is the Strategy Pattern?

**Answer:**

A behavioral design pattern that encapsulates algorithms into separate classes and allows them to be selected and changed at runtime.

---

## Q2. What problem does Strategy Pattern solve?

**Answer:**

It eliminates large conditional statements (`if-else`, `switch`) and allows algorithms to vary independently from the client.

---

## Q3. Which SOLID principle does it support?

### Open/Closed Principle

```text
Open for extension
Closed for modification
```

New strategies can be added without changing existing code.

---

## Q4. Why is Strategy Pattern preferred over inheritance?

**Answer:**

Because it uses composition instead of inheritance, providing greater flexibility and runtime behavior changes.

---

## Q5. What is the relationship between Context and Strategy?

**Answer:**

HAS-A relationship.

```text
Context HAS-A Strategy
```

Not:

```text
Context IS-A Strategy
```

---

## Q6. Give Real-world Examples

* Payment processing
* Tax calculation
* Route navigation
* Sorting algorithms
* Compression algorithms
* Authentication providers

---

## Q7. Difference Between Strategy and State?

### Strategy

```text
Client chooses behavior.
```

### State

```text
Object changes behavior based on internal state.
```

---

## Q8. When Should You Use Strategy Pattern?

Use when:

* Multiple algorithms exist for the same task.
* Algorithms may change at runtime.
* Large conditional logic exists.
* You want adherence to SOLID principles.

---

# Quick Revision (30 Seconds)

### Definition

Encapsulate interchangeable algorithms into separate classes.

### Key Idea

```text
Favor Composition Over Inheritance
```

### Components

```text
Strategy
Concrete Strategy
Context
Client
```

### Benefits

```text
Runtime Flexibility
Open/Closed Principle
Cleaner Code
Easy Testing
```

### Memory Trick

```text
Context HAS-A Strategy
Strategy decides HOW work is done
```
