# Polymorphism and Abstraction in Java

## 1. Polymorphism

### What is Polymorphism?

**Interview definition:**

Polymorphism is an OOP concept that allows the same method name or interface to represent different behaviours depending on the arguments supplied or the actual object involved.

The word polymorphism means **many forms**.

### Real-world example

Consider a person who can perform different actions:

- As a student → studies.
- As an employee → works.
- As a player → plays.

The same person can demonstrate different behaviours depending on the context.

### Types of Polymorphism in Java

Java commonly demonstrates two forms:

```text
              Polymorphism
                   |
          +--------+--------+
          |                 |
     Compile-time       Runtime
     Polymorphism       Polymorphism
          |                 |
     Overloading         Overriding
```

## 2. Compile-Time Polymorphism — Method Overloading

Method overloading means defining multiple methods with the same name but different parameter lists within a class.

The compiler determines which overloaded method to call based on the arguments.

### Example 1: Different number of parameters

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {

    public static void main(String[] args) {

        Calculator calc = new Calculator();

        System.out.println(calc.add(10, 20));
        System.out.println(calc.add(10, 20, 30));
    }
}
```

Output:

```text
30
60
```

### Example 2: Different parameter types

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

### Example 3: Different parameter order

```java
class Display {

    void show(int a, String b) {
        System.out.println(a + " " + b);
    }

    void show(String a, int b) {
        System.out.println(a + " " + b);
    }
}
```

### Important rules of overloading

- Method names must be the same.
- Parameter lists must be different.
- Number of parameters can differ.
- Parameter types can differ.
- Parameter order can differ.
- Changing only the return type is not sufficient.
- Changing only the access modifier is not sufficient.

**Invalid overloading:**

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    // Compilation error: same parameter list
    double add(int a, int b) {
        return a + b;
    }
}
```

### Can we overload the main method?

Yes. However, the JVM starts execution using the standard entry-point signature:

```java
public static void main(String[] args)
```

Example:

```java
class Main {

    public static void main(String[] args) {
        System.out.println("Original main");
        main(10);
    }

    static void main(int number) {
        System.out.println(number);
    }
}
```

Output:

```text
Original main
10
```

## 3. Runtime Polymorphism — Method Overriding

Method overriding happens when a subclass provides its own implementation of an inherited instance method.

The method that executes depends on the actual object at runtime.

### Example

```java
class Animal {

    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Animal a1 = new Dog();
        Animal a2 = new Cat();

        a1.sound();
        a2.sound();
    }
}
```

Output:

```text
Dog barks
Cat meows
```

### Understand the reference and object

Consider:

```java
Animal a1 = new Dog();
```

Breakdown:

```text
Animal a1 = new Dog();
   |           |
   |           |
Reference    Actual object
type         created
```

- `Animal` is the reference type.
- `Dog` is the actual object type.
- The reference type determines which members can be accessed at compile time.
- For overridden instance methods, the actual object type determines the implementation executed at runtime.

This is the foundation of runtime polymorphism.

### How does runtime method dispatch work?

```text
Animal a1 = new Dog();
        |
        v
  a1.sound()
        |
        v
 JVM identifies actual object
        |
        v
      Dog
        |
        v
 Dog's sound() executes
```

This mechanism is called **dynamic method dispatch**.

## 4. Rules of Method Overriding

| Rule | Explanation |
|---|---|
| Same method name | Required |
| Same parameter list | Required |
| Return type | Same or covariant |
| Access modifier | Cannot be more restrictive |
| `@Override` | Recommended |
| `final` method | Cannot be overridden |
| `private` method | Cannot be overridden |
| `static` method | Hidden, not overridden |
| Constructor | Cannot be overridden |

### Covariant return type

An overriding method can return a subtype of the original method's return type.

```java
class Animal {
}

class Dog extends Animal {
}

class Parent {

    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {

    @Override
    Dog getAnimal() {
        return new Dog();
    }
}
```

### Static method hiding

Static methods belong to the class rather than being dynamically dispatched based on the object.

```java
class Parent {

    static void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void display() {
        System.out.println("Child");
    }
}
```

```java
Parent p = new Child();
p.display();
```

Output:

```text
Parent
```

The selected static method depends on the reference type, not the runtime object type.

## 5. Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Meaning | Same method name, different parameters | Subclass redefines inherited method |
| Inheritance required | No | Yes |
| Parameters | Must differ | Must match |
| Return type | Can differ if parameters differ | Same or covariant |
| Binding | Resolved at compile time | Runtime dispatch for instance methods |
| Polymorphism | Compile-time | Runtime |

## 6. Upcasting and Runtime Polymorphism

Upcasting is commonly used to achieve runtime polymorphism.

```java
class Payment {

    void pay() {
        System.out.println("Processing payment");
    }
}

class UpiPayment extends Payment {

    @Override
    void pay() {
        System.out.println("Processing UPI payment");
    }
}

class CardPayment extends Payment {

    @Override
    void pay() {
        System.out.println("Processing card payment");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Payment payment;

        payment = new UpiPayment();
        payment.pay();

        payment = new CardPayment();
        payment.pay();
    }
}
```

Output:

```text
Processing UPI payment
Processing card payment
```

### Why is this useful in real projects?

Imagine a payment application supporting:

- UPI
- Credit cards
- Debit cards
- Wallets

Instead of writing separate logic everywhere, we can work with a common `Payment` type.

Each implementation handles its own payment behaviour.

This makes the system easier to extend and maintain.

---

# Part 2 — Abstraction

## 7. What is Abstraction?

**Interview definition:**

Abstraction is an OOP principle that exposes essential functionality while hiding implementation details.

It focuses on **what an object does rather than how it does it**.

### Real-world example

Consider an ATM.

You can:

- Withdraw money.
- Check your balance.
- Deposit money.

You do not need to understand the internal banking infrastructure, database queries, or transaction processing to use it.

```text
          ATM Interface
               |
       +-------+-------+
       |       |       |
    Withdraw Balance Deposit
               |
               v
       Internal Banking
          Implementation
```

In Java, abstraction is primarily achieved using:

1. Abstract classes.
2. Interfaces.

## 8. Abstract Classes

An abstract class is a class declared with the `abstract` keyword.

It can contain:

- Abstract methods.
- Concrete methods.
- Instance variables.
- Constructors.
- Static methods.
- Final methods.

### What is an abstract method?

An abstract method is a method declared without an implementation.

```java
abstract void sound();
```

### Example

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Animal is eating");
    }
}
```

Here:

- `sound()` is an abstract method.
- `eat()` is a concrete method.

### Implementing an abstract class

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Dog d = new Dog();

        d.sound();
        d.eat();
    }
}
```

Output:

```text
Dog barks
Animal is eating
```

### Important rules

- We cannot directly instantiate an abstract class.
- An abstract class can have constructors.
- An abstract class can have both abstract and concrete methods.
- A subclass must implement inherited abstract methods unless the subclass is also abstract.
- An abstract class can have zero abstract methods.

### Why can't we create an abstract class object?

An abstract class may contain methods without implementations. Java prevents direct instantiation because the class may represent an incomplete abstraction.

However, it can be used as a reference type:

```java
Animal a = new Dog();
```

This is valid because the actual object is a concrete `Dog`.

## 9. Interfaces

An interface defines a contract that implementing classes agree to follow.

It is declared using the `interface` keyword.

### Basic syntax

```java
interface Animal {

    void sound();
}
```

### Implementing an interface

```java
class Dog implements Animal {

    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Animal a = new Dog();
        a.sound();
    }
}
```

Output:

```text
Dog barks
```

### Important interface rules

- A class uses `implements` to implement an interface.
- A class can implement multiple interfaces.
- Interface fields are implicitly `public static final`.
- Abstract methods declared without a body are implicitly `public abstract`.
- Implemented interface methods must be public.
- Interfaces cannot be instantiated directly.
- Interfaces can have default and static methods.
- Interfaces can also have private methods (Java 9 onwards) to share implementation among their own methods.
- Interfaces can extend multiple interfaces.

### Multiple interface implementation

```java
interface Camera {
    void takePhoto();
}

interface MusicPlayer {
    void playMusic();
}

class Smartphone implements Camera, MusicPlayer {

    @Override
    public void takePhoto() {
        System.out.println("Taking photo");
    }

    @Override
    public void playMusic() {
        System.out.println("Playing music");
    }
}
```

```text
       Camera       MusicPlayer
          \             /
           \           /
            Smartphone
```

This is how Java supports multiple inheritance of type through interfaces.

## 10. Default and Static Methods in Interfaces

### Default method

A default method has an implementation inside an interface.

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

A class can inherit the default implementation or override it.

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println("Car started");
    }
}
```

### Static method

An interface can also contain static methods.

```java
interface Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call it using the interface name:

```java
int result = Calculator.add(10, 20);
```

Static interface methods are not inherited as instance methods by implementing classes.

## 11. Abstract Class vs Interface

This is one of the most frequently asked Java interview questions.

| Feature | Abstract Class | Interface |
|---|---|---|
| Declaration | `abstract class` | `interface` |
| Inheritance | One class can extend one class | One class can implement multiple interfaces |
| Constructors | Yes | No |
| Instance variables | Yes | No ordinary instance fields |
| Constants | Can have them | Fields are implicitly `public static final` |
| Abstract methods | Supported | Supported |
| Concrete methods | Supported | Default, static and private methods supported |
| Access modifiers | Normal class access rules | Interface methods have interface-specific rules |
| State | Can maintain instance state | Cannot maintain per-object instance state through fields |

### When should we use an abstract class?

Use an abstract class when related classes share:

- Common state.
- Common implementation.
- Constructor logic.
- A common base-class relationship.

Example:

```java
abstract class Employee {

    String name;
    double salary;

    Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    abstract void calculateBonus();

    void display() {
        System.out.println(name + " " + salary);
    }
}
```

### When should we use an interface?

Use an interface when different classes should follow the same contract, even if they are not related through class inheritance.

Example:

```java
interface Printable {
    void print();
}

class Invoice implements Printable {

    @Override
    public void print() {
        System.out.println("Printing invoice");
    }
}

class Report implements Printable {

    @Override
    public void print() {
        System.out.println("Printing report");
    }
}
```

Both classes can be treated as `Printable`, even though they represent different things.

## 12. Combining Abstraction and Polymorphism

This is where both concepts become especially powerful.

Consider a payment system.

```java
interface Payment {

    void pay(double amount);
}
```

Different payment implementations:

```java
class UpiPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid through UPI: " + amount);
    }
}

class CardPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid through Card: " + amount);
    }
}
```

Using the abstraction:

```java
public class Main {

    public static void main(String[] args) {

        Payment payment;

        payment = new UpiPayment();
        payment.pay(1000);

        payment = new CardPayment();
        payment.pay(2000);
    }
}
```

Output:

```text
Paid through UPI: 1000.0
Paid through Card: 2000.0
```

### What is happening?

```text
             Payment Interface
                    |
          +---------+---------+
          |                   |
      UpiPayment          CardPayment
          |                   |
       pay()                pay()
          |                   |
       UPI logic           Card logic
```

- Abstraction: The application depends on the `Payment` contract.
- Polymorphism: Different implementations respond differently to the same `pay()` call.
- Extensibility: We can introduce another payment implementation without changing the interface.

This is closely related to the Strategy Design Pattern, which you will encounter in LLD.

---

# Part 3 — Quick Comparison

## 13. The Four Pillars of OOP

| Pillar | Meaning | Java mechanism |
|---|---|---|
| Encapsulation | Protect and control object state | Classes, access modifiers |
| Inheritance | Reuse and extend behaviour | `extends` |
| Polymorphism | One interface, multiple behaviours | Overloading, overriding |
| Abstraction | Hide implementation details | Abstract classes, interfaces |

## 14. Common Interview Questions

<details>
<summary>1. What is polymorphism?</summary>

Polymorphism means many forms. It allows the same method name or common reference type to represent different behaviours.

</details>

<details>
<summary>2. What are the two types of polymorphism in Java?</summary>

Compile-time polymorphism, commonly achieved through method overloading, and runtime polymorphism, achieved through method overriding and dynamic method dispatch.

</details>

<details>
<summary>3. Can we overload a method by changing only its return type?</summary>

No. The parameter list must differ. Return type alone cannot distinguish overloaded methods.

</details>

<details>
<summary>4. What is dynamic method dispatch?</summary>

It is the runtime mechanism through which Java selects the overridden instance method implementation based on the actual object type.

</details>

<details>
<summary>5. What is the difference between reference type and object type?</summary>

The reference type determines which members can be accessed at compile time. The actual object type determines which overridden instance method implementation runs at runtime.

</details>

<details>
<summary>6. Can static methods be overridden?</summary>

No. Static methods are hidden, not overridden. Their selection is based on the reference or qualifying class type.

</details>

<details>
<summary>7. What is abstraction?</summary>

Abstraction exposes essential functionality while hiding implementation details.

</details>

<details>
<summary>8. Can we create an object of an abstract class?</summary>

No. An abstract class cannot be directly instantiated. We can create an object of a concrete subclass and refer to it using the abstract class type.

</details>

<details>
<summary>9. Can an abstract class have a constructor?</summary>

Yes. Its constructor runs as part of creating an instance of a concrete subclass.

</details>

<details>
<summary>10. Can an abstract class have no abstract methods?</summary>

Yes. A class can be declared abstract even if it contains no abstract methods. This prevents direct instantiation.

</details>

<details>
<summary>11. Can an interface have method implementations?</summary>

Yes. Interfaces can contain default, static and private methods with implementations.

</details>

<details>
<summary>12. Why must an implementing class declare an interface method as public?</summary>

An interface's abstract methods are implicitly public. An implementing method cannot reduce the visibility of the contract.

</details>

<details>
<summary>13. Can a class implement multiple interfaces?</summary>

Yes. A class can implement multiple interfaces using comma-separated interface names.

</details>

<details>
<summary>14. Can an interface extend another interface?</summary>

Yes. An interface can extend one or multiple interfaces.

</details>

<details>
<summary>15. What is the major difference between an abstract class and an interface?</summary>

An abstract class can maintain instance state and provide constructors and shared implementation. An interface primarily defines a contract and allows multiple interface implementation.

</details>

<details>
<summary>16. Can an interface contain a constructor?</summary>

No. Interfaces cannot be instantiated and do not have constructors.

</details>

<details>
<summary>17. Can an abstract class implement an interface?</summary>

Yes. It can implement an interface without implementing all its abstract methods, provided the class itself is declared abstract.

</details>

<details>
<summary>18. What happens if two interfaces provide the same default method?</summary>

If a class implements both interfaces and inherits conflicting default implementations, it must override the conflicting method and resolve the ambiguity.

</details>

<details>
<summary>19. What is the difference between abstraction and encapsulation?</summary>

Abstraction hides implementation complexity and focuses on what an object does. Encapsulation bundles data and methods together and controls access to the object's state.

</details>

<details>
<summary>20. How do abstraction and polymorphism work together?</summary>

Abstraction provides a common contract, while polymorphism allows different implementations to respond differently through that contract.

</details>

# 15. Practice Tasks

### Beginner

1. Create a `Calculator` class with overloaded `add()` methods.
2. Overload `display()` using different parameter types.
3. Create an `Animal` class and override `sound()` in `Dog` and `Cat`.
4. Demonstrate runtime polymorphism using an `Animal` reference.
5. Create an abstract `Shape` class with an abstract `area()` method.
6. Implement `area()` in `Circle` and `Rectangle`.

### Intermediate

7. Create a `Payment` interface with `pay()`.
8. Implement it using `UpiPayment`, `CardPayment` and `WalletPayment`.
9. Create a `Vehicle` interface with a default `stop()` method.
10. Create an abstract `Employee` class with shared fields and an abstract `calculateSalary()` method.
11. Create a class that implements two interfaces.
12. Demonstrate a default-method conflict between two interfaces and resolve it.

### Think like a developer

13. Design a notification system supporting email, SMS and WhatsApp.
14. Make all notification implementations follow a common interface.
15. Add a new notification type without changing the calling code.

---

# 16. Quick Revision

| Topic | Key point |
|---|---|
| Polymorphism | Many forms |
| Overloading | Same name, different parameters |
| Overriding | Subclass changes inherited method behaviour |
| Dynamic dispatch | Runtime method selection |
| `@Override` | Compiler checks overriding intent |
| Abstraction | Hide implementation details |
| Abstract class | Shared state and behaviour |
| Interface | Contract |
| `implements` | Implement interface |
| `default` method | Interface method with implementation |
| Static interface method | Called through interface name |
| Multiple interfaces | Supported |
| Abstract object | Cannot be instantiated directly |

## 17. References

- [Oracle Java Tutorials — Polymorphism](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
- [Oracle Java Tutorials — Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Oracle Java Tutorials — Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
- [W3Schools — Java Polymorphism](https://www.w3schools.com/java/java_polymorphism.asp)
- [W3Schools — Java Abstraction](https://www.w3schools.com/java/java_abstract.asp)
- [GeeksforGeeks — Polymorphism in Java](https://www.geeksforgeeks.org/java/polymorphism-in-java/)
- [GeeksforGeeks — Interfaces in Java](https://www.geeksforgeeks.org/java/interfaces-in-java/)