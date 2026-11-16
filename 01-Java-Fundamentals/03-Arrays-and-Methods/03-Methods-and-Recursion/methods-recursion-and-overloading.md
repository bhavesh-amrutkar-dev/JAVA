# Methods, Recursion and Method Overloading in Java

> **Interview definition:** A method is a block of code declared inside a class that performs a specific task. It can accept parameters, execute statements and optionally return a value.

## 1. Method Structure

```java
public static int add(int a, int b) {
    return a + b;
}
```

| Part           | Meaning                |
| -------------- | ---------------------- |
| `public`       | Access modifier        |
| `static`       | Belongs to the class   |
| `int`          | Return type            |
| `add`          | Method name            |
| `int a, int b` | Parameters             |
| `return`       | Sends result to caller |

Calling the method:

```java
int result = add(10, 20);
```

- `10` and `20` are arguments.
- `a` and `b` are parameters.
- `result` receives the returned value.

## 2. Four Common Method Categories

These categories are based on whether a method accepts parameters and returns a value.

| Category | Parameters | Return value |
| -------- | ---------- | ------------ |
| 1        | No         | No (`void`)  |
| 2        | Yes        | No (`void`)  |
| 3        | No         | Yes          |
| 4        | Yes        | Yes          |

### 1. No Parameters, No Return Value

```java
static void greet() {
    System.out.println("Hello");
}
```

### 2. Parameters, No Return Value

```java
static void greetUser(String name) {
    System.out.println("Hello " + name);
}
```

### 3. No Parameters, Return Value

```java
static int getAge() {
    return 25;
}
```

### 4. Parameters and Return Value

```java
static int add(int a, int b) {
    return a + b;
}
```

**Remember:** These are common teaching categories, not four separate method types built into Java.

## 3. Method Calling and Chaining

**Interview definition:** Method chaining means calling multiple methods in a single expression, where one method call returns an object or value on which the next method can be invoked.

Example:

```java
String result = "  java programming  "
                    .trim()
                    .toUpperCase()
                    .replace("JAVA", "PYTHON");

System.out.println(result);
```

Conceptual flow:

```text
"  java programming  "
          |
          v
        trim()
          |
          v
  "java programming"
          |
          v
      toUpperCase()
          |
          v
  "JAVA PROGRAMMING"
          |
          v
 replace("JAVA", "PYTHON")
          |
          v
 "PYTHON PROGRAMMING"
```

**Important:** Each method must return a value that supports the next method call. A `void` method cannot be chained in this way.

### Custom Method Chaining

```java
class Calculator {
    private int value;

    Calculator add(int number) {
        value += number;
        return this;
    }

    Calculator multiply(int number) {
        value *= number;
        return this;
    }

    int getResult() {
        return value;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calculator = new Calculator();

        int result = calculator
                .add(10)
                .multiply(2)
                .getResult();

        System.out.println(result); // 20
    }
}
```

- `this` refers to the current object.
- Returning `this` allows another method call on the same object.

## 4. Method Overloading

**Interview definition:** Method overloading allows multiple methods in the same class to have the same name but different parameter lists.

### Three Ways to Overload

**1. Different number of parameters**

```java
static int add(int a, int b) {
    return a + b;
}

static int add(int a, int b, int c) {
    return a + b + c;
}
```

**2. Different parameter types**

```java
static int add(int a, int b) {
    return a + b;
}

static double add(double a, double b) {
    return a + b;
}
```

**3. Different order of parameter types**

```java
static void display(int number, String name) {
    System.out.println(number + " " + name);
}

static void display(String name, int number) {
    System.out.println(name + " " + number);
}
```

### What Can Change in Overloading?

| Change                   | Valid overload? |
| ------------------------ | --------------- |
| Number of parameters     | Yes             |
| Parameter data types     | Yes             |
| Order of parameter types | Yes             |
| Only return type         | No              |
| Only parameter names     | No              |
| Only access modifier     | No              |

**Important:** Overloading is determined by the method signature: method name and parameter types. The return type is not part of the signature used to distinguish overloads.

Invalid example:

```java
static int calculate(int a) {
    return a;
}

// Compilation error: same parameter list
static double calculate(int a) {
    return a * 2.0;
}
```

## 5. How Does Java Choose an Overloaded Method?

Java selects the most appropriate applicable overload based on the arguments and the method invocation conversion rules.

```java
static void show(int value) {
    System.out.println("int");
}

static void show(double value) {
    System.out.println("double");
}

public static void main(String[] args) {
    show(10);   // int
    show(10.5); // double
}
```

**Interview point:** Overload resolution happens at compile time. It is commonly called compile-time polymorphism.

## 6. Method Overloading vs Method Overriding

| Overloading                                    | Overriding                                                          |
| ---------------------------------------------- | ------------------------------------------------------------------- |
| Same method name, different parameter lists    | Subclass provides a new implementation of inherited instance method |
| Usually within the same class                  | Requires inheritance                                                |
| Resolved at compile time                       | Runtime dispatch for overridden instance methods                    |
| Return type alone cannot distinguish overloads | Return type must be same or covariant                               |
| Static methods can be overloaded               | Static methods are hidden, not overridden                           |

## 7. Recursion

**Interview definition:** Recursion is a programming technique in which a method calls itself to solve a smaller version of the same problem.

Every recursive method needs:

1. Base case — condition that stops recursion.
2. Recursive case — method calls itself with a smaller or changed input.

Example: Factorial

```java
static int factorial(int n) {
    if (n == 0 || n == 1) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

Call:

```java
System.out.println(factorial(4)); // 24
```

### Recursion Flow

```text
factorial(4)
     |
     v
4 * factorial(3)
     |
     v
4 * 3 * factorial(2)
     |
     v
4 * 3 * 2 * factorial(1)
     |
     v
4 * 3 * 2 * 1
     |
     v
     24
```

### Stack View

```text
CALLING / STACK GROWTH

factorial(4)
factorial(3)
factorial(2)
factorial(1) → Base case


RETURNING / UNWINDING

factorial(1) = 1
factorial(2) = 2 × 1 = 2
factorial(3) = 3 × 2 = 6
factorial(4) = 4 × 6 = 24
```

**Remember:** Each recursive call creates another method invocation. See [Stack and Heap Memory](../02-Memory-Management/stack-and-heap-memory.md).

## 8. What Happens Without a Base Case?

```java
static void test() {
    test();
}
```

```text
test()
  |
  v
test()
  |
  v
test()
  |
  v
... continues ...
  |
  v
StackOverflowError
```

A missing or unreachable base case can cause unbounded recursion.

## 9. Recursion vs Iteration

| Recursion                                | Iteration                                            |
| ---------------------------------------- | ---------------------------------------------------- |
| Method calls itself                      | Loop repeats statements                              |
| Uses method call frames                  | Uses loop control                                    |
| Can use more stack space                 | Usually uses constant stack space                    |
| Can make tree problems easier to express | Often more memory-efficient                          |
| May cause StackOverflowError             | Can run indefinitely if loop condition never changes |

Example using iteration:

```java
static int factorial(int n) {
    int result = 1;

    for (int i = 1; i <= n; i++) {
        result *= i;
    }

    return result;
}
```

## 10. Variable Scope

**Interview definition:** Scope defines where a variable can be accessed in a program.

### A. Local Variable

Declared inside a method or block.

```java
static void test() {
    int age = 25;
    System.out.println(age);
}
```

Accessible only within its scope.

### B. Parameter Variable

Declared in a method's parameter list.

```java
static void greet(String name) {
    System.out.println(name);
}
```

`name` is available inside the method body.

### C. Block Scope

Declared inside a block `{ }`.

```java
if (true) {
    int number = 10;
    System.out.println(number);
}

// System.out.println(number); // Error
```

### D. Instance Variable

Declared inside a class but outside methods, without `static`.

```java
class Student {
    int age = 25;
}
```

Each object has its own instance field.

### E. Static / Class Variable

Declared using `static`.

```java
class Student {
    static String college = "ABC College";
}
```

The field belongs to the class and is shared by instances of that class.

### Scope Quick Comparison

| Variable  | Declared               | Scope              |
| --------- | ---------------------- | ------------------ |
| Local     | Inside method          | Method/block       |
| Parameter | Method parameter list  | Method body        |
| Instance  | Class, outside methods | Through an object  |
| Static    | Class with `static`    | Class-level access |

**Important:**

- Local variables do not receive automatic default values; they must be definitely assigned before use.
- Instance and static fields receive default values.
- A local variable cannot be accessed outside its block.
- Local variables and parameters cannot be redeclared with conflicting names in overlapping scopes.

## 11. Variable Lifetime vs Scope

- Scope = where the variable can be accessed.
- Lifetime = how long its storage/value exists during execution.
- A local variable's logical lifetime is associated with its method/block execution, though compiler/JIT optimizations can change physical storage.
- An instance field's lifetime is associated with its containing object.
- A static field's lifetime is associated with the class being loaded.

## 12. Interview Questions — Click to Reveal

<details> <summary><strong>1. What is a method in Java?</strong></summary>

A method is a named block of code inside a class that performs a task. It may accept parameters and may return a value.

</details>

<details> <summary><strong>2. What are the four common categories of methods?</strong></summary>

Methods can be grouped by parameters and return values: no parameters/no return value, parameters/no return value, no parameters/return value, and parameters/return value.

</details>

<details> <summary><strong>3. What is method overloading?</strong></summary>

Method overloading means defining multiple methods with the same name but different parameter lists. The compiler selects the appropriate overload based on the invocation.

</details>


<details> <summary><strong>4. Can we overload a method by changing only its return type?</strong></summary>

No. The return type alone does not make a different method signature. The parameter list must differ.

</details>

<details> <summary><strong>5. What are the three common ways to overload a method?</strong></summary>

Change the number of parameters, change their types, or change their order of types.

</details>

<details> <summary><strong>6. What is method chaining?</strong></summary>

Method chaining means invoking multiple methods in one expression, where each method returns a value or object that allows the next call.

</details>

<details> <summary><strong>7. What is recursion?</strong></summary>

Recursion is a technique in which a method calls itself to solve a smaller instance of a problem.

</details>

<details> <summary><strong>8. What are the two essential parts of recursion?</strong></summary>

A base case to stop further calls and a recursive case that moves toward the base case.

</details>

<details> <summary><strong>9. What happens if recursion has no reachable base case?</strong></summary>

The calls can continue until the thread's stack is exhausted, causing `StackOverflowError`.

</details>

<details> <summary><strong>10. What is variable scope?</strong></summary>

Scope is the region of source code where a variable can be accessed.

</details>

<details> <summary><strong>11. Difference between local and instance variables?</strong></summary>

A local variable is declared inside a method or block and is limited to that scope. An instance variable is a field belonging to an object.

</details>

<details> <summary><strong>12. Difference between static and instance variables?</strong></summary>

A static variable belongs to the class and is shared across instances. An instance variable belongs separately to each object.

</details>

<details> <summary><strong>13. Is method overloading compile-time or runtime polymorphism?</strong></summary>

Method overloading is compile-time polymorphism because the compiler resolves the applicable method during compilation.

</details>

<details> <summary><strong>14. What is the difference between a parameter and an argument?</strong></summary>

A parameter is declared in the method definition. An argument is the actual value supplied when the method is called.

</details>

## 13. Quick Revision

| Concept         | One-line reminder                                   |
| --------------- | --------------------------------------------------- |
| Method          | Named block of code inside a class                  |
| Parameter       | Variable in method declaration                      |
| Argument        | Value passed during method call                     |
| `void`          | No return value                                     |
| Method chaining | Multiple calls connected through returned values    |
| Overloading     | Same name, different parameter list                 |
| Overriding      | Subclass changes inherited instance-method behavior |
| Recursion       | Method calls itself                                 |
| Base case       | Stops recursion                                     |
| Stack frame     | Created for method invocation                       |
| Scope           | Where variable can be accessed                      |
| Lifetime        | How long variable/value exists                      |

## 14. Practice Tasks

1. Create one method for each of the four parameter/return categories.
2. Overload `calculateArea()` for a square, rectangle and circle.
3. Create a calculator class supporting method chaining.
4. Write factorial using recursion and iteration.
5. Write a recursive method to calculate the sum of digits.
6. Trace the stack frames for `factorial(5)`.
7. Create a class with local, instance and static variables and compare their behavior.
8. Create a method that accepts an array and returns its maximum element.

## Related Files

- [Arrays in Java](../01-Arrays/arrays-in-java.md)
- [Stack and Heap Memory](../02-Memory-Management/stack-and-heap-memory.md)
