# Classes, Objects, and Constructors in Java

## 1. What is OOP?

**Interview definition:**

Object-Oriented Programming (OOP) is a programming paradigm that organizes software around objects. An object combines **state (data)** and **behaviour (methods)** into a single unit.

Instead of writing a program only as a collection of functions, we model entities as objects that interact with each other.

### Procedural vs Object-Oriented Programming

| Procedural approach | Object-oriented approach |
|---|---|
| Focuses on functions | Focuses on objects |
| Data and functions are often separate | Data and methods are grouped in classes |
| Data may be shared across functions | Objects manage their own state |
| Example: `calculateFee()` | Example: `student.calculateFee()` |

### Real-world example

Think about a student.

- **State:** name, age, roll number, fee
- **Behaviour:** study, attend class, pay fee, display details

An object-oriented representation groups these together.

---

# 2. What is a Class?

**Interview definition:**

A class is a blueprint or template used to create objects. It defines the data (fields) and behaviour (methods) that its objects can have.

### Visual representation

```text
                 CLASS: Student
          ┌──────────────────────────┐
          │                          │
          │       DATA / STATE       │
          │                          │
          │  name                    │
          │  age                     │
          │  rollNumber              │
          │  fee                     │
          │                          │
          ├──────────────────────────┤
          │                          │
          │    METHODS / BEHAVIOUR   │
          │                          │
          │  study()                 │
          │  attendClass()           │
          │  payFee()                │
          │  displayDetails()        │
          │                          │
          └──────────────────────────┘
```

### Java implementation

```java
class Student {

    // Fields / Instance variables
    String name;
    int age;
    int rollNumber;
    double fee;

    // Methods / Behaviour
    void study() {
        System.out.println(name + " is studying");
    }

    void displayDetails() {
        System.out.println(name);
        System.out.println(age);
        System.out.println(rollNumber);
        System.out.println(fee);
    }
}
```

**Important terminology**

| Term | Meaning |
|---|---|
| Class | Blueprint for creating objects |
| Field | Variable declared inside a class |
| Instance variable | Non-static field belonging to an object |
| Method | Defines an object's behaviour |
| Member | General term for fields, methods, constructors, etc. |

---

# 3. What is an Object?

**Interview definition:**

An object is an instance of a class. It has its own identity, state, and behaviour.

For example, `Student` is a class, while an individual student such as Rahul or Priya can be represented as an object.

### One class, multiple objects

```java
class Student {
    String name;
    int age;
    int rollNumber;
    double fee;
}

public class Main {
    public static void main(String[] args) {

        Student s1 = new Student();

        s1.name = "Rahul";
        s1.age = 21;
        s1.rollNumber = 101;
        s1.fee = 25000;

        Student s2 = new Student();

        s2.name = "Priya";
        s2.age = 22;
        s2.rollNumber = 102;
        s2.fee = 30000;
    }
}
```

### Visual representation

```text
                    Student CLASS
                 (Common blueprint)
                         |
             ┌───────────┴───────────┐
             |                       |
             ▼                       ▼

        OBJECT 1                 OBJECT 2
        Student                 Student
    ┌────────────────┐      ┌────────────────┐
    │ name = Rahul   │      │ name = Priya   │
    │ age = 21       │      │ age = 22       │
    │ roll = 101     │      │ roll = 102     │
    │ fee = 25000    │      │ fee = 30000    │
    └────────────────┘      └────────────────┘
```

**Remember:**

- A class defines the structure.
- Each object maintains its own instance-variable values.
- Multiple objects can be created from the same class.
- Changing the state of one object does not automatically change another object's state.

---

# 4. Understanding Instance Variables

**Definition:**

Instance variables are non-static variables declared inside a class but outside methods, constructors, and blocks. Every object gets its own instance variables.

Example:

```java
class Student {

    String name;
    int age;
    double fee;
    boolean isActive;
}
```

Here, all four variables are instance variables.

### Default values of instance variables

When an object is created, its instance fields receive default values before the constructor executes.

| Data type | Default value |
|---|---|
| `byte` | `0` |
| `short` | `0` |
| `int` | `0` |
| `long` | `0L` |
| `float` | `0.0f` |
| `double` | `0.0d` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| Reference types (e.g. `String`) | `null` |

Example:

```java
class Student {
    String name;
    int age;
    double fee;
    boolean isActive;

    void display() {
        System.out.println(name);
        System.out.println(age);
        System.out.println(fee);
        System.out.println(isActive);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.display();
    }
}
```

Output:

```text
null
0
0.0
false
```

### Why do instance variables get default values?

- Java initializes object fields to their default values during object creation.
- This ensures that object fields start with predictable values.
- The Java language specifies this behaviour; it is not simply an optimization trick.

## 5. Local Variables vs Instance Variables

| Instance variables | Local variables |
|---|---|
| Declared inside a class, outside methods/constructors/blocks | Declared inside a method, constructor, or block |
| Belong to an object | Belong to the execution scope |
| Get default values | Do not get default values automatically |
| Exist as part of the object's state | Exist while their scope is active |
| Can be accessed through an object reference (subject to access rules) | Accessible only within their scope |

Example:

```java
class Student {

    int age = 20; // Instance variable

    void display() {

        int marks = 90; // Local variable

        System.out.println(age);
        System.out.println(marks);
    }
}
```

This code will not compile:

```java
class Student {

    void display() {

        int marks;

        System.out.println(marks); // Compilation error
    }
}
```

**Why?**

Java requires local variables to be definitely assigned before they are read.

This is a compile-time language rule. It should not be explained as simply being because local variables are stored on the stack: JVM optimizations and actual storage locations are implementation-dependent.

---

# 6. What is a Reference Variable?

A reference variable holds a reference to an object. It allows us to access the object's fields and methods.

Example:

```java
Student s1 = new Student();
```

Let's break this statement down.

```text
Student s1 = new Student();
   |     |         |
   |     |         |
   |     |         └── Object creation expression
   |     |
   |     └── Reference variable
   |
   └── Reference type
```

### Complete breakdown

| Part | Meaning |
|---|---|
| `Student` (left side) | Declares the variable's reference type |
| `s1` | Name of the reference variable |
| `new` | Creates a new object |
| `Student()` | Invokes the constructor |
| `=` | Assigns the resulting reference to `s1` |

**Important:** `s1` is not the object itself. It is a reference through which we access the object.

### Stack and Heap visualization

For a typical explanation of JVM memory:

```text
             JVM MEMORY (CONCEPTUAL)

       STACK                         HEAP
  ┌────────────────┐          ┌─────────────────────┐
  │                │          │                     │
  │  main() frame  │          │   Student Object    │
  │                │          │                     │
  │  s1 ──────────────────────►│   name = null       │
  │                │          │   age = 0           │
  │                │          │   rollNumber = 0    │
  │                │          │   fee = 0.0         │
  │                │          │                     │
  └────────────────┘          └─────────────────────┘
```

### What happens when we create two objects?

```java
Student s1 = new Student();
Student s2 = new Student();
```

```text
             STACK                         HEAP
       ┌────────────────┐          ┌───────────────────┐
       │                │          │                   │
       │  s1 ─────────────────────►│   Student Object 1│
       │                │          │   name = null     │
       │  s2 ─────────────────────────►Student Object 2│
       │                │          │   name = null     │
       └────────────────┘          └───────────────────┘
```

The two objects have independent instance state.

### What if two references point to the same object?

```java
Student s1 = new Student();

Student s2 = s1;
```

```text
             STACK                         HEAP
       ┌────────────────┐          ┌───────────────────┐
       │                │          │                   │
       │  s1 ────────────────┐     │                   │
       │                │   └────►│   Student Object  │
       │  s2 ────────────────┘     │                   │
       │                │          │                   │
       └────────────────┘          └───────────────────┘
```

Now both references refer to the same object.

Therefore:

```java
s1.name = "Rahul";

System.out.println(s2.name);
```

Output:

```text
Rahul
```

**Key takeaway:** Multiple reference variables can refer to the same object.

For a deeper explanation of stack frames, heap objects, references, and object layout, see [Object Memory Layout](../02-Object-Memory/object-memory-layout.md).

---

# 7. What is a Constructor?

**Interview definition:**

A constructor is a special member of a class that is invoked during object creation and is primarily used to initialize the object's state.

Example:

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 18;
    }
}
```

Usage:

```java
Student s1 = new Student();

System.out.println(s1.name);
System.out.println(s1.age);
```

Output:

```text
Unknown
18
```

### Constructor execution visualization

```text
       Student s1 = new Student();
                     |
                     ▼
              Object allocation
                     |
                     ▼
          Instance fields get default values
                     |
                     ▼
              Constructor executes
                     |
                     ▼
            Fields get initialized
                     |
                     ▼
          Reference assigned to s1
```

The object is initialized before it is used by the calling code.

---

# 8. Rules of Constructors

1. Constructor name must be exactly the same as the class name.
2. A constructor has no return type—not even `void`.
3. Constructors are invoked as part of object creation using `new` or through constructor chaining.
4. Constructors are primarily used to initialize objects.
5. Constructors can be overloaded.
6. Constructors are not inherited.
7. Constructors cannot be declared `static`, `abstract`, or `final`.
8. A constructor can have access modifiers such as `public`, `protected`, and `private`.

Example:

```java
class Student {

    Student() {
        System.out.println("Constructor called");
    }
}
```

Incorrect:

```java
class Student {

    void Student() {
        // This is a method, not a constructor.
    }
}
```

Why? Because it has a return type (`void`).

---

# 9. Types of Constructors

For beginner-level Java, understand these two important categories.

## A. Default Constructor

If you do not declare any constructor in a class, the Java compiler provides a default no-argument constructor.

Example:

```java
class Student {

    String name;
    int age;

}
```

Java effectively provides a constructor similar to:

```java
Student() {
    super();
}
```

Usage:

```java
Student s1 = new Student();
```

The fields initially have their default values:

```text
name = null
age  = 0
```

**Important interview point:**

Java provides the compiler-generated default constructor only when the class declares no constructor at all.

If you declare even one constructor, Java does not automatically generate that default constructor.

## B. No-Argument Constructor

A no-argument constructor is any constructor that accepts no parameters.

You can write one yourself:

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 18;
    }
}
```

This is an explicitly declared no-argument constructor, not a compiler-generated default constructor.

## C. Parameterized Constructor

A parameterized constructor accepts arguments to initialize an object with specific values.

```java
class Student {

    String name;
    int age;
    int rollNumber;

    Student(String name, int age, int rollNumber) {

        this.name = name;
        this.age = age;
        this.rollNumber = rollNumber;
    }
}
```

Usage:

```java
Student s1 = new Student("Rahul", 21, 101);

Student s2 = new Student("Priya", 22, 102);
```

### Visualization

```text
         new Student("Rahul", 21, 101)
                         |
                         ▼
              Parameterized Constructor
                         |
             ┌───────────┼────────────┐
             ▼           ▼            ▼
           name         age       rollNumber
          "Rahul"        21           101
             |           |            |
             └───────────┼────────────┘
                         ▼
                  Student Object
```

---

# 10. Constructor Overloading

Constructor overloading means defining multiple constructors in the same class with different parameter lists.

Example:

```java
class Student {

    String name;
    int age;
    int rollNumber;

    // No-argument constructor
    Student() {
        name = "Unknown";
        age = 0;
        rollNumber = 0;
    }

    // One-parameter constructor
    Student(String name) {
        this.name = name;
    }

    // Two-parameter constructor
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Three-parameter constructor
    Student(String name, int age, int rollNumber) {
        this.name = name;
        this.age = age;
        this.rollNumber = rollNumber;
    }
}
```

Usage:

```java
Student s1 = new Student();
Student s2 = new Student("Rahul");
Student s3 = new Student("Priya", 22);
Student s4 = new Student("Aman", 23, 104);
```

### Visual representation

```text
                    Student
                       |
          ┌────────────┼─────────────┐
          |            |             |
          ▼            ▼             ▼
      Student()   Student(name)  Student(name, age)
          |            |             |
          ▼            ▼             ▼
       Default       Name set      Name + age set
       values
```

**Interview point:** Constructor overloading is an example of compile-time polymorphism because the compiler selects the constructor based on the argument list.

---

# 11. The `this` Keyword

**Interview definition:**

`this` is a reference to the current object whose instance method or constructor is executing.

### Use case 1: Differentiate instance variables from parameters

Consider:

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {

        this.name = name;
        this.age = age;
    }
}
```

What does this mean?

```java
this.name = name;
```

```text
       this.name = name;
          |         |
          |         |
          |         └── Constructor parameter
          |
          └── Current object's instance variable
```

Without `this`, the parameter name shadows the instance variable.

### Visual example

```text
          CURRENT STUDENT OBJECT
        ┌────────────────────────┐
        │                        │
        │  name = "Rahul"        │◄── this.name
        │                        │
        │  age = 21              │◄── this.age
        │                        │
        └────────────────────────┘
                  ▲
                  |
             this reference
```

### Use case 2: Call another constructor using `this()`

Constructor chaining means calling one constructor from another constructor in the same class.

Example:

```java
class Student {

    String name;
    int age;
    int rollNumber;

    Student() {
        this("Unknown");
        System.out.println("No-argument constructor");
    }

    Student(String name) {
        this(name, 18);
        System.out.println("One-argument constructor");
    }

    Student(String name, int age) {
        this(name, age, 0);
        System.out.println("Two-argument constructor");
    }

    Student(String name, int age, int rollNumber) {

        this.name = name;
        this.age = age;
        this.rollNumber = rollNumber;

        System.out.println("Three-argument constructor");
    }
}
```

Usage:

```java
Student s1 = new Student();
```

Output:

```text
Three-argument constructor
Two-argument constructor
One-argument constructor
No-argument constructor
```

### Constructor chaining flow

```text
        Student()
            |
            | this("Unknown")
            ▼
      Student(String name)
            |
            | this(name, 18)
            ▼
      Student(String name, int age)
            |
            | this(name, age, 0)
            ▼
 Student(String name, int age, int rollNumber)
            |
            ▼
       Initialize fields
```

**Rules of `this()`**

- `this()` calls another constructor in the same class.
- It must be the first statement in the constructor.
- It cannot be used inside a static context.
- Avoid circular constructor calls.

### `this` vs `this()`

| `this` | `this()` |
|---|---|
| Refers to the current object | Calls another constructor in the same class |
| Used to access instance members | Used for constructor chaining |
| Can be used in instance methods and constructors | Can only be used as the first statement in a constructor |

---

# 12. Object Creation and the `new` Keyword

Example:

```java
Student s1 = new Student("Rahul", 21, 101);
```

Let's understand the complete process.

| Step | What happens |
|---|---|
| 1 | JVM ensures the class is initialized when required |
| 2 | Memory is allocated for the new object |
| 3 | Instance fields receive default values |
| 4 | Constructor initialization executes |
| 5 | The resulting object reference is assigned to `s1` |

### Important distinction

- `new` is the object creation operator.
- `Student()` is the constructor invocation.
- `s1` is the reference variable.
- `Student` is the reference type.

If the JVM cannot allocate the required object, an `OutOfMemoryError` may occur. This is an error, not a checked exception.

---

# 13. Important Concepts to Remember

| Concept | Key point |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| Instance variable | Non-static field belonging to an object |
| Reference variable | Holds a reference to an object |
| Constructor | Initializes an object during creation |
| Default constructor | Compiler-generated if no constructor is declared |
| No-argument constructor | Any constructor with zero parameters |
| Parameterized constructor | Accepts parameters |
| Constructor overloading | Multiple constructors with different parameter lists |
| `this` | Refers to the current object |
| `this()` | Calls another constructor in the same class |
| `new` | Creates an object |

---

# 14. Interview Questions

<details>
<summary>1. What is the difference between a class and an object?</summary>

A class is a blueprint that defines fields and methods. An object is an instance created from that class.

For example, `Student` is a class, while `new Student()` creates a Student object.
</details>

<details>
<summary>2. Can we create multiple objects from one class?</summary>

Yes. A single class can be used to create multiple independent objects, each with its own instance-variable values.
</details>

<details>
<summary>3. Is an object the same as a reference variable?</summary>

No. An object is an instance in memory. A reference variable holds a reference that allows us to access that object.
</details>

<details>
<summary>4. Do instance variables get default values?</summary>

Yes. Java initializes instance fields with default values when an object is created, before constructor initialization.
</details>

<details>
<summary>5. Do local variables get default values?</summary>

No. Local variables must be definitely assigned before they are read. Otherwise, the compiler reports an error.
</details>

<details>
<summary>6. What happens if we do not create a constructor?</summary>

If a class declares no constructor, the compiler provides a default no-argument constructor.
</details>

<details>
<summary>7. What happens if we create a parameterized constructor but call the class without arguments?</summary>

The code will fail to compile unless a matching no-argument constructor is also available.

Declaring a parameterized constructor prevents the compiler from automatically providing a default constructor.
</details>

<details>
<summary>8. Can a constructor have a return type?</summary>

No. A constructor has no return type. If you write `void Student()`, it becomes a method named `Student`.
</details>

<details>
<summary>9. Can constructors be overloaded?</summary>

Yes. A class can have multiple constructors with different parameter lists.
</details>

<details>
<summary>10. Can constructors be inherited?</summary>

No. Constructors are not inherited by subclasses. A subclass constructor can invoke a superclass constructor using `super()`.
</details>

<details>
<summary>11. What is the purpose of the this keyword?</summary>

`this` refers to the current object. It is commonly used to distinguish instance variables from parameters and to access current-object members.
</details>

<details>
<summary>12. What is constructor chaining?</summary>

Constructor chaining is the process of invoking one constructor from another. `this()` calls a constructor in the same class, while `super()` calls a superclass constructor.
</details>

<details>
<summary>13. Can we use this inside a static method?</summary>

No. `this` refers to a particular object, while a static method belongs to the class and does not execute with a current object reference.
</details>

<details>
<summary>14. What happens when we write Student s1 = new Student()?</summary>

The statement declares a reference variable, creates a new Student object, invokes its constructor, and assigns the resulting reference to `s1`.
</details>

<details>
<summary>15. Can two reference variables point to the same object?</summary>

Yes. Assigning one reference variable to another copies the reference value. Both variables can then access the same object.
</details>

---

# 15. Practice Questions

Try these without looking at the answers.

1. Create a `Student` class with name, age, roll number, and fee.
2. Create three Student objects and assign different values.
3. Print all instance variables without initializing them manually. Observe their default values.
4. Create a parameterized constructor to initialize all fields.
5. Create multiple overloaded constructors.
6. Use `this` to differentiate parameters from instance variables.
7. Use constructor chaining to avoid repeating initialization logic.
8. Create two reference variables pointing to the same object. Change a field through one reference and read it through the other.
9. Try calling a no-argument constructor after declaring only a parameterized constructor. Understand the compiler error.
10. Explain the difference between an object and its reference variable using a memory diagram.

---

# 16. Quick Revision Checklist

- [ ] Explain OOP in your own words.
- [ ] Explain class vs object.
- [ ] Identify fields, instance variables, and methods.
- [ ] Explain instance-variable default values.
- [ ] Explain why local variables must be initialized.
- [ ] Explain object creation using `new`.
- [ ] Break down `Student s1 = new Student()`.
- [ ] Explain default, no-argument, and parameterized constructors.
- [ ] Explain constructor overloading.
- [ ] Explain `this` and `this()`.
- [ ] Draw the stack and heap reference diagram.
- [ ] Explain how two references can point to the same object.

---

# 17. Further Reading

- [Oracle Java Tutorial — Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html)
- [Oracle Java Tutorial — Using the this Keyword](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
- [GeeksforGeeks — Java Classes and Objects](https://www.geeksforgeeks.org/java/classes-objects-java/)
- [W3Schools — Java OOP Visuals](https://www.w3schools.com/java/java_oop_visuals.asp)

**Next document:** `../02-Object-Memory/object-memory-layout.md` — stack vs heap, reference size, object headers, instance fields, memory alignment, and padding.