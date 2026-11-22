# Four Pillars of Object-Oriented Programming (OOP)

## 1. What is Object-Oriented Programming?

**Interview definition:**

Object-Oriented Programming (OOP) is a programming paradigm that organizes software around objects, which combine data (state) and behavior (methods).

OOP helps us structure code into reusable, maintainable, and modular components.

### How humans think about reality

We naturally identify things by their:

- **Properties:** What does the object have?
- **Behaviors:** What can the object do?

For example, consider a car:

| Properties (Data) | Behaviors (Methods) |
|---|---|
| Brand | Start |
| Color | Accelerate |
| Speed | Brake |
| Fuel type | Stop |

In Java:

```java
class Car {
    String brand;
    String color;
    int speed;

    void start() {
        System.out.println("Car started");
    }

    void accelerate() {
        speed += 10;
    }

    void brake() {
        speed = 0;
    }
}
```

OOP models real-world concepts in software. It does not mean every real-world concept must be represented literally as an object.

### Four pillars of OOP

```text
                  OOP
                   |
       +-----------+-----------+
       |           |           |
       |           |           |
  Encapsulation Inheritance Abstraction
                   |
              Polymorphism
```

The four pillars are:

1. Encapsulation
2. Inheritance
3. Abstraction
4. Polymorphism

---

# 2. Encapsulation

## What is Encapsulation?

**Interview definition:**

Encapsulation is the process of bundling data members and methods that operate on that data inside a class, while controlling access to the internal state.

There are two important ideas:

1. Keep related data and behavior together inside a class.
2. Prevent unrestricted access to an object's internal data.

### Example: Bank Account

Imagine a bank account with the following information:

- Account holder name
- Bank name
- IFSC code
- Account balance

Consider this implementation:

```java
class BankAccount {
    public double balance;
}
```

Now create an object:

```java
public class Main {
    public static void main(String[] args) {

        BankAccount account = new BankAccount();

        account.balance = 2000;
        account.balance = -50000;

    }
}
```

### What is the problem?

Because `balance` is public, anyone with access to the object can modify it directly.

```text
          BankAccount Object
                  |
             balance = 2000
                  |
          Anyone can modify it
                  |
          account.balance = -50000
```

This is dangerous because an account balance should follow business rules.

For example:
- A withdrawal should not exceed the available balance.
- A deposit should not accept a negative amount.
- The balance should not be modified arbitrarily.

### Solution: Encapsulation

We can make the balance private and provide controlled methods to access or modify it.

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {

        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        BankAccount account = new BankAccount();

        account.deposit(2000);
        account.withdraw(500);

        System.out.println(account.getBalance());

    }
}
```

Output:

```text
1500.0
```

Now this is not allowed:

```java
account.balance = -50000; // Compilation error
```

Because `balance` is private.

### Visual representation

```text
          BankAccount
     +----------------------+
     |                      |
     |   private balance    |
     |                      |
     |----------------------|
     |                      |
     |   deposit()          |
     |   withdraw()         |
     |   getBalance()       |
     |                      |
     +----------------------+
               |
        Controlled access
               |
          Main Class
```

**Important:** Getters and setters are common ways to provide controlled access, but encapsulation does not require a getter and setter for every field. Methods such as `deposit()` and `withdraw()` are often better because they enforce business rules.

---

## 2.1 Access Modifiers in Java

Access modifiers control where classes, variables, constructors, and methods can be accessed.

Java has four access levels:

1. `private`
2. Default (package-private)
3. `protected`
4. `public`

### Access modifier comparison

| Modifier | Same class | Same package | Subclass outside package | Other package |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| Default | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes* | No |
| `public` | Yes | Yes | Yes | Yes |

*Protected access outside the package is subject to inheritance-related restrictions. A subclass accesses the protected member through inheritance, subject to Java's access rules.*

### 1. Private

Accessible only inside the class where it is declared.

```java
class Student {

    private int age;

    public void setAge(int age) {
        this.age = age;
    }
}
```

### 2. Default (Package-private)

When no access modifier is specified, Java uses package-private access.

Accessible within the same package.

```java
class Student {

    int age;
}
```

Here, `age` has default access.

### 3. Protected

Accessible:
- Within the same package.
- In subclasses outside the package, subject to Java's protected access rules.

```java
class Parent {

    protected void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {

    void show() {
        display();
    }
}
```

### 4. Public

Accessible from anywhere, provided the declaring type itself is accessible.

```java
public class Student {

    public void display() {
        System.out.println("Hello");
    }
}
```

### Access restriction order

```text
Most restricted                     Least restricted

private  →  default  →  protected  →  public
```

---

## 2.2 What is a Package in Java?

**Interview definition:**

A package is a namespace that groups related classes, interfaces, and other types together.

Packages help organize code, avoid naming conflicts, and manage access.

### Types of packages

**1. Built-in packages**

Provided by the Java platform.

Examples:

| Package | Purpose |
|---|---|
| `java.lang` | Fundamental Java classes |
| `java.util` | Collections and utility classes |
| `java.io` | Input/output operations |
| `java.net` | Networking |
| `java.time` | Date and time API |

`java.lang` is imported automatically. Other packages generally need explicit imports.

**2. User-defined packages**

Created by developers to organize their own code.

Example:

```java
package com.example.bank;

public class BankAccount {
}
```

A class from another package can be imported:

```java
import com.example.bank.BankAccount;
```

### Can a class be private or protected?

A top-level class can have only:

- `public`
- Default (package-private)

A top-level class cannot be declared `private` or `protected`.

However, nested classes can use `private`, `protected`, `public`, or default access.

---

# 3. Inheritance

## What is Inheritance?

**Interview definition:**

Inheritance is an OOP mechanism in which one class acquires accessible properties and behaviors from another class.

It promotes code reuse and supports runtime polymorphism through method overriding.

### Parent and Child classes

- Parent class = Superclass
- Child class = Subclass

Java uses the `extends` keyword for class inheritance.

```java
class Animal {

    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Dog is barking");
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        Dog dog = new Dog();

        dog.eat();  // Inherited method
        dog.bark(); // Own method

    }
}
```

The `Dog` class can use the accessible `eat()` method inherited from `Animal`.

### Visual representation

```text
          Animal
             |
          extends
             |
            Dog
             |
      +------+------+
      |             |
     eat()         bark()
   inherited       own method
```

## 3.1 Types of Inheritance

### 1. Single Inheritance

One child class inherits from one parent class.

```text
       A
       |
       B
```

```java
class A {
}

class B extends A {
}
```

### 2. Multilevel Inheritance

A class inherits from another child class, creating a chain.

```text
       A
       |
       B
       |
       C
```

```java
class A {
}

class B extends A {
}

class C extends B {
}
```

### 3. Hierarchical Inheritance

Multiple child classes inherit from the same parent class.

```text
         A
       /   \
      B     C
```

```java
class A {
}

class B extends A {
}

class C extends A {
}
```

### 4. Multiple Inheritance

One class inherits from more than one parent class.

```text
       A       B
        \     /
          C
```

Java does not support multiple inheritance through classes.

This is invalid:

```java
class A {
}

class B {
}

// Not allowed in Java
class C extends A, B {
}
```

However, Java supports implementing multiple interfaces:

```java
interface Camera {
    void clickPhoto();
}

interface MusicPlayer {
    void playMusic();
}

class Smartphone implements Camera, MusicPlayer {

    public void clickPhoto() {
        System.out.println("Photo clicked");
    }

    public void playMusic() {
        System.out.println("Music playing");
    }
}
```

A class can implement multiple interfaces.

---

## 3.2 Why Does Java Not Support Multiple Class Inheritance?

One important reason is ambiguity in cases such as the **Diamond Problem**.

### What is the Diamond Problem?

Imagine two parent classes inherit from the same base class, and a child class inherits from both.

Both parent classes provide a method with the same name.

```text
             A
          show()
           /  \
          /    \
         B      C
       show()  show()
          \    /
           \  /
             D
```

Now imagine:

```java
class A {

    void show() {
        System.out.println("A");
    }
}
```

```java
class B extends A {

    void show() {
        System.out.println("B");
    }
}
```

```java
class C extends A {

    void show() {
        System.out.println("C");
    }
}
```

If Java allowed:

```java
class D extends B, C {
}
```

Then what should happen here?

```java
D obj = new D();
obj.show();
```

Should Java execute `B.show()` or `C.show()`?

This creates ambiguity unless the language defines a clear resolution mechanism.

### How do interfaces help?

Java allows multiple interface implementation.

```java
interface A {
    void show();
}

interface B {
    void show();
}

class C implements A, B {

    public void show() {
        System.out.println("C implementation");
    }
}
```

The implementing class supplies the method implementation.

**Important:** Interfaces avoid the classic multiple-class-inheritance problem, but default methods can still create conflicts. Java requires the class to resolve conflicting inherited default methods explicitly.

---

## 3.3 The `super` Keyword

**Interview definition:**

The `super` keyword is used in a subclass to refer to superclass members and invoke superclass constructors.

It has three major uses.

### 1. Access superclass variables

```java
class Parent {

    int number = 10;
}

class Child extends Parent {

    int number = 20;

    void display() {

        System.out.println(number);
        System.out.println(super.number);
    }
}
```

Output:

```text
20
10
```

- `number` refers to the current class variable.
- `super.number` refers to the superclass variable.

### 2. Call superclass methods

```java
class Parent {

    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {

    void display() {

        super.display();

        System.out.println("Child method");
    }
}
```

Output:

```text
Parent method
Child method
```

### 3. Call superclass constructor

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {

        super();

        System.out.println("Child constructor");
    }
}
```

Output:

```text
Parent constructor
Child constructor
```

**Important rules:**

- `super()` calls a superclass constructor.
- A superclass constructor executes before the subclass constructor body.
- An explicit `super()` call must be the first statement in a constructor.
- If no explicit constructor invocation is written, Java generally inserts an implicit `super()` when possible.
- `this()` and `super()` cannot both be used as explicit constructor invocations in the same constructor.

### `this` vs `super`

| `this` | `super` |
|---|---|
| Refers to the current object | Refers to the superclass portion of the current object |
| Accesses current class members | Accesses superclass members |
| `this()` calls another constructor in the same class | `super()` calls a superclass constructor |

**Important clarification:** `super` does not refer to a separate parent object. It provides access to the superclass part of the current object.

---

# 4. Abstraction

## What is Abstraction?

**Interview definition:**

Abstraction is the process of exposing essential functionality while hiding unnecessary implementation details.

In simple terms:

**Focus on what an object does rather than how it does it.**

### Real-world example: Driving a car

When driving a car, we use:

- Accelerator
- Brake
- Steering wheel
- Gear system

We don't need to understand the internal engine combustion process to drive the car.

```text
       User
        |
        |
   +----------+
   | Car      |
   |----------|
   | start()  |
   | brake()  |
   | drive()  |
   +----------+
        |
        |
   Internal implementation
   Hidden from the user
```

The user interacts with a simple interface without needing to understand every internal detail.

## 4.1 Understanding Abstraction with Code

### Example 1: Concrete class

Consider a simple car class:

```java
class Car {

    void start() {
        System.out.println("Engine started");
    }

    void accelerate() {
        System.out.println("Car accelerating");
    }

    void brake() {
        System.out.println("Car stopped");
    }
}
```

Usage:

```java
Car car = new Car();

car.start();
car.accelerate();
car.brake();
```

The user calls the methods without knowing their internal implementation.

However, the implementation and the exposed functionality are defined together in the concrete class.

### Example 2: Multiple car implementations

Imagine we have two types of cars:

```java
class FuelCar {

    void drive() {
        System.out.println("Driving using fuel engine");
    }
}
```

```java
class ElectricCar {

    void drive() {
        System.out.println("Driving using electric motor");
    }
}
```

Usage:

```java
FuelCar fuelCar = new FuelCar();
ElectricCar electricCar = new ElectricCar();

fuelCar.drive();
electricCar.drive();
```

### What is the limitation?

The calling code needs to know the specific implementation type.

If we add a DieselCar, HybridCar, or another type, we may need to modify the calling code to handle each new class.

We can reduce this coupling using a common abstraction.

## 4.2 High-level abstraction using interfaces

An interface defines a common contract.

```java
interface Car {

    void drive();
}
```

Implementations:

```java
class FuelCar implements Car {

    public void drive() {
        System.out.println("Driving using fuel engine");
    }
}
```

```java
class ElectricCar implements Car {

    public void drive() {
        System.out.println("Driving using electric motor");
    }
}
```

Now the calling code depends on the interface:

```java
public class Main {

    public static void main(String[] args) {

        Car car = new ElectricCar();

        car.drive();

    }
}
```

Output:

```text
Driving using electric motor
```

We can change the implementation without changing the way the calling code interacts with it:

```java
Car car = new FuelCar();
car.drive();
```

### Visual representation

```text
             Car Interface
              drive()
                 |
         +-------+-------+
         |               |
      FuelCar         ElectricCar
      drive()           drive()
         |               |
    Fuel engine      Electric motor
```

The calling code depends on `Car`, not on a specific implementation.

This improves:
- Loose coupling
- Extensibility
- Maintainability
- Testability

**Important:** An interface is not the only way to achieve abstraction. Abstract classes also provide abstraction.

---

## 4.3 Abstract Classes

An abstract class is a class declared using the `abstract` keyword. It cannot be instantiated directly.

It can contain:
- Abstract methods (methods without a body).
- Concrete methods (methods with a body).
- Instance variables.
- Constructors.
- Static methods.

Example:

```java
abstract class Vehicle {

    abstract void start();

    void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

A concrete subclass must implement the inherited abstract methods:

```java
class Car extends Vehicle {

    @Override
    void start() {
        System.out.println("Car started");
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        // Vehicle vehicle = new Vehicle(); // Not allowed

        Vehicle vehicle = new Car();

        vehicle.start();
        vehicle.stop();

    }
}
```

Output:

```text
Car started
Vehicle stopped
```

### What is an abstract method?

An abstract method has a declaration but no implementation.

```java
abstract void start();
```

A concrete subclass must implement it unless that subclass is also abstract.

### Why use abstract classes?

Use an abstract class when multiple related classes share:
- Common state.
- Common behavior.
- A common base type.
- Some behavior that subclasses must implement.

---

## 4.4 Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---|---|---|
| Declaration | `abstract class` | `interface` |
| Object creation | Cannot instantiate directly | Cannot instantiate directly |
| Constructors | Yes | No constructors |
| Instance variables | Yes | No ordinary instance fields |
| Fields | Instance and static fields | Fields are implicitly `public static final` |
| Abstract methods | Yes | Yes, implicitly abstract unless otherwise specified |
| Concrete methods | Yes | Yes, using default, static, or private methods |
| Inheritance | A class can extend one class | A class can implement multiple interfaces |
| Main purpose | Shared base and common state/behavior | Define a contract or capability |

Example of implementing multiple interfaces:

```java
interface Printable {
    void print();
}

interface Scannable {
    void scan();
}

class Printer implements Printable, Scannable {

    public void print() {
        System.out.println("Printing...");
    }

    public void scan() {
        System.out.println("Scanning...");
    }
}
```

### When should you use each?

**Abstract class:**
- When classes are closely related.
- When they share common state.
- When you want to provide common implementation.

**Interface:**
- When you want to define a common contract.
- When unrelated classes need to support the same capability.
- When a class needs to implement multiple types.

---

## 4.5 Low-level and High-level Abstraction

Your lecture uses the terms low-level and high-level abstraction to explain the difference between implementation hiding and separating a contract from its implementation.

A useful way to understand this distinction:

| Concept | Meaning |
|---|---|
| Implementation hiding | Hide internal details behind methods |
| Contract-based abstraction | Define what an implementation must do without tying callers to a particular implementation |

A concrete class can hide internal implementation details through private methods and public methods.

An interface or abstract class can provide a stronger separation between the contract and its implementation.

**Important:** "Low-level abstraction" and "high-level abstraction" are explanatory terms here, not two official Java language constructs. Inheritance by itself does not automatically create abstraction or loose coupling. A shared parent type can help, but interfaces and abstract classes are common ways to define an explicit abstraction.

---

# 5. Polymorphism

## What is Polymorphism?

**Interview definition:**

Polymorphism means one interface or reference type can represent different underlying implementations, allowing the same operation to behave differently depending on the object.

The word polymorphism means **many forms**.

Java supports two commonly discussed forms:

1. Compile-time polymorphism — Method overloading.
2. Runtime polymorphism — Method overriding.

## 5.1 Compile-time Polymorphism (Method Overloading)

Method overloading means defining multiple methods with the same name but different parameter lists.

Example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

Usage:

```java
Calculator calculator = new Calculator();

System.out.println(calculator.add(10, 20));
System.out.println(calculator.add(10, 20, 30));
System.out.println(calculator.add(10.5, 20.5));
```

The compiler determines which overloaded method to call based on the arguments and overload resolution rules.

**Important:** Changing only the return type does not create a valid overload.

## 5.2 Runtime Polymorphism (Method Overriding)

Method overriding happens when a subclass provides its own implementation of an inherited instance method.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal makes a sound");
    }
}
```

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

```java
class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        Animal animal;

        animal = new Dog();
        animal.sound();

        animal = new Cat();
        animal.sound();

    }
}
```

Output:

```text
Dog barks
Cat meows
```

### Visual representation

```text
             Animal
             sound()
                |
         +------+------+
         |             |
        Dog           Cat
      sound()        sound()
         |             |
       Barks          Meows
```

The reference type is `Animal`, but the actual object determines which overridden instance method executes.

This is called **dynamic method dispatch**.

### Reference type vs Object type

```java
Animal animal = new Dog();
```

- Reference type: `Animal`
- Actual object type: `Dog`

The reference type determines which members are accessible at compile time.

The actual object's class determines which overridden instance method implementation executes at runtime.

### Important rules of overriding

- The method name and parameter list must match.
- The overriding method cannot reduce visibility.
- The return type must be the same or covariant.
- `@Override` is recommended.
- `final` methods cannot be overridden.
- `private` methods are not overridden.
- Static methods are hidden, not overridden.

## 5.3 Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Meaning | Same method name, different parameters | Subclass redefines inherited method |
| Inheritance required? | No | Yes |
| Parameters | Must differ | Must match |
| Return type | Can differ if parameters differ | Same or covariant |
| Binding | Compile time | Runtime for overridden instance methods |
| Purpose | Different ways to perform an operation | Different behavior for different implementations |

---

# 6. How the Four Pillars Work Together

Consider a payment processing system.

We want to support:
- Credit card payments.
- UPI payments.
- Net banking payments.

### Step 1: Abstraction

Define a common contract:

```java
interface Payment {

    void pay(double amount);
}
```

### Step 2: Inheritance / Implementation

Different payment classes implement the same interface:

```java
class UpiPayment implements Payment {

    public void pay(double amount) {
        System.out.println("Paid using UPI: " + amount);
    }
}
```

```java
class CardPayment implements Payment {

    public void pay(double amount) {
        System.out.println("Paid using Card: " + amount);
    }
}
```

### Step 3: Encapsulation

Each implementation can hide its internal data and logic.

```java
class UpiPayment implements Payment {

    private String upiId;

    UpiPayment(String upiId) {
        this.upiId = upiId;
    }

    public void pay(double amount) {
        System.out.println("Processing UPI payment for " + amount);
    }
}
```

### Step 4: Polymorphism

The same interface reference can represent different payment implementations.

```java
public class Main {

    public static void main(String[] args) {

        Payment payment = new UpiPayment("user@upi");

        payment.pay(1000);

        payment = new CardPayment();

        payment.pay(2000);
    }
}
```

### Overall architecture

```text
                  Payment
                 Interface
                 pay(amount)
                     |
           +---------+---------+
           |                   |
       UpiPayment          CardPayment
           |                   |
       pay()                pay()
           |                   |
      UPI logic            Card logic
```

The benefits:

| Pillar | Role in the payment system |
|---|---|
| Encapsulation | Protects internal payment data and implementation |
| Inheritance / Implementation | Establishes a shared type and common contract |
| Abstraction | Exposes the `pay()` operation without exposing its internal process |
| Polymorphism | Allows different payment implementations through one reference type |

**Real-world takeaway:** When adding a new payment method, we can create another implementation of `Payment` without changing code that already depends on the interface, provided it is designed to use that contract.

---

# 7. Important Interview Questions

<details>
<summary>1. What are the four pillars of OOP?</summary>

The four pillars are encapsulation, inheritance, abstraction, and polymorphism.

- Encapsulation: Bundles data and behavior and controls access.
- Inheritance: Allows a class to inherit from another class.
- Abstraction: Hides unnecessary implementation details.
- Polymorphism: Allows one common type or operation to represent different behaviors.

</details>

<details>
<summary>2. What is the difference between encapsulation and abstraction?</summary>

Encapsulation focuses on bundling data and behavior together and controlling access to internal state.

Abstraction focuses on exposing essential functionality while hiding implementation details.

For example, a BankAccount class uses encapsulation to protect its balance, while a Payment interface provides abstraction through a common `pay()` method.

</details>

<details>
<summary>3. Why do we make variables private?</summary>

We make variables private to prevent direct external access and protect the object's state.

We can expose controlled operations through public methods that validate input and enforce business rules.

</details>

<details>
<summary>4. Are getters and setters mandatory for encapsulation?</summary>

No. Getters and setters are common techniques, but they are not mandatory.

Methods such as `deposit()` and `withdraw()` can provide controlled access while enforcing business rules.

</details>

<details>
<summary>5. What are the four access levels in Java?</summary>

- `private`
- Default (package-private)
- `protected`
- `public`

They control accessibility at different scopes.

</details>

<details>
<summary>6. Why does Java not support multiple inheritance through classes?</summary>

Multiple class inheritance can introduce ambiguity when two parent classes provide the same method implementation.

This is commonly explained through the Diamond Problem.

Java supports implementing multiple interfaces instead.

</details>

<details>
<summary>7. What is the Diamond Problem?</summary>

The Diamond Problem is an ambiguity that can occur when a class inherits from two parent classes that both inherit from the same base class and provide conflicting implementations.

The child class may not have an obvious implementation to inherit.

</details>

<details>
<summary>8. What is the difference between this and super?</summary>

`this` refers to the current object and can access current class members.

`super` provides access to superclass members and constructors.

</details>

<details>
<summary>9. Can we create an object of an abstract class?</summary>

No. An abstract class cannot be instantiated directly.

However, we can create an object of a concrete subclass and store its reference in an abstract class reference.

</details>

<details>
<summary>10. Can an abstract class have a constructor?</summary>

Yes. An abstract class can have constructors. They execute as part of creating an instance of a concrete subclass.

</details>

<details>
<summary>11. What is the difference between an abstract class and an interface?</summary>

An abstract class can contain instance state, constructors, abstract methods, and concrete methods.

An interface defines a contract and supports multiple interface implementation. It can also contain default, static, and private methods.

</details>

<details>
<summary>12. What is polymorphism?</summary>

Polymorphism means one common type or operation can represent multiple forms of behavior.

Java commonly demonstrates this through method overloading and method overriding.

</details>

<details>
<summary>13. What is dynamic method dispatch?</summary>

Dynamic method dispatch is the runtime mechanism through which Java selects the overridden instance method based on the actual object's class.

</details>

<details>
<summary>14. Can static methods be overridden?</summary>

No. Static methods belong to the class rather than being dynamically dispatched through an object.

A subclass can declare a static method with the same signature, which is called method hiding.

</details>

<details>
<summary>15. Can a class implement multiple interfaces?</summary>

Yes. Java allows a class to implement multiple interfaces.

This provides multiple inheritance of type without multiple inheritance of class implementations.

</details>

<details>
<summary>16. Does inheritance always reduce code duplication?</summary>

No. Inheritance can promote reuse, but incorrect inheritance can create tight coupling and fragile class hierarchies.

Composition is often a better choice when there is no genuine "is-a" relationship.

</details>

---

# 8. Quick Revision

| Pillar | Main idea | Java mechanism |
|---|---|---|
| Encapsulation | Bundle data and behavior; control access | Classes, access modifiers |
| Inheritance | Reuse and extend behavior | `extends`, `implements` |
| Abstraction | Hide implementation details behind a contract | Abstract classes, interfaces |
| Polymorphism | Different implementations through a common type | Overloading, overriding |

## Important Keywords

| Keyword | Purpose |
|---|---|
| `private` | Restricts access to the declaring class |
| `protected` | Allows package and qualifying subclass access |
| `public` | Allows access wherever the declaring type is accessible |
| `extends` | Inherits from a class or extends an interface |
| `implements` | Implements an interface |
| `super` | Accesses superclass members or invokes its constructor |
| `this` | Refers to the current object |
| `abstract` | Declares an abstract class or method |
| `interface` | Declares an interface |
| `@Override` | Indicates that a method is intended to override a superclass method |

---

# 9. Practice Exercises

Try implementing these without looking at the solutions.

### Beginner

1. Create a `BankAccount` class with a private balance.
2. Add `deposit()`, `withdraw()`, and `getBalance()` methods.
3. Create an `Animal` parent class and `Dog` and `Cat` child classes.
4. Demonstrate the use of `this` and `super`.
5. Create an abstract `Vehicle` class with an abstract `start()` method.

### Intermediate

6. Create a `Payment` interface and implement it using `UPIPayment`, `CardPayment`, and `CashPayment`.
7. Demonstrate runtime polymorphism using an array of `Payment` references.
8. Create an example of method overloading.
9. Demonstrate the Diamond Problem concept using interfaces with default methods.
10. Design a notification system with Email, SMS, and WhatsApp implementations.

### Think like a developer

11. Why should a payment service depend on a `Payment` interface rather than directly depending on `UPIPayment`?
12. What happens if we make the BankAccount balance public?
13. When would composition be better than inheritance?
14. How would you add a new payment method without modifying the existing payment-processing logic?

---

# 10. Final Checklist

- [ ] Explain the four pillars of OOP.
- [ ] Explain encapsulation using a BankAccount example.
- [ ] Understand all four access modifiers.
- [ ] Understand Java packages.
- [ ] Explain the types of inheritance.
- [ ] Understand why multiple class inheritance is not supported.
- [ ] Explain the Diamond Problem.
- [ ] Understand all three uses of `super`.
- [ ] Explain abstraction with a real-world example.
- [ ] Understand abstract classes and interfaces.
- [ ] Differentiate abstraction and encapsulation.
- [ ] Explain compile-time and runtime polymorphism.
- [ ] Understand method overloading and overriding.
- [ ] Build a small payment-processing example using all four pillars.

## Recommended References

- [Oracle Java Tutorials — Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/)
- [Oracle Java Tutorials — Controlling Access to Members](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
- [Oracle Java Tutorials — Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
- [Oracle Java Tutorials — Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Oracle Java Tutorials — Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)

---

**Core takeaway:** OOP is not just about creating classes and objects. It is about designing software where data is protected, responsibilities are organized, implementations can be extended, and components can interact through well-defined contracts.