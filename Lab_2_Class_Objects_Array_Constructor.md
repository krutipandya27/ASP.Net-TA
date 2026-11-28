# Lab 2 – Class, Objects, Array of Objects & Constructor

**Course Code:** 2401CS521  
**Course Name:** Building RESTful APIs with ASP.NET Core  
**University:** Darshan Institute of Engineering & Technology, Darshan University  
**Academic Year:** 2026–27  
**Lab:** 2  

---

# 1. Learning Objectives

After completing this lab, you should be able to:

- Understand the purpose of classes and objects in C#.
- Identify data members and member functions in a class.
- Create objects and access their methods.
- Accept input from the user and display stored information.
- Understand constructors and parameterized constructors.
- Use `this` to refer to the current object's fields.
- Create and use arrays of objects to manage multiple records.
- Understand why loops are useful when handling many objects.

# 2. Introduction / Theory

## 2.1 What is Object-Oriented Programming (OOP)?

Object-Oriented Programming (OOP) is a way of writing programs by organizing code around **objects**. An object represents something that has information (data) and can perform actions (methods). For example, an Employee object can store an employee's name, department and salary, and can display those details.

OOP helps organize code, reuse code, and keep related data and functions together.

## 2.2 What is a Class?

A **class is a blueprint or template used to create objects**. It describes which data an object will hold and which operations it can perform. A class itself is a design; creating an object makes an instance of that design.


```csharp
class Faculty
{
    int id;
    string name;
    int age;
}
```

Here, `Faculty` is the class. `id`, `name`, and `age` are fields (data members) that describe a faculty member.

## 2.3 What is an Object?

An **object is an instance of a class**. We generally create an object with the `new` keyword.

```csharp
Faculty f = new Faculty();
```

- `Faculty` – the class/type name.
- `f` – the variable that refers to the created object.
- `new` – creates an instance of the class.
- `Faculty()` – calls the constructor (if available; otherwise C# supplies a parameterless constructor when appropriate).

A class is like a form/template; an object is one actual record made using that template.

## 2.4 Data Members / Fields

Data members are variables declared in a class. They store the information belonging to an object.

```csharp
int id;
string name;
double salary;
```

For example, an Employee object can have its own `id`, `name`, and `salary`. Instance fields belong to each object rather than being shared by all objects.

## 2.5 Member Functions / Methods

A method is a named block of code that performs a task. Methods allow us to keep related operations inside the class.

```csharp
void Display()
{
    Console.WriteLine(name);
}
```

`void` means that this method does not return a value. `Display()` is its name. The statements inside its braces run when the method is called.

## 2.6 Accessing a Member with the Dot Operator

The dot (`.`) operator is used to access members of an object, such as its public methods.

```csharp
f.DisplayDetails();
```

This calls `DisplayDetails()` using object `f`.

## 2.7 What is a Constructor?

A **constructor is a special member of a class that runs when an object is created**. It is commonly used to initialize the object's fields with starting values.

Rules to remember:

- A constructor's name must match its class name exactly.
- A constructor has no return type—not even `void`.
- It runs when an object is created with `new`.
- A class can have more than one constructor (constructor overloading), if their parameter lists differ.

### Parameterless Constructor

A constructor with no parameters is called a parameterless constructor.

```csharp
class Student
{
    public Student()
    {
        Console.WriteLine("Student object created");
    }
}
```

When `new Student()` is executed, the constructor runs automatically.

### Parameterized Constructor

A constructor that accepts parameters is called a parameterized constructor. It lets us supply starting values at the time we create the object.

```csharp
class Box
{
    int side;

    public Box(int s)
    {
        side = s;
    }
}
```

Create it like this:

```csharp
Box b = new Box(5);
```

The value `5` is passed to parameter `s`, and the constructor stores it in `side`.

## 2.8 The `this` Keyword

`this` refers to the current object. It is especially helpful when the constructor parameter has the same name as the class field.

```csharp
class Box
{
    int side;

    public Box(int side)
    {
        this.side = side;
    }
}
```

- `this.side` – the field that belongs to the current object.
- `side` – the parameter received by the constructor.

So `this.side = side;` means “put the parameter value into this object's field.”

## 2.9 Array of Objects

An array can hold multiple references to objects of the same class. This is useful when we need to store many employee, item, or patient records.

```csharp
Employee[] employees = new Employee[5];
```

This creates five array slots, but it does **not** create five Employee objects yet. Each slot initially contains `null` until an object is assigned.

```csharp
for (int i = 0; i < employees.Length; i++)
{
    employees[i] = new Employee();
}
```

This loop creates an Employee object in each slot. `Length` is the array size, and indexes start from `0` (so five elements have indexes `0` through `4`).

## 2.10 Why use a `for` loop with object arrays?

If we have five or ten objects, we don't need to write the same input/display statements repeatedly. A loop performs the task for each array position. The variable `i` tracks the current index.

---

# Part A

# Question 1 – Faculty Details using Class and Member Functions

## Question

Write a program to create a class named `Faculty` with ID, Name, Age, Weight and Height as data members and create member functions `GetFacultyDetails()` and `DisplayFacultyDetails()`.

## Understand the Question

We are making a class to represent one faculty member. It needs five fields:

| Field | What it stores | Type |
|---|---|---|
| `id` | Faculty identification number | `int` |
| `name` | Faculty name | `string` |
| `age` | Age | `int` |
| `weight` | Weight | `double` |
| `height` | Height | `double` |

The input method will read each value from the console and save it into the class fields. The display method will print the values saved in those fields.

## Program

```csharp
using System;

namespace Lab2
{
    internal class Faculty
    {
        int id;
        string name;
        int age;
        double weight;
        double height;

        public void GetFacultyDetails()
        {
            Console.Write("Enter Faculty ID: ");
            id = Convert.ToInt32(Console.ReadLine());

            Console.Write("Enter Faculty Name: ");
            name = Console.ReadLine();

            Console.Write("Enter Age: ");
            age = Convert.ToInt32(Console.ReadLine());

            Console.Write("Enter Weight: ");
            weight = Convert.ToDouble(Console.ReadLine());

            Console.Write("Enter Height: ");
            height = Convert.ToDouble(Console.ReadLine());
        }

        public void DisplayFacultyDetails()
        {
            Console.WriteLine("\n--- Faculty Details ---");
            Console.WriteLine("ID     : " + id);
            Console.WriteLine("Name   : " + name);
            Console.WriteLine("Age    : " + age);
            Console.WriteLine("Weight : " + weight);
            Console.WriteLine("Height : " + height);
        }
    }


    internal class Program
    {
        static void Main(string[] args)
        {
            Faculty f = new Faculty();
            f.GetFacultyDetails();
            f.DisplayFacultyDetails();
        }
    }
}
```

## Sample Output

```text
Enter Faculty ID: 101
Enter Faculty Name: Kruti
Enter Age: 25
Enter Weight: 55
Enter Height: 5.4

--- Faculty Details ---
ID     : 101
Name   : Kruti
Age    : 25
Weight : 55
Height : 5.4
```

## Code Explanation

- `int id;`, `string name;`, etc. declare fields to store faculty information.
- `GetFacultyDetails()` is used to collect input. The assignments use the fields directly, so the values remain stored in the object after the method finishes.
- `Console.ReadLine()` reads what the user types. It returns text, so `Convert.ToInt32()` converts numeric text into an integer, while `Convert.ToDouble()` converts it into a double value.
- `DisplayFacultyDetails()` prints the values stored in the fields.
- `Faculty f = new Faculty();` creates one Faculty object.
- `f.GetFacultyDetails();` calls the input method; `f.DisplayFacultyDetails();` then displays the same object's stored information.

### Important beginner point

Inside `GetFacultyDetails()`, write `id = Convert.ToInt32(...)`, not `int id = ...`. Writing `int id` again would create a local variable that hides the field, and the class field would remain unchanged.

---

# Question 2 – Employee Details for Five Objects

## Question

Create a class named `Employee` with Emp_ID, Name, Department, Designation and Salary as data members. Create `GetEmpDetails()` and `DisplayEmpDetails()` methods for five different employee objects.

## Understand the Question

Unlike Question 1, we need to store **five employees**. Each employee has five pieces of information. We use an array of Employee objects so each array position represents one employee record.

The steps are:
1. Declare an array with five positions.
2. Create an Employee object at each position.
3. Call its input method to fill that object's fields.
4. Loop again to display every employee.

## Program

```csharp
using System;

namespace Lab2
{
    internal class Employee
    {
        int emp_ID;
        string name;
        string department;
        string designation;
        double salary;

        public void GetEmpDetails()
        {
            Console.Write("Enter Employee ID: ");
            emp_ID = Convert.ToInt32(Console.ReadLine());

            Console.Write("Enter Name: ");
            name = Console.ReadLine();

            Console.Write("Enter Department: ");
            department = Console.ReadLine();

            Console.Write("Enter Designation: ");
            designation = Console.ReadLine();

            Console.Write("Enter Salary: ");
            salary = Convert.ToDouble(Console.ReadLine());
        }

        public void DisplayEmpDetails()
        {
            Console.WriteLine("Employee ID : " + emp_ID);
            Console.WriteLine("Name        : " + name);
            Console.WriteLine("Department  : " + department);
            Console.WriteLine("Designation : " + designation);
            Console.WriteLine("Salary      : " + salary);
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Employee[] employees = new Employee[5];

            for (int i = 0; i < employees.Length; i++)
            {
                Console.WriteLine("\nEnter details of Employee " + (i + 1));
                employees[i] = new Employee();
                employees[i].GetEmpDetails();
            }

            Console.WriteLine("\n========== All Employee Details ==========");

            for (int i = 0; i < employees.Length; i++)
            {
                Console.WriteLine("\nEmployee " + (i + 1));
                employees[i].DisplayEmpDetails();
            }
        }
    }
}
```

## Sample Output (Short Illustration)

```text
Enter details of Employee 1
Enter Employee ID: 1
Enter Name: Rahul
Enter Department: IT
Enter Designation: Developer
Enter Salary: 30000

... input for employees 2, 3, 4 and 5 ...

========== All Employee Details ==========
Employee 1
Employee ID : 1
Name        : Rahul
Department  : IT
Designation : Developer
Salary      : 30000
... displayed details for the remaining employees ...
```

## Code Explanation

- `Employee[] employees = new Employee[5];` creates an array that can hold five Employee references.
- The first loop runs from `i = 0` to `i = 4`. Each time, `new Employee()` creates a separate employee object and stores it in `employees[i]`.
- `employees[i].GetEmpDetails()` reads the data into that particular object. Each employee has their own field values.
- The second loop visits all five objects and calls `DisplayEmpDetails()`.
- `salary` is a `double` so the program can accept salary values that include decimal amounts.

### Why are two loops used?

The first loop gathers and stores information. The second loop displays the saved records after all the input has been entered. Keeping these tasks separate makes the output a complete list/report.

---

# Part B

# Question 3 – Calculate Cube Volume using Constructor

## Question

Write a program to calculate the Volume of a Cube using constructor.

## Theory / Formula

A cube has six square faces, and all its edges have the same length. To find its volume, multiply the side length by itself three times:

**Volume = side × side × side = side³**

Example: If the side is 5 units, volume = 5 × 5 × 5 = 125 cubic units.

## Understand the Program

We read the side from the user. When we create the Cube object, we pass the side to its constructor. The constructor stores the side and calculates the volume immediately. A display method prints the result.

## Program

```csharp
using System;

namespace Lab2
{
    internal class Cube
    {
        int side;
        int volume;

        public Cube(int s)
        {
            side = s;
            volume = side * side * side;
        }

        public void DisplayVolume()
        {
            Console.WriteLine("Volume of Cube: " + volume);
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Enter side of cube: ");
            int s = Convert.ToInt32(Console.ReadLine());

            Cube c = new Cube(s);
            c.DisplayVolume();
        }
    }
}
```

## Sample Output

```text
Enter side of cube: 5
Volume of Cube: 125
```

## Code Explanation

- `int side;` stores the side length.
- `int volume;` stores the calculated volume.
- `public Cube(int s)` is the constructor. Its name matches the class (`Cube`) and it has no return type.
- When `new Cube(s)` is executed, `s` is passed to the constructor.
- `side = s;` copies the incoming value into the object's field.
- `volume = side * side * side;` calculates the volume.
- `DisplayVolume()` prints the result.

### Why calculate inside the constructor?

The question specifically asks to use a constructor. It demonstrates initializing an object and performing the calculation using the value provided when the object is created.

---

# Question 4 – Premium Sports Car Details using Constructor

## Question

A car dealership wants to display details of premium sports cars. Create a `Car` object with Make, Model, Year, FuelType and Horsepower details using a constructor.

## Understand the Question

The Car class stores five attributes. We will pass all five values to a parameterized constructor during object creation. The constructor assigns each parameter to its corresponding field. A display method prints the car information.

| Field | Meaning | Type |
|---|---|---|
| `make` | Manufacturer/brand | `string` |
| `model` | Car model | `string` |
| `year` | Manufacturing year | `int` |
| `fuelType` | Fuel used | `string` |
| `horsepower` | Engine power measurement | `int` |

## Program

```csharp
using System;

namespace Lab2
{
    internal class Car
    {
        string make;
        string model;
        int year;
        string fuelType;
        int horsepower;

        public Car(string make, string model, int year,
                  string fuelType, int horsepower)
        {
            this.make = make;
            this.model = model;
            this.year = year;
            this.fuelType = fuelType;
            this.horsepower = horsepower;
        }

        public void DisplayCarDetails()
        {
            Console.WriteLine("\n--- Premium Sports Car Details ---");
            Console.WriteLine("Make       : " + make);
            Console.WriteLine("Model      : " + model);
            Console.WriteLine("Year       : " + year);
            Console.WriteLine("Fuel Type  : " + fuelType);
            Console.WriteLine("Horsepower : " + horsepower);
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Car c = new Car("Porsche", "911", 2025, "Petrol", 640);
            c.DisplayCarDetails();
        }
    }
}
```

## Sample Output

```text
--- Premium Sports Car Details ---
Make       : Porsche
Model      : 911
Year       : 2025
Fuel Type  : Petrol
Horsepower : 640
```

## Code Explanation

- The five fields hold the car's details.
- The constructor takes five parameters in the same order used in the object creation statement.
- `this.make = make;` assigns the parameter `make` to the field `make`. The `this` keyword makes it clear that the left side belongs to the current object's field.
- The same assignment pattern is used for the other fields.
- `new Car("Porsche", "911", 2025, "Petrol", 640)` creates an object and initializes it immediately. This example uses sample values directly rather than asking the user for input, demonstrating constructor initialization.

### Parameter order matters

When creating the object, pass the values in the constructor's expected order: make, model, year, fuel type, horsepower. If the order or type is incorrect, the program may fail to compile or initialize the wrong data.

---

# Question 5 – Supermarket Inventory using an Array of Objects

## Question

Write a program to manage supermarket inventory. Create an `Item` class with Item_Code, Item_Name and Stock_Quantity. Use a constructor to initialize the objects and an array of objects to store and display the details of 10 different items.

## Understand the Question

Each inventory item has:

| Field | Purpose | Type |
|---|---|---|
| `item_Code` | Identifies the item | `int` |
| `item_Name` | Item's name | `string` |
| `stock_Quantity` | Number of units available | `int` |

We need ten items. The program accepts the three values for each item, passes them to the Item constructor, stores the created Item in the array, and then displays all records.

## Program

```csharp
using System;

namespace Lab2
{
    internal class Item
    {
        int item_Code;
        string item_Name;
        int stock_Quantity;

        public Item(int code, string name, int quantity)
        {
            item_Code = code;
            item_Name = name;
            stock_Quantity = quantity;
        }

        public void DisplayItemDetails()
        {
            Console.WriteLine("Item Code      : " + item_Code);
            Console.WriteLine("Item Name      : " + item_Name);
            Console.WriteLine("Stock Quantity : " + stock_Quantity);
            Console.WriteLine("-------------------------------");
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Item[] items = new Item[10];

            for (int i = 0; i < items.Length; i++)
            {
                Console.WriteLine("\nEnter details of Item " + (i + 1));

                Console.Write("Enter Item Code: ");
                int code = Convert.ToInt32(Console.ReadLine());

                Console.Write("Enter Item Name: ");
                string name = Console.ReadLine();

                Console.Write("Enter Stock Quantity: ");
                int quantity = Convert.ToInt32(Console.ReadLine());

                items[i] = new Item(code, name, quantity);
            }

            Console.WriteLine("\n========== Supermarket Inventory ==========");

            for (int i = 0; i < items.Length; i++)
            {
                Console.WriteLine("\nItem " + (i + 1));
                items[i].DisplayItemDetails();
            }
        }
    }
}
```

## Sample Output (Illustration)

```text
Enter details of Item 1
Enter Item Code: 101
Enter Item Name: Rice
Enter Stock Quantity: 50

... input for items 2 through 10 ...

========== Supermarket Inventory ==========
Item 1
Item Code      : 101
Item Name      : Rice
Stock Quantity : 50
-------------------------------
... displayed details for remaining items ...
```

## Code Explanation

- `Item[] items = new Item[10];` declares ten slots for Item references.
- The first loop collects item information. Each input is saved in a local variable (`code`, `name`, `quantity`) before object creation.
- `items[i] = new Item(code, name, quantity);` calls the parameterized constructor and stores the new object at index `i`.
- The constructor assigns each supplied value to the object's fields. Every item object keeps its own values.
- The second loop calls `DisplayItemDetails()` for each stored item.

### Why use a constructor here?

The Item object is created with all required values in one statement. It cannot be created through this constructor without providing code, name and quantity, helping ensure that each item is initialized with those details.

---

# Question 6 – Clinic Patient Records using Array of Objects

## Question

Create a `Patient` class containing Patient_ID, Name, Age and Disease. Use an array of objects to accept details for multiple patients and display a formatted summary report of all patients.

## Understand the Question

The program should manage multiple patient records. Each patient has an ID, name, age and disease. The user first enters how many patients they want to store. We use that number as the array length, so the program can work with different numbers of patients instead of only a fixed number.

Steps:
1. Read the number of patients.
2. Create a Patient array of that size.
3. For each patient, read ID, name, age and disease.
4. Create the patient object using the constructor and store it in the array.
5. Print a report header and display each patient's details in aligned columns.

## Program

```csharp
using System;

namespace Lab2
{
    internal class Patient
    {
        int patient_ID;
        string name;
        int age;
        string disease;

        public Patient(int id, string name, int age, string disease)
        {
            patient_ID = id;
            this.name = name;
            this.age = age;
            this.disease = disease;
        }

        public void DisplayPatientDetails()
        {
            Console.WriteLine("{0,-15} {1,-15} {2,-10} {3,-20}",
                patient_ID, name, age, disease);
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Enter number of patients: ");
            int n = Convert.ToInt32(Console.ReadLine());

            Patient[] patients = new Patient[n];

            for (int i = 0; i < patients.Length; i++)
            {
                Console.WriteLine("\nEnter details of Patient " + (i + 1));

                Console.Write("Enter Patient ID: ");
                int id = Convert.ToInt32(Console.ReadLine());

                Console.Write("Enter Name: ");
                string name = Console.ReadLine();

                Console.Write("Enter Age: ");
                int age = Convert.ToInt32(Console.ReadLine());

                Console.Write("Enter Disease: ");
                string disease = Console.ReadLine();

                patients[i] = new Patient(id, name, age, disease);
            }

            Console.WriteLine("\n================ Patient Summary Report ================\n");
            Console.WriteLine("{0,-15} {1,-15} {2,-10} {3,-20}",
                "Patient ID", "Name", "Age", "Disease");
            Console.WriteLine("---------------------------------------------------------");

            for (int i = 0; i < patients.Length; i++)
            {
                patients[i].DisplayPatientDetails();
            }
        }
    }
}
```

## Sample Output (Illustration)

```text
Enter number of patients: 2

Enter details of Patient 1
Enter Patient ID: 101
Enter Name: Rahul
Enter Age: 25
Enter Disease: Fever

Enter details of Patient 2
Enter Patient ID: 102
Enter Name: Priya
Enter Age: 30
Enter Disease: Cough

================ Patient Summary Report ================
Patient ID      Name            Age        Disease
---------------------------------------------------------
101            Rahul           25         Fever
102            Priya           30         Cough
```

## Code Explanation

- `int n` stores the number of patient records requested by the user.
- `Patient[] patients = new Patient[n];` creates an array with `n` positions.
- The first `for` loop reads the information for each patient.
- `new Patient(id, name, age, disease)` invokes the constructor, passing all four details.
- `patients[i] = ...` stores that object in the current array slot.
- The display method uses formatted output to align the data into columns.
- `{0,-15}` means print the first value left-aligned in a field of 15 characters; `{1,-15}` applies to the next value; `{2,-10}` and `{3,-20}` set the widths for the other columns. This is formatting for readability, not a change to the data.

---

# 3. Concepts Compared

## Class vs Object

| Class | Object |
|---|---|
| Blueprint/template | Instance created from a class |
| Defines the fields and methods | Holds its own instance field values |
| Example: `Faculty` | Example: `Faculty f = new Faculty();` |

## Method vs Constructor

| Method | Constructor |
|---|---|
| Performs an operation | Initializes a newly created object |
| Has a method name | Name matches the class |
| May have a return type (or `void`) | Has no return type |
| Usually called explicitly | Runs automatically as object is created |

## Parameterless vs Parameterized Constructor

| Parameterless | Parameterized |
|---|---|
| Takes no arguments | Accepts one or more arguments |
| Example: `Student()` | Example: `Student(int id)` |
| Initializes using fixed/default logic | Initializes using values supplied at creation |

## One Object vs Array of Objects

| One Object | Array of Objects |
|---|---|
| Holds one instance reference | Holds several instance references |
| `Faculty f = new Faculty();` | `Employee[] employees = new Employee[5];` |
| Use for one record | Use for multiple records |

---

# 4. Common Errors / Mistakes to Avoid

## Mistake 1 – Declaring a constructor incorrectly

Incorrect:
```csharp
class Student(int id)
{
}
```

Correct (constructor written inside the class):
```csharp
class Student
{
    public Student(int id)
    {
    }
}
```

## Mistake 2 – Giving a constructor a return type

Incorrect:
```csharp
public void Student(int id) { }
```

Correct:
```csharp
public Student(int id) { }
```

A constructor has no return type, including `void`.

## Mistake 3 – Creating a local variable instead of updating a field

Incorrect:
```csharp
int id;
void GetDetails()
{
    int id = Convert.ToInt32(Console.ReadLine());
}
```

The `int id` inside the method creates a separate local variable. It hides the class field, so the field does not receive the entered value.

Correct:
```csharp
int id;
void GetDetails()
{
    id = Convert.ToInt32(Console.ReadLine());
}
```

## Mistake 4 – Mixing up `this` assignment

Correct:
```csharp
this.id = id;
```

The field is on the left; the incoming constructor parameter is on the right.

## Mistake 5 – Calling an object method before creating the object

Incorrect:
```csharp
Employee[] employees = new Employee[5];
employees[0].GetEmpDetails();
```

The array exists, but `employees[0]` is still `null` because an Employee instance has not been created there.

Correct:
```csharp
Employee[] employees = new Employee[5];
employees[0] = new Employee();
employees[0].GetEmpDetails();
```

## Mistake 6 – Off-by-one array indexes

An array of five elements uses indexes `0, 1, 2, 3, 4`. Use `i < array.Length`, not `i <= array.Length`, because index `5` would be outside the array.

Correct loop:
```csharp
for (int i = 0; i < employees.Length; i++)
{
    // process employees[i]
}
```

## Mistake 7 – Forgetting to convert console input

`Console.ReadLine()` returns text. If you need an integer or decimal number, convert it, for example:
```csharp
int age = Convert.ToInt32(Console.ReadLine());
double salary = Convert.ToDouble(Console.ReadLine());
```
If the user types non-numeric text when a number is expected, conversion can fail.

---

# Source

Questions are taken from **2401CS521 – Building RESTful APIs with ASP.NET Core, Laboratory/Tutorial Manual, Lab 2: Class, Objects, Array of Objects & Constructor, Academic Year 2026–27**. The manual lists six exercises: Faculty details; five Employee objects; Cube volume using a constructor; Car details using a constructor; supermarket inventory using ten Item objects; and multiple clinic Patient records with a formatted summary report.
