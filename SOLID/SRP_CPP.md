# Single Responsibility Principle (SRP) - C++

## Definition

The **Single Responsibility Principle (SRP)** states:

> A class should have only one reason to change.

In simple words:

> A class should perform only one job.

---

# ❌ Without SRP (Bad Design)

Suppose we have an Employee class.

```cpp
#include <iostream>
using namespace std;

class Employee
{
public:
    string name;
    double salary;

    void calculateSalary()
    {
        cout << "Calculating Salary..." << endl;
    }

    void saveToDatabase()
    {
        cout << "Saving Employee..." << endl;
    }

    void generateReport()
    {
        cout << "Generating Report..." << endl;
    }
};
```

---

## Problems

This class has multiple responsibilities:

1. Employee Data
2. Salary Calculation
3. Database Operations
4. Report Generation

### Reasons to Change

The class changes if:

* Salary rules change
* Database changes
* Report format changes

This violates SRP.

---

# ✅ With SRP (Good Design)

Separate responsibilities.

---

## Employee Entity

```cpp
class Employee
{
public:
    string name;
    double salary;
};
```

Responsibility:

> Store employee information.

---

## Salary Calculator

```cpp
class SalaryCalculator
{
public:
    double calculate(Employee& employee)
    {
        return employee.salary;
    }
};
```

Responsibility:

> Salary calculation.

---

## Repository

```cpp
class EmployeeRepository
{
public:
    void save(Employee& employee)
    {
        cout << "Saving Employee..." << endl;
    }
};
```

Responsibility:

> Database operations.

---

## Report Generator

```cpp
class EmployeeReportGenerator
{
public:
    void generate(Employee& employee)
    {
        cout << "Generating Report..." << endl;
    }
};
```

Responsibility:

> Report generation.

---

## Usage

```cpp
int main()
{
    Employee employee;

    employee.name = "Sunil";
    employee.salary = 50000;

    SalaryCalculator calculator;
    EmployeeRepository repository;
    EmployeeReportGenerator reportGenerator;

    calculator.calculate(employee);
    repository.save(employee);
    reportGenerator.generate(employee);
}
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

```cpp
class UserService
{
public:
    void registerUser() {}

    void sendEmail() {}

    void saveUser() {}

    void generatePdfReport() {}
};
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

Each class performs one job.

---

# Interview Questions & Answers

## Q1. What is SRP?

**Answer:**

> SRP states that a class should have only one reason to change.

---

## Q2. What is meant by one reason to change?

**Answer:**

Only changes related to its responsibility should affect the class.

---

## Q3. What are the benefits of SRP?

**Answer:**

* Better maintainability
* Better readability
* Better testing
* Reduced coupling
* Easier debugging

---

## Q4. Does SRP mean one method per class?

**Answer:**

No.

A class can contain multiple methods as long as all methods belong to the same responsibility.

---

## Q5. How does SRP improve large systems?

**Answer:**

Changes remain localized, reducing impact on unrelated modules.

---

# Common Interview Mistake

❌ Wrong Answer:

> SRP means one class should contain one method.

Incorrect.

✅ Correct Answer:

> SRP means one class should have one responsibility and one reason to change.

---

# Quick Revision (30 Seconds)

* SRP = Single Responsibility Principle
* One class → One responsibility
* One class → One reason to change
* Separate business logic, database logic, reporting logic
* Easier maintenance and testing

---

# C++ Interview One-Liner

> The Single Responsibility Principle states that a class should have only one reason to change. In C++, we achieve this by separating business logic, persistence logic, and reporting logic into different classes, making the system easier to maintain and extend.
