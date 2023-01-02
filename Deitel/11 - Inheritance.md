# Object-Oriented Programming: Inheritance

## Objectives

* Understand how inheritance promotes software reusability.

* Create a derived class that inherits attributes and behaviors from a base class.
 
* Override base-class methods in derived classes.
 
* Use access modifier protected to give derived-class methods access to base-class members.
 
* Access base-class members with base.
 
* Understand how constructors are used in inheritance hierarchies.
 
* See an overview of the methods of class object, the direct or indirect base class of all classes.

## Introduction

Inheritance, a form of software reuse in which a new class is created by absorbing an existing class’s members and enhancing them with new or modified capabilities. Inheritance lets you save time during app development by reusing proven, high-performance and debugged high-quality software. This also increases the likelihood that a system will be implemented effectively.

The existing class from which a new class inherits members is called the ``base class``, and the new class is the ``derived class``. Each derived class can become the base class for future derived classes. A derived class normally adds its own fields, properties and methods. Therefore, it’s more specific than its base class and represents a more specialized group of objects. Typically, the derived class exhibits the behaviors of its base class and additional ones that are specific to itself.

The class hierarchy begins with class ``object`` - a C# keyword that’s an alias for ``System.Object`` in the Framework Class Library. Every class directly or indirectly extends (or “inherits from”) object.

C# supports only single inheritance.

``Is-a`` represents **inheritance**. In an ``is-a`` relationship, an object of a derived class also can be treated as an object of its base class. For example, a car ``is a`` vehicle, and a truck ``is a`` vehicle. 

New classes can inherit from classes in class libraries.

### Base Classes and Derived Classes

**Base classes tend to be more general, and derived classes tend to be more specific**.

One base class can have many derived classes, the set of objects represented by a base class is typically larger than the set of objects represented by any of its derived classes. For example, the base class Vehicle represents all vehicles-cars, trucks, boats, bicycles and so on. By contrast, derived class Car represents a smaller, more specific subset of vehicles.

| Base class      | Derived classes                                   |
|-----------------|---------------------------------------------------|
| Student         | GraduateStudent, UndergraduateStudent             |
| Shape           | Circle, Triangle, Rectangle                       |
| Loan            | CarLoan, HomeImprovementLoan, MortgageLoan        |
| Employee        | Faculty, Staff, HourlyWorker, CommissionWorker    |
| Space           | Object Star, Moon, Planet, FlyingSaucer           |
| BankAccount     | CheckingAccount, SavingsAccount                   |

A derived class can **customize** methods it inherits from its base class. In such cases, the derived class can **override** (redefine) the base-class method with an appropriate implementation.

Inheritance relationships form treelike hierarchical structures.

![Community members](../assets/images/community.jpg "Community members")

Each arrow with a hollow triangular arrowhead in the hierarchy diagram represents an ``is-a`` relationship. As we follow the arrows, we can state, for instance, that “an Employee ``is a`` CommunityMember” and “a Teacher ``is a`` Faculty member.” 

CommunityMember is the direct base class of Employee, Student and Alumnus and is an indirect base class of all the other classes in the diagram. Starting from the bottom, you can follow the arrows and apply the ``is-a`` relationship up to the topmost base class. For example, an Administrator is a Faculty member, is an Employee and is a CommunityMember.

It’s possible to treat base-class objects and derived-class objects similarly - their commonalities are expressed in the base class’s members. Objects of all classes that extend a common base class can be treated as objects of that base class - such objects have an ``is-a`` relationship with the base class. 

However, base-class objects cannot be treated as objects of their derived classes. For example, all cars are vehicles, but not all vehicles are cars (other vehicles could be trucks, planes, bicycles, etc.). 

### protected Members

A class’s **public** members are accessible wherever the app has a reference to an object of that class or one of its derived classes. 

A class’s **private** members are accessible only within the class itself. A base class’s private members are inherited by its derived classes, but are not directly accessible by derived-class methods and properties. 

Another access modifier is **protected**. Using **protected** access offers an intermediate level of access between public and private. A base class’s protected members **can be** accessed by members of that base class and by members of its derived classes, but not by clients of the class.

All **non-private** base-class members retain their original access modifier when they become members of the derived class - public members of the base class become public members of the derived class, and protected members of the base class become protected members of the derived class.

Derived-class methods can refer to public and protected members inherited from the base class simply by using the member names. When a derived-class method overrides a base-class method, the base-class version can be accessed from the derived class by preceding the base-class method name with the keyword ``base`` and the member access (.) operator.

Example: BasePlusCommissionEmployee inherits from CommissionEmployee.

```csharp
public BasePlusCommissionEmployee(string firstName, string lastName, string socialSecurityNumber, decimal grossSales,
      decimal commissionRate, decimal baseSalary) : base(firstName, lastName, socialSecurityNumber, grossSales, commissionRate)
{
   BaseSalary = baseSalary;
}
```

### Relationship between Base Classes and Derived Classes

Now, we use an inheritance hierarchy containing types of employees in a company’s payroll app to discuss the relationship between a base class and a derived class. In this company:

* **commission employees** - who will be represented as objects of a base class - are paid a percentage of their sales, while
* **base-salaried commission employees** - who will be represented as objects of a derived class receive a base salary plus a percentage of their sales.

The example shows that if base class CommissionEmployee’s instance variables are declared as **protected**, a BasePlusCommissionEmployee class that inherits from CommissionEmployee can access that data directly.

#### CommissionEmployee.cs

```csharp
// CommissionEmployee class represents a commission employee.
using System;

public class CommissionEmployee : object
{
   public string FirstName { get; }
   public string LastName { get; }
   public string SocialSecurityNumber { get; }
   protected decimal grossSales; 
   protected decimal commissionRate;

   // five-parameter constructor
   public CommissionEmployee(string firstName, string lastName, string socialSecurityNumber, decimal grossSales, decimal commissionRate)
   {
      FirstName = firstName;
      LastName = lastName;
      SocialSecurityNumber = socialSecurityNumber;
      GrossSales = grossSales;
      CommissionRate = commissionRate;
   }

   public decimal GrossSales
   {
      get
      {
         return grossSales;
      }
      set
      {
         if (value < 0) 
         {
            throw new ArgumentOutOfRangeException(nameof(value), value, $"{nameof(GrossSales)} must be >= 0");
         }

         grossSales = value;
      }
   }

   public decimal CommissionRate
   {
      get
      {
         return commissionRate;
      }
      set
      {
         if (value <= 0 || value >= 1)
         {
            throw new ArgumentOutOfRangeException(nameof(value), value, $"{nameof(CommissionRate)} must be > 0 and < 1");
         }

         commissionRate = value;
      }
   }

   public virtual decimal Earnings() => commissionRate * grossSales;

   public override string ToString() =>
      $"commission employee: {FirstName} {LastName}\n" +
      $"social security number: {SocialSecurityNumber}\n" +
      $"gross sales: {grossSales:C}\n" +
      $"commission rate: {commissionRate:F2}";
} 
```

**Note:** *grossSales* and *commissionRate* are protected which means that they can be used by derived classes.

#### BasePlusCommissionEmployee

``BasePlusCommissionEmployee`` inherits from ``CommissionEmployee`` and has access to CommissionEmployee's **protected** members.

```csharp
using System;

public class BasePlusCommissionEmployee : CommissionEmployee
{
   private decimal baseSalary; // base salary per week

   // six-parameter derived-class constructor
   // with call to base class CommissionEmployee constructor
   public BasePlusCommissionEmployee(string firstName, string lastName,
      string socialSecurityNumber, decimal grossSales,
      decimal commissionRate, decimal baseSalary)
      : base(firstName, lastName, socialSecurityNumber,
           grossSales, commissionRate)
   {
      BaseSalary = baseSalary;
   }

   public decimal BaseSalary
   {
      get
      {
         return baseSalary;
      }
      set
      {
         if (value < 0) // validation
         {
            throw new ArgumentOutOfRangeException(nameof(value), value, $"{nameof(BaseSalary)} must be >= 0");
         }

         baseSalary = value;
      }
   }

   public override decimal Earnings() => baseSalary + (commissionRate * grossSales);

   public override string ToString() =>
      $"base-salaried commission employee: {FirstName} {LastName}\n" +
      $"social security number: {SocialSecurityNumber}\n" +
      $"gross sales: {grossSales:C}\n" +
      $"commission rate: {commissionRate:F2}\n" +
      $"base salary: {baseSalary}";
} 
```

**Note:** we are overriding the base classes *Earnings* property so that we can allow for this employees bas salary. Same with the ``ToString()`` method.

#### BasePlusCommissionEmployeeTest.cs

Testing class BasePlusCommissionEmployee.

```csharp
using System;

public class BasePlusCommissionEmployeeTest
{
   static void Main()
   {
      // instantiate BasePlusCommissionEmployee object
      var employee = new BasePlusCommissionEmployee("Bob", "Lewis", "333-33-3333", 5000.00M, .04M, 300.00M);

      // display BasePlusCommissionEmployee's data
      Console.WriteLine("Employee information obtained by properties and methods: \n");
      Console.WriteLine($"First name is {employee.FirstName}");
      Console.WriteLine($"Last name is {employee.LastName}");
      Console.WriteLine($"Social security number is {employee.SocialSecurityNumber}");
      Console.WriteLine($"Gross sales are {employee.GrossSales:C}");
      Console.WriteLine($"Commission rate is {employee.CommissionRate:F2}");
      Console.WriteLine($"Earnings are {employee.Earnings():C}");
      Console.WriteLine($"Base salary is {employee.BaseSalary:C}");

      employee.BaseSalary = 1000.00M; // set base salary

      Console.WriteLine("\nUpdated employee information obtained by ToString:\n");
      Console.WriteLine(employee);
      Console.WriteLine($"earnings: {employee.Earnings():C}");
   }
}
```

Returns.

> Employee information obtained by properties and methods:
> 
> First name is Bob
> Last name is Lewis
> Social security number is 333-33-3333
> Gross sales are $5,000.00
> Commission rate is 0.04
> Earnings are $500.00
> Base salary is $300.00
> 
> Updated employee information obtained by ToString:
> 
> base-salaried commission employee: Bob Lewis
> social security number: 333-33-3333
> gross sales: $5,000.00
> commission rate: 0.04
> base salary: 1000.00
> earnings: $1,200.00

Points to consider:

We literally copied the code from class ``CommissionEmployee`` and pasted it into class ``BasePlusCommissionEmployee``.

E.g. firstName, lastName, socialSecurityNumber, grossSales and commissionRate

We then modified class ``BasePlusCommissionEmployee`` to include a **base salary** and methods and properties that manipulate the base salary. 

This “copy-and-paste” approach is error prone and time consuming. Worse yet, it can spread many physical copies of the same code throughout a system, creating a code-maintenance nightmare. Is there a way to “absorb” the members of one class in a way that makes them part of other classes without copying code?

### Creating a CommissionEmployee–BasePlusCommissionEmployee Inheritance Hierarchy

Now we declare class ``BasePlusCommissionEmployee``, which extends (inherits from) class ``CommissionEmployee``. A ``BasePlusCommissionEmployee`` object is a ``CommissionEmployee`` (because inheritance passes on the capabilities of class ``CommissionEmployee``), but class ``BasePlusCommissionEmployee`` also has instance variable ``baseSalary``. The colon (``:``) of the class declaration indicates inheritance. As a derived class, ``BasePlusCommissionEmployee`` inherits `CommissionEmployee`’s members and can access only those that are **non-private**. ``CommissionEmployee``’s constructor **is not inherited**. Thus, the public services of ``BasePlusCommissionEmployee`` include its constructor, public methods and properties inherited from class ``CommissionEmployee``, property ``BaseSalary``, method ``Earnings`` and method ``ToString``. 

Again, ``BasePlusCommissionEmployee``’s constructor must explicitly call ``CommissionEmployee``’s constructor, because ``CommissionEmployee`` does not provide a parameterless constructor that could be invoked implicitly.

Next version of our code.

### CommissionEmployee.cs

We are declaring private variables again.

```csharp
using System;

public class CommissionEmployee
{
   public string FirstName { get; }
   public string LastName { get; }
   public string SocialSecurityNumber { get; }
   private decimal grossSales;
   private decimal commissionRate;

   // five-parameter constructor
   public CommissionEmployee(string firstName, string lastName, string socialSecurityNumber, decimal grossSales, decimal commissionRate)
   {
      FirstName = firstName;
      LastName = lastName;
      SocialSecurityNumber = socialSecurityNumber;
      GrossSales = grossSales;
      CommissionRate = commissionRate;
   }

   public decimal GrossSales
   {
      get
      {
         return grossSales;
      }
      set
      {
         if (value < 0)
         {
            throw new ArgumentOutOfRangeException(nameof(value), value, $"{nameof(GrossSales)} must be >= 0");
         }

         grossSales = value;
      }
   }

   public decimal CommissionRate
   {
      get
      {
         return commissionRate;
      }
      set
      {
         if (value <= 0 || value >= 1) // validation
         {
            throw new ArgumentOutOfRangeException(nameof(value),
               value, $"{nameof(CommissionRate)} must be > 0 and < 1");
         }

         commissionRate = value;
      }
   }

   public virtual decimal Earnings() => CommissionRate * GrossSales;

   // return string representation of CommissionEmployee object
   public override string ToString() =>
      $"commission employee: {FirstName} {LastName}\n" +
      $"social security number: {SocialSecurityNumber}\n" +
      $"gross sales: {GrossSales:C}\n" +
      $"commission rate: {CommissionRate:F2}";
} 
```

**Note:** we have gone back to private properties, ``grossSales`` and ``commissionRate``.

#### BasePlusCommissionEmployee.cs

``BasePlusCommissionEmployee`` inherits from ``CommissionEmployee`` and has controlled access to ``CommissionEmployee``'s private data via its public properties.

```csharp
using System;

public class BasePlusCommissionEmployee : CommissionEmployee
{
   private decimal baseSalary; // base salary per week

   // six-parameter derived-class constructor
   // with call to base class CommissionEmployee constructor
   public BasePlusCommissionEmployee(string firstName, string lastName,
      string socialSecurityNumber, decimal grossSales,
      decimal commissionRate, decimal baseSalary)
      : base(firstName, lastName, socialSecurityNumber,
           grossSales, commissionRate)
   {
      BaseSalary = baseSalary;
   }

   public decimal BaseSalary
   {
      get
      {
         return baseSalary;
      }
      set
      {
         if (value < 0)
         {
            throw new ArgumentOutOfRangeException(nameof(value), value, $"{nameof(BaseSalary)} must be >= 0");
         }

         baseSalary = value;
      }
   }

   public override decimal Earnings() => BaseSalary + base.Earnings();

   public override string ToString() => $"base-salaried {base.ToString()}\nbase salary: {BaseSalary:C}";
}
```

#### A Derived Class’s Constructor Must Call Its Base Class’s Constructor

Each derived-class constructor must implicitly or explicitly call its base-class constructor to ensure that the instance variables inherited from the base class are initialized properly.

``BasePlusCommissionEmployee``’s six-parameter constructor explicitly calls class ``CommissionEmployee``’s five-parameter constructor to initialize the ``CommissionEmployee`` portion of a ``BasePlusCommissionEmployee`` object—that is, the ``FirstName``, ``LastName``, ``SocialSecurityNumber``, ``GrossSales`` and ``CommissionRate``.

After the ``:`` it invokes ``CommissionEmployee``’s constructor via a constructor initializer with keyword **base** to invoke the base-class constructor, passing arguments to initialize the base class’s corresponding properties that were inherited into the derived-class object. If ``BasePlusCommissionEmployee``’s constructor did not invoke ``CommissionEmployee``’s constructor explicitly, C# would attempt to invoke class ``CommissionEmployee``’s parameterless or default constructor implicitly. ``CommissionEmployee`` does not have such a constructor, so the compiler would issue an error.

#### BasePlusCommissionEmployee Method earnings

We declare method ``Earnings`` using keyword **override** to override the ``CommissionEmployee``'s ``Earnings`` method, as we did with method ``ToString`` in previous examples. We would get an error saying that we cannot override the base classes ``Earnings`` method because it was not explicitly “marked virtual, abstract, or override.” The **virtual** and **abstract** keywords indicate that a base-class method can be overridden in derived classes. 

The override modifier declares that a derived-class method overrides a virtual or abstract base-class method. This modifier also implicitly declares the derived-class method virtual and allows it to be overridden in derived classes further down the inheritance hierarchy. Adding the keyword **virtual** to method ``Earnings``’ declaration eliminates the compilation errors.

The compiler generates additional errors because base class ``CommissionEmployee``’s instance variables ``commissionRate`` and ``grossSales`` are private-derived class ``BasePlusCommissionEmployee``’s methods are not allowed to access the base class’s private members. The compiler also issues errors in method ``ToString`` for the same reason. The errors in ``BasePlusCommissionEmployee`` can be prevented by using the public properties inherited from class ``CommissionEmployee``.

For example, you could have invoked the get accessors of properties ``CommissionRate`` and ``GrossSales`` to access ``CommissionEmployee``’s private instance variables ``commissionRate`` and ``grossSales``, respectively.

#### CommissionEmployee–BasePlusCommissionEmployee

Inheritance Hierarchy Using protected Instance Variables To enable class BasePlusCommissionEmployee to directly access base-class instance variables ``grossSales`` and ``commissionRate``, we can declare those members as protected in the base class. As we discussed, a base class’s protected members are accessible to all derived classes of that base class. Class ``CommissionEmployee`` in this example is a modification of the earlier version from that declares its instance variables ``grossSales`` and ``commissionRate`` as rather than private. We also declare the Earnings method virtual so that BasePlusCommissionEmployee can override the method. The rest of class CommissionEmployee in this example is identical to Fig. 11.4. The complete source code for class CommissionEmployee
is included in this example’s project.

#### Class BasePlusCommissionEmployee

Class ``BasePlusCommissionEmployee`` in this example extends the version of class ``CommissionEmployee`` with protected instance variables ``grossSales`` and ``commissionRate``. 

Each ``BasePlusCommissionEmployee`` object contains these ``CommissionEmployee`` instance variables, which are inherited into ``BasePlusCommissionEmployee`` as protected members. As a result, directly accessing instance variables grossSales and commissionRate no longer generates compilation errors in methods ``Earnings`` and ``ToString``. If another class extends ``BasePlusCommissionEmployee``, the new derived class also inherits ``grossSales`` and ``commissionRate`` as protected members.

Class ``BasePlusCommissionEmployee`` does not inherit class ``CommissionEmployee``’s constructor. However, class ``BasePlusCommissionEmployee``’s constructor (lines 12–19) calls class ``CommissionEmployee``’s constructor with a constructor initializer. Again ``BasePlusCommissionEmployee``’s constructor must explicitly call ``CommissionEmployee``’s constructor, because ``CommissionEmployee`` does not provide a parameterless constructor that could be invoked implicitly.

#### BasePlusCommissionEmployeeTest.cs

```csharp
using System;

public class BasePlusCommissionEmployeeTest
{
   static void Main()
   {
      var employee = new BasePlusCommissionEmployee("Bob", "Lewis", "333-33-3333", 5000.00M, .04M, 300.00M);

      Console.WriteLine("Employee information obtained by properties and methods: \n");
      Console.WriteLine($"First name is {employee.FirstName}");
      Console.WriteLine($"Last name is {employee.LastName}");
      Console.WriteLine($"Social security number is {employee.SocialSecurityNumber}");
      Console.WriteLine($"Gross sales are {employee.GrossSales:C}");
      Console.WriteLine($"Commission rate is {employee.CommissionRate:F2}");
      Console.WriteLine($"Earnings are {employee.Earnings():C}");
      Console.WriteLine($"Base salary is {employee.BaseSalary:C}");

      employee.BaseSalary = 1000.00M;

      Console.WriteLine("\nUpdated employee information obtained by ToString:\n");
      Console.WriteLine(employee);
      Console.WriteLine($"earnings: {employee.Earnings():C}");
   }
}
```
