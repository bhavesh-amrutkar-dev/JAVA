# Encapsulation and Access Modifiers in Java

## 1. What is Encapsulation?

**Interview definition:**

Encapsulation is an OOP principle that combines data (fields) and the methods that operate on that data into a single unit, while controlling direct access to the object's internal state.

In Java, encapsulation is commonly implemented by:
- Declaring fields as `private`.
- Providing controlled access through `public` methods.
- Validating data before modifying an object's state.

### Real-world example

Think about a bank account.

You can:
- Deposit money.
- Withdraw money.
- Check your balance.

But you should not be able to directly assign any arbitrary value to the account balance.

For example, this should not be allowed:

```java
account.balance = -50000;
```

Instead, the account should control how its balance changes.

### Visual representation

```text
             BANK ACCOUNT OBJECT
        ┌──────────────────────────┐
        │                          │
        │     PRIVATE DATA         │
        │                          │
        │     balance = 5000       │
        │                          │
        ├──────────────────────────┤
        │                          │
        │     PUBLIC METHODS       │
        │                          │
        │     deposit()            │
        │     withdraw()           │
        │     getBalance()         │
        │                          │
        └──────────────────────────┘
                    ▲
                    |
             External code
          interacts through methods
```

The internal data is protected, while controlled operations are exposed.

---

# 2. Why Do We Need Encapsulation?

Consider this class:

```java
class Student {

    public String name;
    public int age;
    public double fee;
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        s1.name = "Rahul";
        s1.age = -20;
        s1.fee = -5000;
    }
}
```

### What's the problem?

There is no validation.

Anyone who can access these public fields can assign invalid values.

For example:
- Age cannot logically be negative.
- Fee should not be negative.
- Name may be required.

This is where encapsulation helps.

### Without encapsulation vs with encapsulation

| Without encapsulation | With encapsulation |
|---|---|
| Direct access to fields | Controlled access to fields |
| Easy to assign invalid data | Validation can be applied |
| Internal implementation is exposed | Internal implementation is hidden |
| Changes can affect external code | Internal implementation can change more safely |

---

# 3. How to Implement Encapsulation in Java

The most common approach is:

1. Declare instance variables as `private`.
2. Provide public getter methods to read values.
3. Provide public setter methods to modify values.
4. Apply validation wherever required.

### Example

```java
class Student {

    private String name;
    private int age;
    private double fee;

    // Getter
    public String getName() {
        return name;
    }

    // Setter
    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {

        if (age > 0) {
            this.age = age;
        } else {
            System.out.println("Invalid age");
        }
    }

    public double getFee() {
        return fee;
    }

    public void setFee(double fee) {

        if (fee >= 0) {
            this.fee = fee;
        } else {
            System.out.println("Fee cannot be negative");
        }
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        s1.setName("Rahul");
        s1.setAge(21);
        s1.setFee(25000);

        System.out.println(s1.getName());
        System.out.println(s1.getAge());
        System.out.println(s1.getFee());

        s1.setAge(-20);
    }
}
```

Output:

```text
Rahul
21
25000.0
Invalid age
```

### Visual representation

```text
       EXTERNAL CODE
             |
             |
       ┌─────▼─────┐
       │  Student  │
       │           │
       │  PRIVATE  │
       │           │
       │  name     │
       │  age      │
       │  fee      │
       │           │
       ├───────────┤
       │  PUBLIC   │
       │           │
       │ getName() │
       │ setName() │
       │ getAge()  │
       │ setAge()  │
       │ getFee()  │
       │ setFee()  │
       └───────────┘
```

---

# 4. What are Getters and Setters?

### Getter

A getter is a method that returns the value of a field.

Example:

```java
public int getAge() {
    return age;
}
```

### Setter

A setter is a method that modifies the value of a field.

Example:

```java
public void setAge(int age) {
    this.age = age;
}
```

### Common naming convention

| Field | Getter | Setter |
|---|---|---|
| `name` | `getName()` | `setName()` |
| `age` | `getAge()` | `setAge()` |
| `fee` | `getFee()` | `setFee()` |
| `active` | `isActive()` | `setActive()` |

For boolean properties, `isActive()` is a common getter convention.

**Important:** Getters and setters are not mandatory for every field. A class should expose only the operations that make sense for its design.

For example, a class may provide a getter but no setter if a value should not be changed after construction.

---

# 5. What are Access Modifiers?

**Interview definition:**

Access modifiers in Java control the visibility and accessibility of classes, fields, constructors, and methods.

Java has four access levels:

1. `private`
2. Default (package-private)
3. `protected`
4. `public`

### Access modifier visualization

```text
             ACCESS LEVELS
                   |
     ┌─────────────┼──────────────┐
     |             |              |
     ▼             ▼              ▼
  private       protected       public
     |             |              |
  Same class   Same package    Everywhere
                 + subclass
```

Default access is package-private: it allows access within the same package.

## 6. Understanding Each Access Modifier

### A. private

Accessible only inside the class where the member is declared.

```java
class Student {

    private int age;

    private void displayAge() {
        System.out.println(age);
    }
}
```

This is not allowed from another class:

```java
Student s1 = new Student();

s1.age = 21; // Compilation error
```

### B. Default (Package-Private)

If no access modifier is specified, Java uses package-private access.

```java
class Student {

    int age;

    void displayAge() {
        System.out.println(age);
    }
}
```

Accessible from classes in the same package, but not directly from unrelated classes in other packages.

### C. protected

Accessible:
- Within the same package.
- In subclasses in other packages, subject to Java's protected-access rules.

```java
class Person {

    protected String name;
}
```

Subclass:

```java
class Student extends Person {

    void display() {
        System.out.println(name);
    }
}
```

### D. public

Accessible from anywhere the declaring type is accessible.

```java
public class Student {

    public String name;

    public void display() {
        System.out.println(name);
    }
}
```

---

# 7. Access Modifier Comparison Table

| Access modifier | Same class | Same package | Subclass in another package | Other package, non-subclass |
|---|---:|---:|---:|---:|
| `private` | Yes | No | No | No |
| Default | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes* | No |
| `public` | Yes | Yes | Yes | Yes |

*Protected access across packages has additional restrictions: a subclass can access the inherited protected member through itself or an appropriate subclass reference, not through an arbitrary superclass instance.*

### Memory trick

```text
private    → Same class
default    → Same package
protected  → Same package + subclass access
public     → Accessible wherever the type is accessible
```

---

# 8. Class-Level Access Modifiers

Top-level classes have fewer access choices than fields and methods.

A top-level class can be:

- `public`
- Package-private (no modifier)

Example:

```java
public class Student {
}
```

Or:

```java
class Teacher {
}
```

A top-level class cannot be declared `private` or `protected`.

Nested classes can use additional access modifiers.

---

# 9. Encapsulation vs Data Hiding

These concepts are related, but they are not exactly the same.

| Encapsulation | Data hiding |
|---|---|
| Combines data and methods into a unit and controls access | Restricts direct access to internal data |
| Achieved through class design and access control | Commonly achieved through `private` members |
| Focuses on controlled interaction | Focuses on restricting visibility |

**Interview answer:**

Encapsulation is the broader OOP concept of bundling state and behaviour while controlling access. Data hiding is one of the techniques used to protect internal state.

---

# 10. Does Encapsulation Always Require Getters and Setters?

No.

Consider:

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

Notice:
- We have a getter for the balance.
- We do not provide a setter for the balance.
- Balance can only be changed through valid deposit and withdrawal operations.

This is often better than providing unrestricted setters.

**Key idea:** Encapsulation means controlling how an object is used, not blindly generating getters and setters for every field.

---

# 11. Encapsulation and Immutability

Encapsulation can also help us create immutable classes.

**Definition:**

An immutable object is an object whose state cannot be changed after it has been created.

Example:

```java
final class Employee {

    private final String name;
    private final int age;

    public Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

Here:
- The class is `final`, so it cannot be subclassed.
- Fields are `private`.
- Fields are `final`.
- There are no setters.

This example is immutable because its fields are immutable types (`String` and `int`) and no method changes them.

**Important:** If an immutable class contains mutable objects, simply using `private final` is not enough. Defensive copying may be needed.

We will study immutable classes in more detail later.

---

# 12. Practical Example — Bank Account

```java
class BankAccount {

    private String accountHolder;
    private double balance;

    public BankAccount(String accountHolder, double initialBalance) {

        if (initialBalance < 0) {
            throw new IllegalArgumentException(
                "Initial balance cannot be negative"
            );
        }

        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }

    public String getAccountHolder() {
        return accountHolder;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {

        if (amount <= 0) {
            throw new IllegalArgumentException(
                "Deposit amount must be positive"
            );
        }

        balance += amount;
    }

    public void withdraw(double amount) {

        if (amount <= 0 || amount > balance) {
            throw new IllegalArgumentException(
                "Invalid withdrawal amount"
            );
        }

        balance -= amount;
    }
}

public class Main {

    public static void main(String[] args) {

        BankAccount account =
            new BankAccount("Rahul", 10000);

        account.deposit(5000);
        account.withdraw(2000);

        System.out.println(account.getAccountHolder());
        System.out.println(account.getBalance());
    }
}
```

Output:

```text
Rahul
13000.0
```

### What did we achieve?

- Internal balance is private.
- External code cannot directly modify balance.
- Deposit and withdrawal operations validate input.
- The object controls its own state.
- Invalid operations are rejected.

This is a practical example of encapsulation.

---

# 13. Common Mistakes

| Mistake | Correct understanding |
|---|---|
| Encapsulation means only making fields private | It also means providing controlled access to state and behaviour |
| Every private field needs a setter | No; expose only necessary operations |
| `protected` means accessible everywhere in a subclass | Cross-package protected access has specific restrictions |
| Default means public | No; default is package-private |
| Getters and setters automatically guarantee validation | Validation must be implemented explicitly |
| `final` makes an object deeply immutable | No; referenced mutable objects can still change |
| Encapsulation and abstraction are identical | They are related but distinct OOP concepts |

---

# 14. Interview Questions

<details>
<summary>1. What is encapsulation in Java?</summary>

Encapsulation is the process of combining data and methods into a class while controlling access to the internal state of an object. It is commonly implemented using private fields and controlled public methods.
</details>

<details>
<summary>2. How do you achieve encapsulation in Java?</summary>

Declare fields as private and expose only the necessary operations through public methods such as getters, setters, or business methods.
</details>

<details>
<summary>3. What is data hiding?</summary>

Data hiding means restricting direct access to an object's internal data. Java commonly achieves this through private fields and controlled access methods.
</details>

<details>
<summary>4. What is the difference between encapsulation and data hiding?</summary>

Encapsulation combines state and behaviour and controls how they interact. Data hiding focuses specifically on restricting direct access to internal data.
</details>

<details>
<summary>5. What are access modifiers in Java?</summary>

Access modifiers control the visibility of classes and their members. Java provides private, package-private (default), protected, and public access levels.
</details>

<details>
<summary>6. What is the difference between default and protected access?</summary>

Default access allows access within the same package. Protected access allows access within the same package and also provides qualified access to subclasses in other packages.
</details>

<details>
<summary>7. Can we access a private variable outside its class?</summary>

Not through ordinary direct access. The class can expose controlled public methods to read or modify the field. Reflection can bypass ordinary access checks in some circumstances, subject to access restrictions.
</details>

<details>
<summary>8. Are getters and setters compulsory for encapsulation?</summary>

No. They are common techniques, but a class can expose only the operations that make sense for its design.
</details>

<details>
<summary>9. Can we have a getter without a setter?</summary>

Yes. This is useful when a value should be readable but not externally modifiable.
</details>

<details>
<summary>10. Can a top-level Java class be private or protected?</summary>

No. A top-level class can be public or package-private. Nested classes can use private and protected access modifiers.
</details>

<details>
<summary>11. What is an immutable class?</summary>

An immutable class creates objects whose state cannot be changed after construction. Common techniques include private final fields, no setters, and defensive copying of mutable values.
</details>

<details>
<summary>12. Does private mean that data is completely secure?</summary>

No. Private controls access through Java's normal language access rules. It is not a complete security boundary against every mechanism, such as reflection or unsafe runtime access.
</details>

<details>
<summary>13. Why is encapsulation useful in large applications?</summary>

It reduces coupling, protects object invariants, allows internal implementation changes, and gives developers controlled interfaces for interacting with objects.
</details>

<details>
<summary>14. Is encapsulation possible without getters and setters?</summary>

Yes. For example, a BankAccount class can expose deposit and withdraw methods without exposing a setter for its balance.
</details>

---

# 15. Practical Exercises

1. Create a `Student` class with private name, age, and fee fields.
2. Create getters and setters for the fields.
3. Validate age so that negative values are rejected.
4. Validate fee so that negative amounts are rejected.
5. Create a `BankAccount` class with a private balance.
6. Implement deposit and withdrawal methods.
7. Do not provide a direct balance setter.
8. Create an immutable `Employee` class.
9. Create two packages and test all four access modifiers.
10. Explain why encapsulation is useful in a large application.

---

# 16. Quick Revision Checklist

- [ ] Explain encapsulation with a real-world example.
- [ ] Explain why direct public fields can be problematic.
- [ ] Explain getters and setters.
- [ ] Explain all four access levels.
- [ ] Differentiate default and protected access.
- [ ] Explain top-level class access restrictions.
- [ ] Differentiate encapsulation and data hiding.
- [ ] Explain why setters are not always necessary.
- [ ] Explain immutable classes.
- [ ] Implement a BankAccount class with controlled state.

---

# 17. Further Reading

- [Oracle Java Tutorial — Controlling Access to Members](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
- [Oracle Java Language Specification — Access Control](https://docs.oracle.com/javase/specs/jls/se21/html/jls-6.html#jls-6.6)
- [W3Schools — Java Encapsulation](https://www.w3schools.com/java/java_encapsulation.asp)
- [GeeksforGeeks — Encapsulation in Java](https://www.geeksforgeeks.org/java/encapsulation-in-java/)

**Previous document:** [Object Memory Layout](../02-Object-Memory/object-memory-layout.md)

**Next document:** [Inheritance and the super Keyword](../04-Inheritance/inheritance-and-super-keyword.md)