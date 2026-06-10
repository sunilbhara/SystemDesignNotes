# Single Responsibility Principle (SRP) - C#

## Definition

The **Single Responsibility Principle (SRP)** states:

> A class should have only one reason to change.

In simple words:

> A class should have only one responsibility or one job.

---

# ❌ Without SRP (Bad Design)

Suppose we have an Employee class.

```csharp
public class Employee
{
    public string Name { get; set; }
    public double Salary { get; set; }

    public void CalculateSalary()
    {
        Console.WriteLine("Calculating Salary...");
    }

    public void SaveToDatabase()
    {
        Console.WriteLine("Saving Employee...");
    }

    public void GenerateReport()
    {
        Console.WriteLine("Generating Report...");
    }
}
```

---

## Problems

This class has multiple responsibilities:

1. Employee Data Management
2. Salary Calculation
3. Database Operations
4. Report Generation

### Reasons to Change

The class changes if:

* Salary calculation changes
* Database changes
* Report format changes

This violates SRP.

---

# ✅ With SRP (Good Design)

Separate responsibilities into different classes.

---

## Employee Entity

```csharp
public class Employee
{
    public string Name { get; set; }
    public double Salary { get; set; }
}
```

Responsibility:

> Store employee information.

---

## Salary Service

```csharp
public class SalaryCalculator
{
    public double Calculate(Employee employee)
    {
        return employee.Salary;
    }
}
```

Responsibility:

> Salary calculation.

---

## Repository

```csharp
public class EmployeeRepository
{
    public void Save(Employee employee)
    {
        Console.WriteLine("Saving Employee...");
    }
}
```

Responsibility:

> Database operations.

---

## Report Service

```csharp
public class EmployeeReportGenerator
{
    public void Generate(Employee employee)
    {
        Console.WriteLine("Generating Report...");
    }
}
```

Responsibility:

> Report generation.

---

## Usage

```csharp
Employee employee = new Employee
{
    Name = "Sunil",
    Salary = 50000
};

SalaryCalculator calculator =
    new SalaryCalculator();

EmployeeRepository repository =
    new EmployeeRepository();

EmployeeReportGenerator reportGenerator =
    new EmployeeReportGenerator();

double salary =
    calculator.Calculate(employee);

repository.Save(employee);

reportGenerator.Generate(employee);
```

---

# Benefits of SRP

* Easier maintenance
* Easier testing
* Better readability
* Lower coupling
* Better scalability

---

# Real World Example

## Bad Design

```csharp
public class UserService
{
    public void RegisterUser() {}

    public void SendEmail() {}

    public void SaveUser() {}

    public void GeneratePdfReport() {}
}
```

Too many responsibilities.

---

## Good Design

```text
UserService
EmailService
UserRepository
ReportService
```

Each class has a single responsibility.

---

# Interview Questions & Answers

## Q1. What is SRP?

**Answer:**

> SRP states that a class should have only one reason to change, meaning it should have only one responsibility.

---

## Q2. What is meant by "One Reason to Change"?

**Answer:**

A class should change only because of changes related to its responsibility.

---

## Q3. What are the benefits of SRP?

**Answer:**

* Maintainability
* Readability
* Testability
* Reduced Coupling
* Better Reusability

---

## Q4. Does one class mean one method?

**Answer:**

No.

A class can have multiple methods as long as they belong to the same responsibility.

---

## Q5. Is SRP the most important SOLID principle?

**Answer:**

Many developers consider SRP the foundation because violating SRP often leads to violations of other SOLID principles.

---

# Common Interview Mistake

❌ Wrong Answer:

> A class should contain only one method.

Incorrect.

✅ Correct Answer:

> A class can contain multiple methods, but all methods should contribute to one responsibility.

---

# Quick Revision (30 Seconds)

* SRP = Single Responsibility Principle
* One class → One responsibility
* One class → One reason to change
* Separate business logic, database logic, reporting logic
* Improves maintainability and testing

---

# C# Interview One-Liner

> The Single Responsibility Principle states that a class should have only one reason to change. In C#, we achieve this by separating business logic, persistence logic, and presentation logic into different classes.
