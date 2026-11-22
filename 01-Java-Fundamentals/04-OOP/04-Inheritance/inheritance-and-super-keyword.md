# Inheritance in Java

## 1. What is Inheritance?

**Interview definition:**

Inheritance is an OOP mechanism in which one class acquires the accessible fields and methods of another class, allowing code reuse and establishing an `is-a` relationship.

- Parent class → Superclass
- Child class → Subclass
- Keyword → `extends`

### Real-world example

Think about a general `Person`.

A `Student` is a person, and a `Teacher` is also a person.

Both can share common properties such as `name` and `age`, while having their own specific properties.

```text
                 Person
                /      \
               /        \
          Student      Teacher
```

Instead of writing common properties in every class, we can define them once in the parent class.

## 2. How to Implement Inheritance

### Parent class

```java
class Person {
    String name;
    int age;

    void introduce() {
        System.out.println("My name is " + name);
    }
}
```

### Child class

```java
class Student extends Person {
    int rollNumber;

    void study() {
        System.out.println("Student is studying");
    }
}
```

### Main class

```java
public class Main {
    public static void main(String[] args) {

        Student s1 = new Student();

        s1.name = "Rahul";       // Inherited field
        s1.age = 21;             // Inherited field
        s1.rollNumber = 101;     // Student's own field

        s1.introduce();          // Inherited method
        s1.study();              // Student's own method
    }
}
```

**Output:**

```text
My name is Rahul
Student is studying
```

### What happens here?

```text
Person (Superclass)
-------------------
name
age
introduce()

        |
        | extends
        v

Student (Subclass)
-------------------
name       <- inherited
age        <- inherited
introduce  <- inherited

rollNumber <- own field
study()    <- own method
```

Important: The subclass object contains the inherited object state as well as its own state. Private superclass members are not directly accessible by the subclass, but they are still part of the superclass portion of the object.

## 3. Why Do We Need Inheritance?

| Without inheritance | With inheritance |
|---|---|
| Repeated code | Reusable code |
| Common logic duplicated | Common logic in parent |
| Changes need multiple updates | Common changes in one place |
| Harder to maintain | Easier to maintain |

### Example without inheritance

```java
class Student {
    String name;
    int age;
}

class Teacher {
    String name;
    int age;
}
```

### Example with inheritance

```java
class Person {
    String name;
    int age;
}

class Student extends Person {
    int rollNumber;
}

class Teacher extends Person {
    String subject;
}
```

We have moved the common properties into `Person`.

**Remember:** Do not use inheritance only to avoid writing duplicate code. The relationship should genuinely represent an `is-a` relationship.

## 4. Types of Inheritance in Java

### 4.1 Single Inheritance

One child class inherits from one parent class.

```text
    A
    |
    B
```

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}
```

### 4.2 Multilevel Inheritance

A class inherits from a class that itself inherits from another class.

```text
    A
    |
    B
    |
    C
```

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}

class Puppy extends Dog {
    void weep() {
        System.out.println("Weeping");
    }
}
```

A `Puppy` can access accessible methods from both `Dog` and `Animal`.

### 4.3 Hierarchical Inheritance

Multiple child classes inherit from the same parent class.

```text
       Animal
       /    \
      /      \
    Dog      Cat
```

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}

class Cat extends Animal {
    void meow() {
        System.out.println("Meowing");
    }
}
```

### 4.4 Multiple Inheritance

One class inherits from multiple parent classes.

```text
    A       B
     \     /
      \   /
        C
```

Java does not support multiple inheritance through classes.

```java
// Not valid Java

class A {
}

class B {
}

// Compilation error
class C extends A, B {
}
```

**Why does Java avoid multiple inheritance of classes?**

One important reason is ambiguity, commonly illustrated by the diamond problem.

```text
        A
       / \
      B   C
       \ /
        D
```

Suppose both `B` and `C` inherit a method from `A` and provide their own implementations. Which implementation should `D` inherit?

Java avoids this ambiguity by allowing a class to extend only one class.

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
        System.out.println("Taking photo");
    }

    public void playMusic() {
        System.out.println("Playing music");
    }
}
```

**Important distinction:**

- Multiple inheritance of classes: Not supported.
- Multiple inheritance of interfaces: Supported.
- A class can extend one class and implement multiple interfaces.

## 5. Understanding the `super` Keyword

The `super` keyword refers to the superclass portion of the current object.

It is commonly used in three ways:

1. Accessing a superclass variable.
2. Calling a superclass method.
3. Calling a superclass constructor.

### 5.1 Accessing a Parent Class Variable

What if both parent and child classes have a variable with the same name?

```java
class Parent {
    String name = "Parent";
}

class Child extends Parent {
    String name = "Child";

    void display() {
        System.out.println(name);
        System.out.println(super.name);
    }
}
```

Output:

```text
Child
Parent
```

- `name` → Current class variable.
- `super.name` → Superclass variable.

This is called **variable hiding**.

### 5.2 Calling a Parent Class Method

What if the child class overrides a parent method?

```java
class Parent {
    void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {

    @Override
    void display() {
        System.out.println("Child display");
    }

    void show() {
        display();
        super.display();
    }
}
```

Output:

```text
Child display
Parent display
```

- `display()` → Calls the current class implementation.
- `super.display()` → Calls the superclass implementation.

### 5.3 Calling a Parent Class Constructor

The `super()` call invokes a superclass constructor.

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

Why does the parent constructor execute first?

Because the superclass portion of the object must be initialized before the subclass constructor completes its initialization.

## 6. Understanding `super()` and `this()`

Both are used for constructor chaining, but they serve different purposes.

| Keyword | Purpose |
|---|---|
| `this()` | Calls another constructor in the same class |
| `super()` | Calls a constructor of the immediate parent class |

### Example: `this()`

```java
class Student {

    Student() {
        this("Unknown");
        System.out.println("Default constructor");
    }

    Student(String name) {
        System.out.println("Name: " + name);
    }
}
```

Output:

```text
Name: Unknown
Default constructor
```

### Example: `super()`

```java
class Person {

    Person(String name) {
        System.out.println("Person: " + name);
    }
}

class Student extends Person {

    Student() {
        super("Rahul");
        System.out.println("Student constructor");
    }
}
```

Output:

```text
Person: Rahul
Student constructor
```

### Important constructor rules

- `this()` and `super()` must be the first statement in a constructor.
- You cannot use both directly in the same constructor because both need to be first.
- If you do not explicitly write `this()` or `super()`, the compiler inserts `super()` when applicable.
- The implicit `super()` requires an accessible no-argument constructor in the parent class.
- If the parent has no accessible no-argument constructor, you must explicitly invoke an appropriate parent constructor.

## 7. Constructor Execution Order

Consider this hierarchy:

```text
       Object
          |
        Animal
          |
         Dog
```

When we create:

```java
Dog d = new Dog();
```

The constructor execution order is:

```text
Object constructor
        |
        v
Animal constructor
        |
        v
Dog constructor
```

Example:

```java
class Animal {

    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {

    Dog() {
        System.out.println("Dog constructor");
    }
}

public class Main {

    public static void main(String[] args) {
        Dog d = new Dog();
    }
}
```

Output:

```text
Animal constructor
Dog constructor
```

Every Java class ultimately inherits from `Object`, unless it explicitly extends another class.

## 8. The Object Class

`Object` is the root superclass of Java's class hierarchy.

Every class directly or indirectly inherits from it.

```text
              Object
             /      \
         Person     Animal
           |          |
        Student      Dog
```

Some important methods provided by `Object`:

| Method | Purpose |
|---|---|
| `toString()` | Returns a string representation of an object |
| `equals()` | Compares objects according to its implementation |
| `hashCode()` | Returns a hash code value |
| `getClass()` | Returns the runtime class of the object |

Example:

```java
class Student {
    int rollNumber = 101;
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        System.out.println(s1.toString());
        System.out.println(s1.getClass());
        System.out.println(s1.hashCode());
    }
}
```

Note: The default `equals()` implementation in `Object` compares object identity. Classes can override it to define logical equality.

## 9. Method Overriding in Inheritance

Method overriding occurs when a subclass provides its own implementation of an inherited instance method with a compatible method signature.

```java
class Animal {

    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

```java
Dog d = new Dog();
d.sound();
```

Output:

```text
Dog barks
```

### Rules of method overriding

- Method name and parameter list must match.
- Return type must be the same or covariant.
- Access visibility cannot be more restrictive.
- `@Override` is recommended to catch mistakes.
- `final` methods cannot be overridden.
- `private` methods are not inherited and cannot be overridden.
- Static methods are hidden, not overridden.
- Constructors cannot be overridden.

We will explore overriding and runtime polymorphism in the next OOP topic.

## 10. Inheritance vs Composition

Inheritance represents an `is-a` relationship.

Composition represents a `has-a` relationship.

### Inheritance

A Student is a Person.

```java
class Person {
}

class Student extends Person {
}
```

### Composition

A Car has an Engine.

```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    private Engine engine = new Engine();

    void startCar() {
        engine.start();
    }
}
```

### Comparison

| Inheritance | Composition |
|---|---|
| Is-a relationship | Has-a relationship |
| Uses `extends` | Uses object references |
| Tighter coupling | Often more flexible |
| Reuses inherited behaviour | Delegates behaviour to contained objects |

**Interview point:** Prefer composition when the relationship is not genuinely an `is-a` relationship. Composition can make systems easier to change and test.

## 11. The `final` Keyword and Inheritance

The `final` keyword can restrict inheritance and overriding.

### Final class

```java
final class Vehicle {
}

// Compilation error
class Car extends Vehicle {
}
```

A final class cannot be extended.

### Final method

```java
class Parent {

    final void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    // Compilation error
    // void display() {}
}
```

A final method cannot be overridden.

### Quick revision

| Usage | Meaning |
|---|---|
| `final` variable | Cannot be reassigned after initialization |
| `final` method | Cannot be overridden |
| `final` class | Cannot be extended |

## 12. Upcasting and Downcasting

Inheritance also allows references to be assigned across a parent-child hierarchy.

### Upcasting

Converting a child reference to a parent reference.

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}

Dog d = new Dog();

Animal a = d; // Upcasting
a.eat();
```

Upcasting is implicit and safe.

However, through the `Animal` reference, you can directly access only members available through the `Animal` type.

### Downcasting

Converting a parent reference back to a child reference.

```java
Animal a = new Dog();

Dog d = (Dog) a; // Downcasting
d.bark();
```

Downcasting requires an explicit cast.

An invalid downcast can cause `ClassCastException`.

```java
Animal a = new Animal();

// Runtime exception
Dog d = (Dog) a;
```

### Safer downcasting with `instanceof`

```java
Animal a = new Dog();

if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.bark();
}
```

Modern Java also supports pattern matching:

```java
if (a instanceof Dog d) {
    d.bark();
}
```

## 13. Important Interview Questions

<details>
<summary>1. What is inheritance in Java?</summary>

Inheritance allows a class to acquire accessible fields and methods from another class using `extends`. It promotes reuse and establishes an `is-a` relationship.

</details>

<details>
<summary>2. What is the difference between a superclass and a subclass?</summary>

A superclass is the parent class whose members can be inherited. A subclass is the child class that extends the superclass.

</details>

<details>
<summary>3. Does Java support multiple inheritance?</summary>

Java does not support multiple inheritance through classes. However, a class can implement multiple interfaces.

</details>

<details>
<summary>4. Why does Java not support multiple inheritance through classes?</summary>

It avoids ambiguity and complexity, especially problems such as the diamond problem where multiple parent classes could provide conflicting implementations.

</details>

<details>
<summary>5. What is the difference between this and super?</summary>

`this` refers to the current object. `super` is used to access superclass members and invoke superclass constructors.

</details>

<details>
<summary>6. Why must super() be the first statement in a constructor?</summary>

The superclass constructor must execute before the subclass constructor body so that the superclass portion of the object is initialized first.

</details>

<details>
<summary>7. Does a constructor get inherited?</summary>

No. Constructors are not inherited. A subclass constructor can invoke a superclass constructor using `super()`.

</details>

<details>
<summary>8. What happens if the parent class has only a parameterized constructor?</summary>

The child constructor cannot rely on the compiler-inserted no-argument `super()` call. It must explicitly invoke an accessible parent constructor with matching arguments.

</details>

<details>
<summary>9. Can private members be inherited?</summary>

Private superclass members are part of the superclass portion of the object, but they are not directly accessible by the subclass. The subclass may interact with them through accessible superclass methods.

</details>

<details>
<summary>10. What is the difference between method overloading and overriding?</summary>

Overloading means defining multiple methods with the same name but different parameter lists. Overriding means a subclass provides a compatible implementation of an inherited instance method.

</details>

<details>
<summary>11. Can we override a static method?</summary>

No. Static methods are associated with the class and are hidden when a subclass declares a compatible static method. This is called method hiding.

</details>

<details>
<summary>12. Can a final class be inherited?</summary>

No. A class declared with `final` cannot be extended.

</details>

<details>
<summary>13. What is the difference between inheritance and composition?</summary>

Inheritance models an `is-a` relationship, while composition models a `has-a` relationship. Composition is often preferred when a class needs to use another object's functionality without becoming its subtype.

</details>

<details>
<summary>14. What is the Object class in Java?</summary>

`Object` is the root superclass of Java's class hierarchy. Every class directly or indirectly inherits from it.

</details>

<details>
<summary>15. What is the difference between upcasting and downcasting?</summary>

Upcasting converts a subclass reference to a superclass reference and is implicit. Downcasting converts a superclass reference to a subclass reference and requires an explicit cast. An invalid downcast can throw `ClassCastException`.

</details>

## 14. Practice Tasks

1. Create a `Person` class with `name` and `age`.
2. Create a `Student` class extending `Person` with `rollNumber`.
3. Create a `Teacher` class extending `Person` with `subject`.
4. Demonstrate single, multilevel and hierarchical inheritance.
5. Create a parent and child class with the same variable name. Demonstrate `super`.
6. Override a parent method and call the parent implementation using `super`.
7. Demonstrate constructor chaining using both `this()` and `super()`.
8. Create an example of composition using `Car` and `Engine`.
9. Demonstrate upcasting and safe downcasting using `instanceof`.
10. Try extending a final class and overriding a final method. Understand the compilation errors.

## 15. Quick Revision

| Concept | Remember |
|---|---|
| Inheritance | Reuse and `is-a` relationship |
| `extends` | Inherit from a class |
| `implements` | Implement interfaces |
| Superclass | Parent class |
| Subclass | Child class |
| `super.field` | Access parent field |
| `super.method()` | Call parent method |
| `super()` | Invoke parent constructor |
| `this()` | Invoke another constructor in same class |
| Overriding | Redefine inherited instance method |
| `final` class | Cannot be inherited |
| `final` method | Cannot be overridden |
| Composition | `has-a` relationship |
| Upcasting | Child reference to parent reference |
| Downcasting | Parent reference to child reference |

## 16. References

- [Oracle Java Tutorials — Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
- [Oracle Java Tutorials — Using the super Keyword](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)
- [W3Schools — Java Inheritance](https://www.w3schools.com/java/java_inheritance.asp)
- [GeeksforGeeks — Inheritance in Java](https://www.geeksforgeeks.org/java/inheritance-in-java/)

---

**Next topic:** Polymorphism and Abstraction — method overloading, overriding, dynamic method dispatch, abstract classes, and interfaces.