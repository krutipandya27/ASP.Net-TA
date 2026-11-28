# Lab 4 – Student and Subject API Using In-Memory Data

**Course:** 2401CS521 – Building RESTful APIs with ASP.NET Core  
**Topic:** Classes, Collections, Static In-Memory Data, Controllers, and GET Endpoints

---

## 1. Aim

Create a `Student` class that contains a list of `Subject` objects and a `Subject` class. Preload five sample student records with their subjects in a static in-memory `List<Student>`. Create a `StudentsController` with endpoints to:

- Fetch all students.
- Fetch one student by ID (return HTTP 404 when the student does not exist).
- Fetch the subjects of a student (return HTTP 404 when the student does not exist).

No database is used in this practical.

## 2. Learning Objectives

After completing this practical, students should understand:

- How to create model classes and properties in C#.
- How one class can contain a collection of objects of another class.
- How to create and initialize objects using constructors.
- How a static list can hold sample data in memory.
- How ASP.NET Core API controllers and route attributes work.
- How to return successful and not-found HTTP responses.
- How to test GET endpoints.

## 3. Theory and Concepts

### 3.1 Model Class

A model class represents the structure of a type of data used by an application. For this practical, `Student` represents a student record and `Subject` represents one subject. Properties describe the information stored in each object.

### 3.2 Properties (`get` and `set`)

A C# property provides a way to read or update a class value. For example:

```csharp
public int StudentId { get; set; }
```

- `get` allows the value to be read.
- `set` allows the value to be assigned or changed.
- `public` makes the property accessible from other classes.

### 3.3 Classes and Objects

A class is a blueprint for creating objects. An object is an instance of a class. The `new` keyword creates an object and invokes its constructor.

For example, `new Subject(1, "C#")` creates one subject object with the given ID and name.

### 3.4 `List<T>`

`List<T>` is a collection that can hold multiple values or objects of the specified type. Unlike a fixed-size array, a list can grow or shrink as elements are added or removed.

In this lab, `List<Subject>` stores multiple subjects for a student, and `List<Student>` stores the sample student records.

### 3.5 List of Objects / Relationship Between Student and Subject

A student can study multiple subjects. The `Student` class therefore has a `List<Subject>` property. This represents a simple one-student-to-many-subjects relationship in our in-memory sample data.

### 3.6 Static In-Memory List

A `static` member belongs to the class itself rather than to a particular object. It can be accessed using the class name without creating an instance.

The sample data in this lab is stored in a static `List<Student>`. It is held in application memory, not in a database. It is available while the app is running, but changes are not persistent and the sample list is recreated when the app restarts.

### 3.7 ASP.NET Core Controller

A controller handles HTTP requests and returns HTTP responses. `ControllerBase` provides common API response methods such as `Ok()` and `NotFound()`.

- `Ok(data)` returns HTTP **200 OK** with the supplied data.
- `NotFound()` returns HTTP **404 Not Found** when the requested record is not available.

### 3.8 Routing and Attributes

Attributes are written in square brackets above classes or methods and provide instructions to ASP.NET Core.

- `[ApiController]` marks the class as an API controller and enables API-specific behaviors.
- `[Route("api/[controller]")]` defines the controller's base route. `[controller]` is replaced with the controller name without the `Controller` suffix. For `StudentsController`, the base route is `api/students`.
- `[HttpGet]` specifies that the method handles a GET request.
- `[HttpGet("{id}")]` adds an ID parameter to the route.
- `[HttpGet("{id}/subjects")]` adds a route to fetch the subjects for a particular student ID.

### 3.9 `ActionResult<T>`

`ActionResult<T>` allows a controller method to return either a value of the specified type or an HTTP result such as `NotFound()`. It is useful when an endpoint can return data on success or a different status when the requested item is missing.

### 3.10 Finding a Student with `Find()`

The list's `Find()` method searches for the first element that matches a condition. In this lab, a lambda expression checks each student's ID:

```csharp
var student = StudentData.Students.Find(s => s.StudentId == id);
```

Here, `s` represents a student being checked, and `s.StudentId == id` is the condition. If a matching student is found, `Find()` returns that object. If none is found, it returns `null`.

---

## 4. Create the Project

Create an ASP.NET Core Web API project in Visual Studio. If prompted for project options, select a Web API template and make sure controllers are enabled. This practical uses controller-based endpoints.

Add these folders and files to the project:

```text
StudentAPI
├── Controllers
│   └── StudentsController.cs
├── Data
│   └── StudentData.cs
├── Models
│   ├── Student.cs
│   └── Subject.cs
└── Program.cs
```

> If your project has a different namespace, use that namespace consistently in the files and `using` statements below. The example uses `StudentAPI`.

## 5. Create the Subject Class

Create `Models/Subject.cs`.


```csharp
namespace StudentAPI.Models
{
    public class Subject
    {
        public int SubjectId { get; set; }
        public string SubjectName { get; set; }

        public Subject(int subjectId, string subjectName)
        {
            SubjectId = subjectId;
            SubjectName = subjectName;
        }
    }
}
```

### Explanation

- `SubjectId` stores the subject's identification number.
- `SubjectName` stores the name of the subject.
- The constructor accepts an ID and name and assigns them to the object's properties.
- When `new Subject(1, "C#")` is called, a Subject object is created with ID 1 and name C#.

## 6. Create the Student Class

Create `Models/Student.cs`.

```csharp
using System.Collections.Generic;

namespace StudentAPI.Models
{
    public class Student
    {
        public int StudentId { get; set; }
        public string StudentName { get; set; }
        public List<Subject> Subjects { get; set; }

        public Student(int studentId, string studentName, List<Subject> subjects)
        {
            StudentId = studentId;
            StudentName = studentName;
            Subjects = subjects;
        }
    }
}
```

### Explanation

- `StudentId` stores the student's ID.
- `StudentName` stores the student's name.
- `Subjects` stores a list of Subject objects belonging to this student.
- The constructor accepts the student's ID, name, and subject list, then assigns them to the properties.

The `List<Subject>` property is important because one student can have more than one subject. For example, a student may have C#, Database, and Web Development subjects.

## 7. Create the Static In-Memory Student Data

Create `Data/StudentData.cs`. This class contains five predefined student records and their subjects.

```csharp
using System.Collections.Generic;
using StudentAPI.Models;

namespace StudentAPI.Data
{
    public static class StudentData
    {
        public static List<Student> Students = new List<Student>
        {
            new Student(1, "Rahul", new List<Subject>
            {
                new Subject(1, "C#"),
                new Subject(2, "Database")
            }),

            new Student(2, "Priya", new List<Subject>
            {
                new Subject(1, "Java"),
                new Subject(2, "Web Development")
            }),

            new Student(3, "Amit", new List<Subject>
            {
                new Subject(1, "Python"),
                new Subject(2, "Machine Learning")
            }),

            new Student(4, "Neha", new List<Subject>
            {
                new Subject(1, "C++"),
                new Subject(2, "Data Structures")
            }),

            new Student(5, "Karan", new List<Subject>
            {
                new Subject(1, "C#"),
                new Subject(2, "ASP.NET Core")
            })
        };
    }
}
```

### Explanation

- `static class StudentData` means the class is accessed directly by its name and does not need to be instantiated.
- `static List<Student> Students` is the shared list of student objects.
- Each `new Student(...)` creates one student object.
- Inside each student, `new List<Subject>` creates that student's subject list.
- Each `new Subject(...)` creates one subject object.

The records are sample data. They are not stored in a database or a file. If the application is stopped and started again, this list is initialized again with the sample values.

## 8. Create StudentsController

Create `Controllers/StudentsController.cs`.

```csharp
using Microsoft.AspNetCore.Mvc;
using StudentAPI.Data;
using StudentAPI.Models;
using System.Collections.Generic;


namespace StudentAPI.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class StudentsController : ControllerBase
    {
        // GET: api/students
        [HttpGet]
        public ActionResult<List<Student>> GetAllStudents()
        {
            return Ok(StudentData.Students);
        }

        // GET: api/students/1
        [HttpGet("{id}")]
        public ActionResult<Student> GetStudentById(int id)
        {
            var student = StudentData.Students.Find(s => s.StudentId == id);

            if (student == null)
            {
                return NotFound();
            }

            return Ok(student);
        }

        // GET: api/students/1/subjects
        [HttpGet("{id}/subjects")]
        public ActionResult<List<Subject>> GetStudentSubjects(int id)
        {
            var student = StudentData.Students.Find(s => s.StudentId == id);

            if (student == null)
            {
                return NotFound();
            }

            return Ok(student.Subjects);
        }
    }
}
```

### Explanation of the Controller

#### 8.1 Controller Route

`[Route("api/[controller]")]` sets the controller's base route. ASP.NET Core replaces `[controller]` with `Students`, so the route begins with `/api/students`.

#### 8.2 `GetAllStudents()`

The `[HttpGet]` attribute maps a GET request to this method. `Ok(StudentData.Students)` returns all the student objects and their subject lists with HTTP status 200.

#### 8.3 `GetStudentById(int id)`

The route `[HttpGet("{id}")]` accepts an ID from the URL. `Find()` searches the static list for a matching `StudentId`. If the result is `null`, `NotFound()` returns 404. Otherwise, `Ok(student)` returns the matched student with 200.

#### 8.4 `GetStudentSubjects(int id)`

The route `[HttpGet("{id}/subjects")]` includes the student ID and the `subjects` path segment. It first finds the student. If no student exists, it returns 404. If the student is found, `Ok(student.Subjects)` returns only that student's subject list with 200.

## 9. Configure Program.cs

Ensure your `Program.cs` registers controllers and maps controller routes. For a minimal controller-based API, it can look like this:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

var app = builder.Build();

app.MapControllers();

app.Run();
```

### Explanation

- `WebApplication.CreateBuilder(args)` prepares the application builder and configuration.
- `AddControllers()` registers controller services.
- `Build()` creates the web application.
- `MapControllers()` makes controller routes available to incoming requests.
- `Run()` starts the application.

If your project template includes OpenAPI/Swagger configuration, you can keep those existing lines as well; the important requirement for these controller endpoints is registering controllers and mapping them.

## 10. Run and Test the API

Start the project. Use Swagger (if enabled), a browser, or Postman to send GET requests to these endpoints. Replace the base address/port with the one shown by your application when it starts.


### Endpoint 1: Fetch all students

**Request:**

```http
GET /api/students
```

**Expected result:** HTTP `200 OK`. The response contains five students, and each student includes their subjects.

### Endpoint 2: Fetch a student by ID

**Request:**

```http
GET /api/students/1
```

**Expected result:** HTTP `200 OK` and the details for student ID 1 (Rahul).

Try a non-existent ID:

```http
GET /api/students/99
```

**Expected result:** HTTP `404 Not Found`, because there is no student with ID 99.

### Endpoint 3: Fetch a student's subjects

**Request:**

```http
GET /api/students/1/subjects
```

**Expected result:** HTTP `200 OK` and only the subjects for student ID 1.

Try a non-existent ID:

```http
GET /api/students/99/subjects
```

**Expected result:** HTTP `404 Not Found`, because the student does not exist.

## 11. Example Response Shapes

### All students

The response is an array of students. Each student contains a `studentId`, `studentName`, and a `subjects` array.

```json
[
  {
    "studentId": 1,
    "studentName": "Rahul",
    "subjects": [
      { "subjectId": 1, "subjectName": "C#" },
      { "subjectId": 2, "subjectName": "Database" }
    ]
  }
]
```

The actual response contains all five sample students; the example above shows only one record to make the structure easier to understand.

### Single student

```json
{
  "studentId": 1,
  "studentName": "Rahul",
  "subjects": [
    { "subjectId": 1, "subjectName": "C#" },
    { "subjectId": 2, "subjectName": "Database" }
  ]
}
```

### Subjects only

```json
[
  { "subjectId": 1, "subjectName": "C#" },
  { "subjectId": 2, "subjectName": "Database" }
]
```

## 12. Common Errors and Their Causes

### Error: Controller endpoints return 404

Check that `builder.Services.AddControllers()` is present and that `app.MapControllers()` is called in `Program.cs`. Confirm the URL and controller route are correct.

### Error: A namespace cannot be found

Ensure the namespace names and `using` statements match your project. If your project is not named `StudentAPI`, replace the example namespaces accordingly.

### Error: Student lookup returns 404

The requested ID may not exist in the predefined list. The sample data only contains IDs 1 through 5.

### Error: Subjects are missing from the response

Check that each Student is constructed with a `List<Subject>` and that the `Subjects` property is assigned in the Student constructor.

### Error: Data disappears after restarting

This is expected. The list is in memory, not saved permanently. Restarting the app initializes the sample data again.

## 13. Conclusion

In this practical, we created `Student` and `Subject` model classes, stored five sample student records and their subjects in a static in-memory list, and implemented a controller with GET endpoints to retrieve all students, one student by ID, and a student's subjects. We also handled missing student IDs by returning HTTP 404 Not Found. The application uses in-memory data only and does not require a database.
