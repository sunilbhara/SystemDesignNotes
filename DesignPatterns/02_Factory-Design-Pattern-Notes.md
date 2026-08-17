# Factory Design Pattern

## What is Factory Design Pattern?

The **Factory Design Pattern** is a **Creational Design Pattern** that provides an interface for creating objects without exposing the object creation logic to the client.

Instead of creating objects directly using the `new` keyword, the client requests the factory to create the object.

### Intent

> Define an interface for creating an object, but let subclasses or a factory class decide which class to instantiate.

---

# Why Do We Need Factory Pattern?

Suppose we have a notification system.

Without Factory Pattern:

```csharp
Notification notification;

if(type == "Email")
{
    notification = new EmailNotification();
}
else if(type == "SMS")
{
    notification = new SMSNotification();
}
else if(type == "Push")
{
    notification = new PushNotification();
}
```

### Problems

* Tight coupling
* Violates Open/Closed Principle
* Object creation logic scattered throughout code
* Difficult to maintain

---

# Solution

Move object creation into a dedicated Factory class.

```text
Client
   |
   v
Factory
   |
   +------> EmailNotification
   |
   +------> SMSNotification
   |
   +------> PushNotification
```

---

# Factory Pattern Structure

```text
              Product Interface
                     ^
                     |
        ------------------------------
        |             |              |
        |             |              |
 EmailProduct   SMSProduct    PushProduct

                     ^
                     |
                 Factory
                     ^
                     |
                   Client
```

---

# Components

## Product

Defines common interface.

## Concrete Product

Actual implementations.

## Factory

Contains object creation logic.

## Client

Uses factory instead of creating objects directly.

---

# Real-World Analogy

Think of a restaurant.

Without Factory:

```text
Customer enters kitchen
Customer cooks food
Customer serves food
```

With Factory:

```text
Customer -> Order Counter

Order Counter (Factory)
            |
            +--> Pizza
            +--> Burger
            +--> Pasta
```

Customer doesn't know how food is prepared.

---

# C# Example

## Step 1: Product Interface

```csharp
public interface INotification
{
    void Send();
}
```

---

## Step 2: Concrete Products

### Email Notification

```csharp
public class EmailNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending Email Notification");
    }
}
```

### SMS Notification

```csharp
public class SMSNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending SMS Notification");
    }
}
```

### Push Notification

```csharp
public class PushNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending Push Notification");
    }
}
```

---

## Step 3: Factory

```csharp
public class NotificationFactory
{
    public static INotification CreateNotification(string type)
    {
        switch(type)
        {
            case "Email":
                return new EmailNotification();

            case "SMS":
                return new SMSNotification();

            case "Push":
                return new PushNotification();

            default:
                throw new ArgumentException("Invalid type");
        }
    }
}
```

---

## Step 4: Client

```csharp
class Program
{
    static void Main()
    {
        INotification notification =
            NotificationFactory.CreateNotification("Email");

        notification.Send();
    }
}
```

### Output

```text
Sending Email Notification
```

---

# Complete C++ Example

## Product Interface

```cpp
class Notification
{
public:
    virtual void send() = 0;
    virtual ~Notification() = default;
};
```

---

## Concrete Products

### Email Notification

```cpp
class EmailNotification : public Notification
{
public:
    void send() override
    {
        std::cout << "Sending Email Notification\n";
    }
};
```

### SMS Notification

```cpp
class SMSNotification : public Notification
{
public:
    void send() override
    {
        std::cout << "Sending SMS Notification\n";
    }
};
```

### Push Notification

```cpp
class PushNotification : public Notification
{
public:
    void send() override
    {
        std::cout << "Sending Push Notification\n";
    }
};
```

---

## Factory

```cpp
class NotificationFactory
{
public:
    static std::unique_ptr<Notification>
    createNotification(const std::string& type)
    {
        if(type == "Email")
            return std::make_unique<EmailNotification>();

        if(type == "SMS")
            return std::make_unique<SMSNotification>();

        if(type == "Push")
            return std::make_unique<PushNotification>();

        return nullptr;
    }
};
```

---

## Client

```cpp
int main()
{
    auto notification =
        NotificationFactory::createNotification("Email");

    notification->send();

    return 0;
}
```

### Output

```text
Sending Email Notification
```

---

# Before vs After Factory Pattern

## Without Factory

```csharp
var email = new EmailNotification();
var sms = new SMSNotification();
var push = new PushNotification();
```

### Issues

* Client knows concrete classes.
* Tight coupling.
* Hard to extend.

---

## With Factory

```csharp
var notification =
    NotificationFactory.CreateNotification("Email");
```

### Benefits

* Client knows only factory.
* Loose coupling.
* Easy to extend.

---

# Real-World Use Cases

## Database Connections

```text
MySQL Connection
SQL Server Connection
PostgreSQL Connection
MongoDB Connection
```

Factory decides which connection object to create.

---

## Payment Gateways

```text
Stripe
PayPal
Razorpay
Square
```

---

## Cloud Providers

```text
AWS
Azure
GCP
```

---

## Logging Systems

```text
File Logger
Database Logger
Cloud Logger
```

---

## UI Components

```text
Windows Button
Mac Button
Linux Button
```

---

# Advantages

## Loose Coupling

Client depends on abstraction.

---

## Centralized Object Creation

All creation logic stays in one place.

---

## Better Maintainability

Easy to add new products.

---

## Open/Closed Principle

Add new classes without changing client code.

---

## Cleaner Code

Removes repeated object creation logic.

---

# Disadvantages

## More Classes

Additional factory classes are required.

---

## Factory Can Become Large

Too many products may create a huge switch statement.

---

## Slight Complexity

For small projects, direct object creation may be simpler.

---

# Factory vs Simple Factory vs Factory Method

This is a common interview topic.

## 1. Simple Factory

```text
One factory class
One create() method
Returns different objects
```

Example:

```csharp
NotificationFactory.CreateNotification()
```

This is what we implemented above.

---

## 2. Factory Method

Creation responsibility is delegated to subclasses.

```text
Creator
   ^
   |
ConcreteCreator
```

Each subclass decides what object to create.

---

## 3. Abstract Factory

Creates families of related objects.

Example:

```text
Windows UI
   -> Button
   -> Checkbox

Mac UI
   -> Button
   -> Checkbox
```

Creates multiple related products together.

---

# Factory Method Example (Interview Level)

## Product

```csharp
public interface ITransport
{
    void Deliver();
}
```

---

## Concrete Products

```csharp
public class Truck : ITransport
{
    public void Deliver()
    {
        Console.WriteLine("Deliver by Road");
    }
}
```

```csharp
public class Ship : ITransport
{
    public void Deliver()
    {
        Console.WriteLine("Deliver by Sea");
    }
}
```

---

## Creator

```csharp
public abstract class Logistics
{
    public abstract ITransport CreateTransport();
}
```

---

## Concrete Creators

```csharp
public class RoadLogistics : Logistics
{
    public override ITransport CreateTransport()
    {
        return new Truck();
    }
}
```

```csharp
public class SeaLogistics : Logistics
{
    public override ITransport CreateTransport()
    {
        return new Ship();
    }
}
```

---

## Client

```csharp
Logistics logistics = new RoadLogistics();

ITransport transport =
    logistics.CreateTransport();

transport.Deliver();
```

---

# UML Diagram

```text
                     +----------------+
                     |   Product      |
                     +----------------+
                             ^
                             |
                  ---------------------
                  |                   |
                  |                   |
           EmailProduct        SMSProduct

                             ^
                             |
                     +----------------+
                     |    Factory     |
                     +----------------+
                             ^
                             |
                           Client
```

---

# Factory Pattern and SOLID Principles

## Dependency Inversion Principle

Client depends on abstraction.

```text
Client -> INotification
```

NOT

```text
Client -> EmailNotification
```

---

## Open/Closed Principle

Add new notification types without modifying client code.

---

# Interview Questions

## Q1. What is Factory Pattern?

### Answer

A creational design pattern that encapsulates object creation logic and provides objects through a factory instead of direct instantiation.

---

## Q2. Why use Factory Pattern?

### Answer

* Reduce coupling
* Hide creation logic
* Improve maintainability
* Support Open/Closed Principle

---

## Q3. What problem does Factory solve?

### Answer

Eliminates direct object creation and centralizes creation logic.

---

## Q4. Which SOLID principle does Factory support?

### Answer

#### Dependency Inversion Principle

```text
Depend on abstractions
Not concrete classes
```

#### Open/Closed Principle

```text
Open for extension
Closed for modification
```

---

## Q5. Difference Between Factory and Strategy?

### Factory

```text
Creates Objects
```

### Strategy

```text
Changes Behavior
```

Memory Trick:

```text
Factory -> Object Creation

Strategy -> Algorithm Selection
```

---

## Q6. Difference Between Factory and Builder?

### Factory

Creates object in one step.

```text
Car car = Factory.CreateCar();
```

### Builder

Creates object step by step.

```text
Car car =
    new CarBuilder()
        .SetEngine()
        .SetColor()
        .Build();
```

---

## Q7. What is Factory Method?

### Answer

A variation of Factory Pattern where subclasses decide which object to create.

---

## Q8. Give Real-World Examples

* Database connections
* Payment gateways
* Cloud providers
* Notification services
* UI frameworks
* Logging systems

---

# Quick Revision (30 Seconds)

## Definition

Creates objects without exposing creation logic.

---

## Purpose

```text
Encapsulate Object Creation
```

---

## Components

```text
Product
Concrete Product
Factory
Client
```

---

## Benefits

```text
Loose Coupling
Open/Closed Principle
Centralized Creation Logic
Maintainability
```

---

## Memory Trick

```text
Factory = WHO creates object

Strategy = HOW work is done

Builder = HOW object is constructed
```

---

# One-Line Interview Answer

> Factory Pattern is a Creational Design Pattern that centralizes object creation and allows clients to work with abstractions instead of concrete classes, improving flexibility, maintainability, and adherence to SOLID principles.
