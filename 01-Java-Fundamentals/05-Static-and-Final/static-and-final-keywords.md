# Static and Final Keywords in Java

## 1. The `static` Keyword

### What is static?

**Interview definition:**

`static` is a keyword used to declare members that belong to a class rather than to individual objects.

A static member is shared at the class level.

Without `static`, an instance variable or instance method belongs to an individual object.

### Real-world example

Imagine a college with multiple students.

Every student has:
- Their own name.
- Their own roll number.
- The same college name.

Instead of storing a separate college name for every student, we can declare it as a static variable.

```text
                 Student Class
                       |
          +------------+------------+
          |            |            |
       name          rollNo       college
     (instance)    (instance)     (static)
```

## 2. Static Variables vs Instance Variables

### Example

```java
class Student {

    String name;
    int rollNo;

    static String college = "PBG";
}
```

Here:

| Variable | Type | Belongs to |
|---|---|---|
| `name` | Instance variable | Individual object |
| `rollNo` | Instance variable | Individual object |
| `college` | Static variable | Class |

### Creating objects and assigning values

```java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        s1.name = "Rahul";
        s1.rollNo = 101;

        Student s2 = new Student();

        s2.name = "Amit";
        s2.rollNo = 102;

        System.out.println(s1.name);
        System.out.println(s2.name);

        System.out.println(Student.college);
    }
}
```

Output:

```text
Rahul
Amit
PBG
```

### How do we access static variables?

There are two ways:

```java
// Recommended: access through class name
Student.college = "IIT";

// Also possible, but not recommended
s1.college = "IIT";
```

**Best practice:** Access static variables using the class name.

```java
Student.college = "PBG";
```

Why?

Because the variable belongs to the class, not to `s1`.

## 3. Visual Representation — Static vs Instance Variables

Consider:

```java
Student s1 = new Student();
Student s2 = new Student();

s1.name = "Rahul";
s2.name = "Amit";

Student.college = "PBG";
```

Conceptual memory representation:

```text
                 STACK
          +------------------+
          |                  |
          | s1               |
          |  |               |
          |  +----------+    |
          |             |    |
          | s2          |    |
          |  |          |    |
          +--|----------|----+
             |          |
             v          v

                  HEAP
          +-------------------+
          | Student Object 1  |
          |-------------------|
          | name = Rahul      |
          | rollNo = 0        |
          +-------------------+

          +-------------------+
          | Student Object 2  |
          |-------------------|
          | name = Amit       |
          | rollNo = 0        |
          +-------------------+


        CLASS-LEVEL DATA
       +---------------------+
       | Student Class       |
       |---------------------|
       | college = PBG       |
       +---------------------+
```

Important:

- Each object has its own instance fields.
- Static fields belong to the class.
- Static fields are not duplicated inside every object.
- The diagram is conceptual. The Java specification does not prescribe one exact physical memory location for every static field.

### What happens if we change a static variable through one object?

```java
class Student {

    String name;

    static String college = "PBG";
}
```

```java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();
        Student s2 = new Student();

        s1.college = "IIT";

        System.out.println(s1.college);
        System.out.println(s2.college);
        System.out.println(Student.college);
    }
}
```

Output:

```text
IIT
IIT
IIT
```

Why?

All three expressions access the same static variable.

Changing it through one object changes the shared class-level value.

**Remember:** Static does not mean that every object has a copy. It means the member is associated with the class.

## 4. How to Initialize Static Variables

There are three common ways to initialize static variables.

### Method 1: Direct initialization

```java
class Student {

    static String college = "PBG";
}
```

### Method 2: Static block

A static block is used to initialize static data when class initialization takes place.

```java
class Student {

    static String college;

    static {
        college = "PBG";
        System.out.println("Static block executed");
    }
}
```

### Method 3: Assign through the class name

```java
class Student {

    static String college;
}

public class Main {

    public static void main(String[] args) {

        Student.college = "PBG";
    }
}
```

### Static block execution

```java
class Demo {

    static {
        System.out.println("Static block");
    }

    Demo() {
        System.out.println("Constructor");
    }
}

public class Main {

    public static void main(String[] args) {

        System.out.println("Main started");

        Demo d1 = new Demo();
        Demo d2 = new Demo();
    }
}
```

Output:

```text
Main started
Static block
Constructor
Constructor
```

Notice:

- The static block executes once when `Demo` is initialized.
- The constructor executes whenever a new object is created.
- Class initialization happens before the first active use of the class, such as creating an instance or accessing a non-constant static field.

**Important distinction:**

Class loading and class initialization are different operations. A class can be loaded before its static initialization executes.

## 5. Static Methods

A static method belongs to the class rather than to an individual object.

### Example

```java
class Student {

    static String college = "PBG";

    static void displayCollege() {
        System.out.println(college);
    }
}
```

### How to call a static method?

```java
public class Main {

    public static void main(String[] args) {

        Student.displayCollege();
    }
}
```

Output:

```text
PBG
```

We do not need to create a Student object to call this method.

### Static method vs instance method

```java
class Student {

    String name;
    static String college = "PBG";

    void displayName() {
        System.out.println(name);
    }

    static void displayCollege() {
        System.out.println(college);
    }
}
```

| Instance method | Static method |
|---|---|
| Belongs to an object | Belongs to a class |
| Called through an object | Best accessed through class name |
| Can directly access instance variables | Cannot directly access instance variables |
| Can directly access static members | Can access static members |
| Has access to `this` | Does not have `this` |

## 6. Rules of Static Methods

### Rule 1: A static method cannot directly access instance variables

```java
class Student {

    String name;

    static void display() {

        // Compilation error
        // System.out.println(name);
    }
}
```

Why?

An instance variable belongs to an object, but a static method does not automatically have a reference to any particular object.

However, it can access an instance variable through an object:

```java
class Student {

    String name;

    static void display(Student s) {
        System.out.println(s.name);
    }
}
```

### Rule 2: A static method can directly access static variables

```java
class Student {

    static String college = "PBG";

    static void display() {
        System.out.println(college);
    }
}
```

### Rule 3: A static method can call another static method directly

```java
class Demo {

    static void first() {
        System.out.println("First method");
    }

    static void second() {
        first();
        System.out.println("Second method");
    }
}
```

### Rule 4: A static method cannot directly call an instance method

```java
class Demo {

    void display() {
        System.out.println("Instance method");
    }

    static void show() {

        // Compilation error
        // display();
    }
}
```

But it can call an instance method using an object:

```java
static void show() {

    Demo d = new Demo();

    d.display();
}
```

### Rule 5: Static methods cannot use `this` or `super`

```java
class Demo {

    static void display() {

        // Compilation errors
        // this.display();
        // super.toString();
    }
}
```

Why?

`this` refers to the current object. A static method is not associated with a particular object.

### Quick revision

```text
STATIC METHOD
     |
     +-- Can access static variables directly
     |
     +-- Can call static methods directly
     |
     +-- Cannot access instance variables directly
     |
     +-- Cannot call instance methods directly
     |
     +-- Cannot use this or super
     |
     +-- Can access instance members through an object
```

## 7. Why is the main Method Static?

You have already seen this method many times:

```java
public static void main(String[] args) {

}
```

Let's understand why `static` is used.

The JVM needs an entry point to start executing a Java application.

The conventional Java launcher invokes the main method without requiring an instance of the class.

Consider:

```java
class Demo {

    public static void main(String[] args) {
        System.out.println("Application started");
    }
}
```

The launcher can invoke the method without first creating a `Demo` object.

Conceptually:

```text
          Java Launcher
                |
                v
         Demo.main(args)
                |
                v
        Application starts
```

### What if main were not static?

```java
class Demo {

    public void main(String[] args) {
        System.out.println("Application started");
    }
}
```

This is a valid instance method declaration, but it is not the conventional launcher entry point.

We could call it ourselves after creating an object:

```java
Demo d1 = new Demo();

d1.main(args);
```

But the conventional launcher does not need to create a `Demo` object to invoke the standard static entry point.

**Interview answer:**

The main method is static so the Java launcher can invoke it without creating an object of the class.

## 8. Can We Declare Static Classes?

This is an important interview question.

### Top-level static class

A top-level class cannot be declared `static`.

```java
// Invalid Java
static class Student {
}
```

### Static nested class

A class declared inside another class can be static.

```java
class Outer {

    static class Inner {

        void display() {
            System.out.println("Static nested class");
        }
    }
}
```

Usage:

```java
Outer.Inner obj = new Outer.Inner();

obj.display();
```

### Static nested class vs inner class

```java
class Outer {

    int value = 10;

    static class StaticNested {
        void display() {
            // Cannot directly access Outer.value
        }
    }

    class Inner {
        void display() {
            System.out.println(value);
        }
    }
}
```

- A static nested class does not require an instance of the outer class.
- A non-static inner class is associated with an instance of the outer class.

## 9. Can We Declare Static Parameters or Local Variables?

No.

The `static` keyword cannot be applied to local variables or method parameters.

```java
class Demo {

    void display() {

        // Invalid
        // static int number = 10;
    }

    void show(static int number) {
        // Invalid
    }
}
```

Why?

Local variables and parameters belong to a method invocation. They are not class-level members.

## 10. When Should We Use Static?

Use static when the data or behaviour logically belongs to the class rather than a particular object.

Common examples:

| Use case | Example |
|---|---|
| Shared data | College name |
| Utility methods | `Math.max()` |
| Counters shared by objects | Total student count |
| Constants | `Math.PI` |
| Application entry point | `main()` |
| Factory/helper methods | `Integer.valueOf()` |

Example: counting objects.

```java
class Student {

    static int count = 0;

    Student() {
        count++;
    }
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();
        Student s2 = new Student();
        Student s3 = new Student();

        System.out.println(Student.count);
    }
}
```

Output:

```text
3
```

The count is shared across all Student objects.

---

# Part 2 — The `final` Keyword

## 11. What is final?

**Interview definition:**

The `final` keyword restricts modification. Depending on where it is used, it can prevent reassignment of variables, overriding of methods, and inheritance of classes.

There are three major uses:

```text
                 final
                   |
          +--------+--------+
          |        |        |
       Variable  Method   Class
          |        |        |
      Reassign   Override  Extend
      blocked    blocked   blocked
```

## 12. Final Variables

A final variable can be assigned only once.

```java
class Demo {

    final int number = 10;
}
```

This is not allowed:

```java
final int number = 10;

// Compilation error
number = 20;
```

### Constant naming convention

Constants are conventionally written in uppercase letters.

For multiple words, use underscores.

```java
final double PI = 3.14159;

final int MAX_SIZE = 100;

final String COLLEGE_NAME = "PBG";
```

For class-level constants, we commonly use:

```java
static final double PI = 3.14159;
```

Why combine `static` and `final`?

- `static` → One class-level member.
- `final` → Cannot be reassigned after initialization.

### Important: final does not always mean immutable

Consider:

```java
final StringBuilder sb = new StringBuilder("Hello");

// Allowed: modifying the object
sb.append(" World");

// Not allowed: reassigning the reference
// sb = new StringBuilder("Java");
```

Output:

```text
Hello World
```

The reference cannot be reassigned, but the object can still be modified.

**Remember:** `final` reference does not automatically make an object immutable.

## 13. Blank Final Variables

A blank final variable is declared without an initial value and assigned later.

```java
class Student {

    final int rollNo;

    Student(int rollNo) {
        this.rollNo = rollNo;
    }
}
```

The variable must be assigned exactly once before the constructor finishes.

This is useful when a value is available only when an object is created.

## 14. Final Instance Variables vs Final Static Variables

| Type | Meaning |
|---|---|
| `final` instance variable | Each object has its own value, assigned once |
| `static final` variable | Shared class-level value, assigned once |
| `final` local variable | Local variable cannot be reassigned |
| `final` parameter | Parameter cannot be reassigned inside the method |

Example:

```java
class Student {

    final int rollNo;
    static final String COLLEGE = "PBG";

    Student(int rollNo) {
        this.rollNo = rollNo;
    }
}
```

## 15. Final Method

A final method cannot be overridden by a subclass.

```java
class Parent {

    final void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {

    // Compilation error
    // void display() {
    //     System.out.println("Child display");
    // }
}
```

Why use a final method?

When you want subclasses to inherit a method's implementation without replacing it through overriding.

**Note:** A final method can still be overloaded. Final prevents overriding, not creating other methods with different parameter lists.

## 16. Final Class

A final class cannot be extended.

```java
final class Vehicle {

    void start() {
        System.out.println("Vehicle started");
    }
}
```

This is invalid:

```java
// Compilation error
class Car extends Vehicle {
}
```

### Why declare a class final?

- Prevent inheritance.
- Prevent subclasses from changing its behaviour through overriding.
- Support design decisions where extension is not intended.

Example from Java:

```java
public final class String {
    // Implementation
}
```

The `String` class is final, so it cannot be extended.

## 17. Final Method Parameters

The `final` keyword can also be used with method parameters.

```java
class Demo {

    void display(final int number) {

        System.out.println(number);

        // Compilation error
        // number = 20;
    }
}
```

The parameter cannot be reassigned inside the method.

For an object parameter, `final` prevents reassigning the local parameter, but it does not prevent modifying the object's mutable state.

## 18. Static vs Final vs Static Final

| Keyword | Meaning |
|---|---|
| `static` | Belongs to class |
| `final` | Restricts reassignment, overriding or inheritance |
| `static final` | Class-level value that cannot be reassigned |

Example:

```java
class Configuration {

    static String environment = "DEV";

    final int userId = 101;

    static final double PI = 3.14159;
}
```

- `environment` can be changed, and the changed value is shared.
- `userId` cannot be reassigned after initialization.
- `PI` is a shared constant.

## 19. Static Block vs Instance Initialization Block vs Constructor

Java provides different initialization mechanisms.

```java
class Demo {

    static {
        System.out.println("Static block");
    }

    {
        System.out.println("Instance initialization block");
    }

    Demo() {
        System.out.println("Constructor");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Demo d1 = new Demo();
        Demo d2 = new Demo();
    }
}
```

Output:

```text
Static block
Instance initialization block
Constructor
Instance initialization block
Constructor
```

### Comparison

| Feature | Static block | Instance block | Constructor |
|---|---|---|---|
| Runs when | Class initializes | Each object is initialized | Each object is constructed |
| Execution count | Once per class initialization | Once per object creation | Once per constructor invocation |
| Main purpose | Initialize static state | Common instance initialization | Initialize object |
| Can use `this` | No | Yes | Yes |

The instance initialization block executes as part of constructor execution, after the superclass constructor and before the constructor body.

---

# Part 3 — Interview Preparation

## 20. Common Interview Questions

<details>
<summary>1. What is the static keyword in Java?</summary>

`static` declares a class-level member that belongs to the class rather than to individual objects.

</details>

<details>
<summary>2. What is the difference between static and instance variables?</summary>

Static variables are shared at the class level. Instance variables have separate values for each object.

</details>

<details>
<summary>3. Can we access static variables using an object?</summary>

Yes, Java allows it, but accessing static members using the class name is recommended.

</details>

<details>
<summary>4. Can a static method access instance variables?</summary>

Not directly. It needs an object reference to access instance variables.

</details>

<details>
<summary>5. Can a static method call an instance method?</summary>

Yes, through an object. It cannot call an instance method directly without an object reference.

</details>

<details>
<summary>6. Can we use this inside a static method?</summary>

No. `this` refers to the current object, and a static method does not belong to a particular object.

</details>

<details>
<summary>7. Why is main static in Java?</summary>

The Java launcher can invoke the conventional main entry point without creating an instance of the class.

</details>

<details>
<summary>8. Can a top-level class be static?</summary>

No. Only nested classes can be declared static.

</details>

<details>
<summary>9. Can we declare static local variables?</summary>

No. Static is not allowed for local variables or method parameters.

</details>

<details>
<summary>10. How many times does a static block execute?</summary>

It executes once when that class is initialized by a particular class loader.

</details>

<details>
<summary>11. What is the final keyword?</summary>

`final` restricts reassignment of variables, overriding of methods and inheritance of classes.

</details>

<details>
<summary>12. Can a final variable be initialized later?</summary>

Yes. A blank final variable can be initialized later, provided it is assigned exactly once according to Java's initialization rules.

</details>

<details>
<summary>13. Can we override a final method?</summary>

No. A final method cannot be overridden by a subclass.

</details>

<details>
<summary>14. Can we inherit a final class?</summary>

No. A final class cannot be extended.

</details>

<details>
<summary>15. Does final make an object immutable?</summary>

No. Final prevents reassignment of the reference, but the referenced object can still be mutable.

</details>

<details>
<summary>16. What is the difference between static final and final?</summary>

`final` prevents reassignment. `static final` combines class-level sharing with the restriction against reassignment.

</details>

<details>
<summary>17. What is a blank final variable?</summary>

A final variable declared without an initial value and assigned later, commonly through a constructor.

</details>

<details>
<summary>18. What is the difference between a static block and a constructor?</summary>

A static block executes when the class is initialized. A constructor executes when an object is created.

</details>

<details>
<summary>19. Are static variables stored inside objects?</summary>

No. Static variables are class-level data and are not duplicated inside each object. Their exact physical storage is JVM-implementation-dependent.

</details>

<details>
<summary>20. Can we overload a static method?</summary>

Yes. Static methods can be overloaded by using different parameter lists.

</details>

## 21. Practice Tasks

### Beginner

1. Create a `Student` class with `name`, `rollNo` and static `college`.
2. Create three objects and assign different instance values.
3. Change the college using one object and observe the result through other objects.
4. Access a static variable using the class name.
5. Create a static method to display college information.
6. Create a static block and observe when it executes.

### Intermediate

7. Create a static counter that tracks the number of objects created.
8. Create a utility class with static methods for addition, subtraction and multiplication.
9. Create a class with a final instance variable initialized through its constructor.
10. Create a class with a `static final` constant.
11. Try overriding a final method and extending a final class.
12. Create a final method parameter and test reassignment.

### Think like a developer

13. Design a `Configuration` class that stores:
    - Application name.
    - Environment.
    - Maximum connection count.
    - A shared constant for the application version.

14. Decide which fields should be static, final or both.
15. Explain why each field belongs at the class level or object level.

---

## 22. Quick Revision

| Concept | Remember |
|---|---|
| Static variable | Shared class-level variable |
| Instance variable | Separate for each object |
| Static method | Class-level method |
| Static block | Executes during class initialization |
| Static access | Prefer class name |
| `this` in static method | Not allowed |
| Static top-level class | Not allowed |
| Static nested class | Allowed |
| Final variable | Assigned only once |
| Final method | Cannot be overridden |
| Final class | Cannot be extended |
| Final parameter | Cannot be reassigned |
| Static final | Class-level constant pattern |
| Blank final | Initialized later, exactly once |

## 23. References

- [Oracle Java Tutorials — Understanding Class Members](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)
- [Oracle Java Tutorials — Initializing Fields](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)
- [Oracle Java Tutorials — Nested Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)
- [Oracle Java Tutorials — final Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)
- [W3Schools — Java Static](https://www.w3schools.com/java/java_class_methods.asp)
- [W3Schools — Java Modifiers](https://www.w3schools.com/java/java_modifiers.asp)
- [GeeksforGeeks — Static Keyword in Java](https://www.geeksforgeeks.org/static-keyword-java/)
- [GeeksforGeeks — Final Keyword in Java](https://www.geeksforgeeks.org/final-keyword-java/)