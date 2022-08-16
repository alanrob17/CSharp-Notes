# Object Oriented Principles - Part 1

## Static Classes

We are going to start using static classes for this module.

We create a ``Program.cs`` class in a Console application.

```csharp
namespace ConsoleUI
{
    public class Program
    {
        public static void Main(string[] args)
        {
            string firstName = RequestData.GetName("What is your first name: ");

            UserMessages.ApplicationStartMessage(firstName);

            double x = RequestData.GetADouble("Please enter your first number: ");
            double y = RequestData.GetADouble("Please enter your second number: ");

            double result = CalculateData.Add(x, y);

            UserMessages.PrintResultMessage($"The sum of { x } and { y } is { result }");

            Console.ReadLine();
        }
    }
}
```

The first thing we need to do is make the class have a ``public`` access modifier. Do the same with the ``Main`` method.

We have a number of methods being called in the ``Main`` method and the temptation is to write all these methods in the ``Program`` class.

This is a mistake! We should make separate classes for each different type of method we are creating.

The first thing we are doing is getting a user name. Build a class to to get data. Name it ``RequestData``.

Next we want to print messages so create another class for this named ``UserMessages``.

We are doing calculations on double number values the user enters so we could create a ``Calculate`` class.

Rather than print the output in the ``Program`` class we should do this in the UserMessages class with a method named ``PrintResultMessage()``.

Separating your work out into different classes is based on the **Single Responsibility Principle**. It makes you code easier to find and work with and makes it easier for another programmer to figure out what is going on in the code.

Methods should also work with the **Single Responsibility Principle** meaning that a method only does one thing.

### UserMessages class

```csharp
namespace ConsoleUI
{
    public static class UserMessages
    {
        public static void ApplicationStartMessage(string firstName)
        {
            Console.Clear();
            Console.WriteLine("Welcome to the Static Class Demo App");

            int hourOfDay = DateTime.Now.Hour;

            if (hourOfDay < 12)
            {
                Console.WriteLine($"Good morning { firstName }!");
            }
            else if (hourOfDay < 19)
            {
                Console.WriteLine($"Good afternoon { firstName }!");
            }
            else
            {
                Console.WriteLine($"Good evening { firstName }!");
            }
        }

        public static void PrintResultMessage(string message)
        {
            Console.WriteLine(message);
            Console.WriteLine();
            Console.WriteLine("Thank you for using our app to calculate for you.");
        }
    }
}
```

Contains all of the user messaging.

### RequestData class

```csharp
namespace ConsoleUI
{
    public static class RequestData
    {
        public static string GetName(string message)
        {
            Console.Write(message);
            string output = Console.ReadLine();
            return output;
        }

        public static double GetADouble(string message)
        {
            Console.Write(message);
            string numberText = Console.ReadLine();
            double output;

            bool isDouble = double.TryParse(numberText, out output);

            while (isDouble == false)
            {
                Console.WriteLine("That was not a valid number. Please try again.");
                Console.Write(message);
                numberText = Console.ReadLine();

                isDouble = double.TryParse(numberText, out output);
            }

            return output;
        }
    }
}
```

Contains all the data entry code.

### CalculateData class

```csharp
namespace ConsoleUI
{
    public static class CalculateData
    {
        public static double Add(double x, double y)
        {
            double output = x + y;

            return output;
        }
    }
}
```

Contains all the code for calculating values.

Note, seeing as we are working with static methods we can also make the classes static as well. If we forget to make a particular method a static method this will cause an error. This makes sure that we are using static methods.

There is no reason why we should make all of our classes ``static`` and we can happily work with ``static`` and ``instantiated`` classes in the same project.

The first thing that we see when we look at our code is that there are a number of files in our solution. That isn't a problem and when we work on large projects there could be dozens (or hundreds) of files.

If you name your classes properly it should be easy to find the methods you need to work with. In fact, some of this code could be reused in other projects with minimal changes.

## Instantiated Classes

### Important note

Static classes have an issue. If you store static data in the classes it lasts for the lifetime of the program and everyone has access to that same data. If you store a lot of data it could be a problem.

Instantiated classes are different and they can solve a big problem.

I can save details for a person with.

```csharp
    string firstName;
    string lastName;
    string email;
```

This is okay for one person but what if I have a number of people?

This is where class instantiation comes into play.

We will create a new class and call it ``PersonModel``. We will **NOT** make it static.

```csharp
namespace ConsoleUI
{
    public class PersonModel
    {
        public string FirstName;
        public string LastName;
        public string Email;
    }
}
```

This can be viewed as a blueprint, a plan of your object but you can't use it.

I can't access the ``PersonModel`` properties in ``Main()``.

![No access to fields](assets/images/no-access.jpg "No access to fields")

To get around this I have to create an instance of this object (blueprint). i.e. I have to instantiate the object.

![Create an instance of an object](assets/images/instance-of-object.jpg "Create an instance of an object")

This allows us to access the fields of this object.

```csharp
namespace ConsoleUI
{
    public class Program
    {
        public static void Main(string[] args)
        {
            PersonModel person = new PersonModel(); // instantiate PersonModel

            person.FirstName = "Alan";
            person.LastName = "Robson";
            person.Email = "alan@alan.com";
        }
    }
}
```

Here you are building a house from your blueprint. ``PersonModel`` is the blueprint and once you instantiate it with ``person`` it becomes a real house and you can add values to it.

You can also do this

```csharp
    PersonModel secondPerson = new PersonModel();

    secondPerson.FirstName = "James";
    // ...
```

So basically we have built two different houses with the same blueprint.

This is how we store multiple bits of data together and have multiples of them. Obviously this isn't very efficient.

What we can do is make a ``List`` of type ``PersonModel``.

```csharp
    List<PersonModel> people = new List<PersonModel>();
```

So our ``List`` isn't of one of the primitive types but of a type that we created, ``PersonModel``.

I can add a person to the people list with this code.

```csharp
    List<PersonModel> people = new List<PersonModel>();

    PersonModel person = new PersonModel();

    person.FirstName = "Alan";
    person.LastName = "Robson";
    person.Email = "alan@alan.com";

    people.Add(person);
```

We can add on to this to add another ``person``.

```csharp
    List<PersonModel> people = new List<PersonModel>();

    PersonModel person = new PersonModel();

    person.FirstName = "Alan";
    person.LastName = "Robson";
    person.Email = "alan@alan.com";

    people.Add(person);

    person = new PersonModel();

    person.FirstName = "Charley";

    people.Add(person);
```

Because we have created a new ``PersonModel`` (``= new PersonModel();``) we can add new details to it and save it to the ``people`` list.

We can also view this data.

```csharp
    foreach (var p in people)
    {
        Console.WriteLine($"{p.FirstName}");
    }
```

Returns.

> Alan
> Charley

We can add as many people as we want to the ``people`` list.

Now we are going to modify our ``PersonModel`` class because this is not the way we want to create our objects. They don't work as well as they should and we can improve on this by changing the **fields** to **properties**.

We do this by using auto properties.

```csharp
    public class PersonModel
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
    }
```

``get;`` gets our property and ``set;`` sets our property.

Now we are going to change our data entry code to add any number of new people into our list.

```csharp
    public static void Main(string[] args)
    {
        List<PersonModel> people = new List<PersonModel>();
        string firstName = String.Empty    
        
        do
        {
            Console.Write("What is your first name (or type exit to stop): ");
            firstName = Console.ReadLine() 
        
            Console.Write("What is your last name: ");
        
            string lastName = Console.ReadLine()   
        
            if (firstName.ToLower() != "exit")
            {
                PersonModel person = new PersonModel();
                person.FirstName = firstName;
                person.LastName = lastName;
                people.Add(person);
            }
        } while (firstName.ToLower() != "exit")

        foreach (PersonModel p in people)
        {
            Console.WriteLine($"{p.FirstName} {p.LastName}");
        }
    }
```

Returns.

> What is your first name (or type exit to stop): Alan
> What is your last name: Robson
> What is your first name (or type exit to stop): Charley
> What is your last name: Robson
> What is your first name (or type exit to stop): James
> What is your last name: Robson
> What is your first name (or type exit to stop): exit
> What is your last name: exit
> Alan Robson
> Charley Robson
> James Robson

**Important Note** When you create a new person variable it lives in the context where it was created and when it hits the ending curly brace and it drops out of context and that person gets "blown away" and is erased.

``people`` has the context of the ``Main`` method so it still exists and you can continue on and create another ``person`` object to add to the ``people`` list.

This is the power of instantiation and we will use it a lot.

Class instances are where we store data and we can control how long we want it to live making it more efficient than ``static`` data.

Now we are going to refactor our code. 

First, modify the ``PersonModel`` class.

```csharp
    public class PersonModel
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
        public bool HasBeenGreeted { get; set; }
    }
```

We have added a boolean property named ``HasBeenGreeted``.

Now we will take out the message and put it into separate static class.

```csharp
    public static class ProcessPerson
    {
        public static void GreetPerson(PersonModel person)
        {
            Console.WriteLine($"Hello {person.FirstName} {person.LastName}");
            person.HasBeenGreeted = true;
        }
    }
```

Modify the ``Main()`` method.

```csharp
    foreach (PersonModel p in people)
    {
        ProcessPerson.GreetPerson(p);
    }

    Console.ReadLine();
```

**Note:** our ``GreetPerson(PersonModel person)`` method in the ``ProcessPerson`` class is **void** meaning it is not returning any data.

So what happens when we set ``person.HasBeenGreeted = true;``?

Remember when we are passing in ``person`` to ``GreetPerson()`` we are NOT passing in a copy. We are sending in the location of the variable and when you change the value of the property ``HasBeenGreeted`` you are adding it to its address so then it can be seen in ``Main``.

So if you add a breakpoint on the ``foreach`` line and the ``Console.ReadLine();`` you can see that ``HasBeenGreeted`` has been changed.

When you finally hit ``Console.ReadLine()`` this is what you see in the ``Locals`` window.

![Locals window show property has been changed](assets/images/locals-window.jpg "Locals window show property has been changed")

You could make a mistake and change ``void`` to ``List<PersonModel>`` and return the object but you don't need to do that because you aren't sending in a copy, you are sending in the real object.

Primitive types are passed by ``value`` and class objects are passed by ``reference``. So when you send an object to another function you can change its values.

> **Tip** 
> 
> In general instantiated classes store data and static classes are for stateless processing.

### Final code

#### Program.cs

```csharp
public static void Main(string[] args)
{
    List<PersonModel> people = new List<PersonModel>();
    string firstName = String.Empty;

    do
    {
        Console.Write("What is your first name (or type exit to stop): ");
        firstName = Console.ReadLine();

        Console.Write("What is your last name: ");
        string lastName = Console.ReadLine();

        if (firstName.ToLower() != "exit")
        {
            PersonModel person = new PersonModel();
            person.FirstName = firstName;
            person.LastName = lastName;
            people.Add(person);
        }
    } while (firstName.ToLower() != "exit");

    foreach (PersonModel p in people)
    {
        ProcessPerson.GreetPerson(p);
    }

    Console.ReadLine();
}
```

#### PersonModel.cs

```csharp
    public class PersonModel
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
        public bool HasBeenGreeted { get; set; }
    }
```

#### ProcessPerson.cs

```csharp
    public static class ProcessPerson
    {
        public static void GreetPerson(PersonModel person)
        {
            Console.WriteLine($"Hello {person.FirstName} {person.LastName}");
            person.HasBeenGreeted = true;
        }
    }
```
