# Call by Value and Object Copying in Java

## 1. How Java Passes Arguments

**Interview definition:**

Java is always pass-by-value. When an object is passed to a method, a copy of its reference value is passed, not the original reference variable.

This is one of the most important Java interview concepts.

Java does not support pass-by-reference.

## 2. Pass by Value — Primitive Types

When we pass a primitive variable to a method, Java copies its value.

```java
public class Main {

    static void change(int number) {
        number = 100;
    }

    public static void main(String[] args) {

        int value = 10;

        change(value);

        System.out.println(value);
    }
}
```

Output:

```text
10
```

### What happened internally?

```text
main()
------
value = 10
   |
   | Copy value
   v
change()
--------
number = 10

number = 100
```

The method receives its own copy.

Changing `number` does not change `value`.

## 3. Pass by Value — Objects

This is where things become interesting.

When we pass an object to a method:

- The reference value is copied.
- Both reference variables initially refer to the same object.
- The method can modify the object's state through its reference.
- Reassigning the method's reference does not reassign the caller's reference.

### Example

```java
class Student {

    String name;
}

public class Main {

    static void changeName(Student s) {
        s.name = "Rahul";
    }

    public static void main(String[] args) {

        Student s1 = new Student();
        s1.name = "Amit";

        changeName(s1);

        System.out.println(s1.name);
    }
}
```

Output:

```text
Rahul
```

### Memory representation

Before calling the method:

```text
Stack                       Heap

s1 ----------------------> Student Object
                            name = "Amit"
```

During the method call:

```text
Stack                       Heap

main()
s1 -----------+
              |
              +----------> Student Object
              |             name = "Amit"
changeName()  |
s -----------+
```

Both references point to the same object.

When we execute:

```java
s.name = "Rahul";
```

We modify the shared object.

Therefore, `s1.name` also becomes `"Rahul"`.

## 4. Proving That Java Is Pass by Value

Consider another example.

```java
class Student {

    String name;
}

public class Main {

    static void changeReference(Student s) {

        s = new Student();
        s.name = "Rahul";
    }

    public static void main(String[] args) {

        Student s1 = new Student();
        s1.name = "Amit";

        changeReference(s1);

        System.out.println(s1.name);
    }
}
```

Output:

```text
Amit
```

### Why?

Initially:

```text
s1 ---------> Object A
               name = "Amit"
```

During method execution, the reference value is copied:

```text
s1 ---------> Object A
               name = "Amit"

s  ---------> Object A
```

Now the method executes:

```java
s = new Student();
```

A new object is created.

```text
s1 ---------> Object A
               name = "Amit"

s  ---------> Object B
               name = "Rahul"
```

Only the local reference `s` is reassigned.

The original reference `s1` still points to Object A.

Therefore:

```java
System.out.println(s1.name);
```

Prints:

```text
Amit
```

**Interview conclusion:** Java passes the object reference by value. It does not pass the caller's reference variable itself.

## 5. Reference Reassignment vs Object Modification

| Operation | Effect on caller's object |
|---|---|
| `s.name = "Rahul"` | Modifies shared object |
| `s = new Student()` | Reassigns only local reference |
| `s = null` | Changes only local reference |
| `s.name = null` | Changes the object's field |

This distinction is essential for understanding Java methods.

## 6. What is Object Copying?

Object copying means creating another object with values based on an existing object.

Consider:

```java
class Student {

    int id;
    String name;
}
```

We create one object:

```java
Student s1 = new Student();

s1.id = 101;
s1.name = "Amit";
```

Now suppose we want another object with the same data.

There are different ways to achieve this.

## 7. Reference Copy — Not an Object Copy

Consider:

```java
Student s1 = new Student();

s1.id = 101;
s1.name = "Amit";

Student s2 = s1;
```

Here, no new object is created.

Both references point to the same object.

```text
s1 --------+
           |
           +------> Student Object
           |         id = 101
s2 --------+         name = Amit
```

If we modify:

```java
s2.name = "Rahul";
```

Then:

```java
System.out.println(s1.name);
```

Output:

```text
Rahul
```

Because `s1` and `s2` refer to the same object.

This is reference copying, not object copying.

## 8. Shallow Copy

A shallow copy creates a new outer object but copies the values of its fields as they are.

For reference-type fields, the references are copied, not the referenced objects.

### Example

```java
class Address {

    String city;

    Address(String city) {
        this.city = city;
    }
}

class Student {

    int id;
    String name;
    Address address;

    Student(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }

    Student(Student other) {
        this.id = other.id;
        this.name = other.name;
        this.address = other.address;
    }
}
```

Main class:

```java
public class Main {

    public static void main(String[] args) {

        Address address = new Address("Mumbai");

        Student s1 = new Student(101, "Amit", address);

        Student s2 = new Student(s1);

        s2.name = "Rahul";
        s2.address.city = "Pune";

        System.out.println(s1.name);
        System.out.println(s1.address.city);
    }
}
```

Output:

```text
Amit
Pune
```

### Why?

The outer `Student` object was copied, but both objects still share the same `Address` object.

```text
           Student Object A
s1 ------> id = 101
           name = Amit
           address --------+
                            |
                            |
                            v
                       Address Object
                       city = Pune
                            ^
                            |
           Student Object B
s2 ------> id = 101
           name = Rahul
           address --------+
```

Notice:

- Changing `s2.name` does not affect `s1.name`.
- Changing `s2.address.city` affects `s1.address.city`.

This happens because the address reference is shared.

## 9. Deep Copy

A deep copy creates a new outer object and also creates independent copies of the nested mutable objects.

### Example

```java
class Address {

    String city;

    Address(String city) {
        this.city = city;
    }
}

class Student {

    int id;
    String name;
    Address address;

    Student(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }

    Student(Student other) {

        this.id = other.id;
        this.name = other.name;

        // Create a separate Address object
        this.address = new Address(other.address.city);
    }
}
```

Main class:

```java
public class Main {

    public static void main(String[] args) {

        Address address = new Address("Mumbai");

        Student s1 = new Student(101, "Amit", address);

        Student s2 = new Student(s1);

        s2.name = "Rahul";
        s2.address.city = "Pune";

        System.out.println(s1.name);
        System.out.println(s1.address.city);
    }
}
```

Output:

```text
Amit
Mumbai
```

### Memory representation

```text
s1 --------> Student Object A
              name = Amit
              address --------> Address Object A
                                  city = Mumbai


s2 --------> Student Object B
              name = Rahul
              address --------> Address Object B
                                  city = Pune
```

Now both students have independent address objects.

Changing one does not affect the other.

## 10. Shallow Copy vs Deep Copy

| Feature | Shallow Copy | Deep Copy |
|---|---|---|
| Outer object | New object | New object |
| Primitive fields | Values copied | Values copied |
| Reference fields | References copied | Referenced mutable objects copied |
| Nested mutable objects | Shared | Independent copies |
| Memory usage | Usually lower | Usually higher |
| Complexity | Simpler | More complex |

**Important:** A deep copy must consider the entire relevant object graph. Simply copying one nested object is not necessarily enough if it contains other mutable references.

## 11. Ways to Copy Objects in Java

### 11.1 Copy Constructor

A constructor accepts another object of the same class.

```java
class Student {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    Student(Student other) {
        this.id = other.id;
        this.name = other.name;
    }
}
```

This is a common and explicit way to implement copying.

Whether the result is shallow or deep depends on how reference fields are handled.

### 11.2 Clone Method

Java provides the `clone()` method through the `Object` class.

A common implementation uses `Cloneable`:

```java
class Student implements Cloneable {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public Student clone() {

        try {
            return (Student) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);
        }
    }
}
```

Example:

```java
Student s1 = new Student(101, "Amit");
Student s2 = s1.clone();
```

Important points:

- `Cloneable` is a marker interface.
- `Object.clone()` performs a field-by-field shallow copy.
- It does not automatically deep-copy nested objects.
- `clone()` has several design limitations, so copy constructors or explicit copy methods are often preferred in modern Java code.

### 11.3 Copying with an Explicit Method

You can also provide a method that creates and returns a new object.

```java
class Student {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    Student copy() {
        return new Student(this.id, this.name);
    }
}
```

The copying behaviour is controlled by your implementation.

## 12. Immutable Objects and Copying

An immutable object cannot be changed after it is created.

Example:

```java
final class Employee {

    private final int id;
    private final String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Since the object cannot be modified after construction, sharing its reference is generally safe.

However, immutability must be deep enough: a final reference to a mutable object does not automatically make that referenced object immutable.

## 13. Common Mistakes

| Mistake | Correct understanding |
|---|---|
| Java passes objects by reference | Java passes reference values by value |
| `s2 = s1` creates a new object | It copies the reference |
| Shallow copy duplicates nested objects | It shares nested references |
| `clone()` always creates a deep copy | Default cloning is shallow |
| Reassigning a method parameter changes caller's variable | Only the local parameter is reassigned |
| `final` reference makes object immutable | It prevents reference reassignment, not object mutation |

## 14. Important Interview Questions

<details>
<summary>1. Is Java pass-by-value or pass-by-reference?</summary>

Java is always pass-by-value. For objects, the value being copied is the object reference.

</details>

<details>
<summary>2. Why can a method modify an object passed to it?</summary>

Because the copied reference points to the same object as the caller's reference. Both can access and modify that shared object.

</details>

<details>
<summary>3. If Java is pass-by-value, why does changing an object's field affect the original object?</summary>

Because the method receives a copy of the reference value, not a copy of the object. Both references point to the same object.

</details>

<details>
<summary>4. What happens if we reassign an object parameter inside a method?</summary>

Only the local parameter is reassigned. The caller's reference variable remains unchanged.

</details>

<details>
<summary>5. What is the difference between reference copying and object copying?</summary>

Reference copying creates another reference to the same object. Object copying creates a separate object.

</details>

<details>
<summary>6. What is shallow copying?</summary>

Shallow copying creates a new outer object but copies reference values directly. Nested referenced objects may remain shared.

</details>

<details>
<summary>7. What is deep copying?</summary>

Deep copying creates a new object and independent copies of its relevant nested mutable objects.

</details>

<details>
<summary>8. What is the difference between shallow and deep copy?</summary>

Shallow copy shares nested referenced objects, while deep copy creates independent copies of those objects.

</details>

<details>
<summary>9. Does Object.clone() perform deep copying?</summary>

No. Its default behaviour is a shallow field-by-field copy.

</details>

<details>
<summary>10. What is Cloneable?</summary>

Cloneable is a marker interface that indicates an object supports cloning through the conventional `Object.clone()` mechanism.

</details>

<details>
<summary>11. What is a copy constructor?</summary>

A copy constructor is a constructor that accepts another object of the same class and initializes a new object using its values.

</details>

<details>
<summary>12. Is String immutable in Java?</summary>

Yes. Java's String objects are immutable. Operations that appear to modify a string create another String value instead.

</details>

<details>
<summary>13. Does final make an object immutable?</summary>

No. `final` prevents reassignment of the reference variable, but the referenced object may still be mutable.

</details>

<details>
<summary>14. Which is preferable: clone() or a copy constructor?</summary>

Copy constructors or explicit copy methods are often preferred because they make copying behaviour clearer and allow precise control over shallow or deep copying.

</details>

## 15. Practice Tasks

### Beginner

1. Pass a primitive integer to a method and try modifying it.
2. Pass a Student object to a method and modify its name.
3. Pass a Student object to a method and reassign the parameter to a new object.
4. Assign one object reference to another and observe the result.

### Intermediate

5. Create a Student class with an Address object.
6. Implement a shallow copy using a copy constructor.
7. Modify a nested Address field and observe the effect.
8. Implement a deep copy using a copy constructor.
9. Compare the results of both implementations.
10. Implement a `copy()` method for a class containing both primitive and reference fields.

### Think like a developer

11. Design a `UserProfile` class containing:
    - User details.
    - Address.
    - Contact details.

12. Create a copy of the profile.
13. Decide which fields should be shared and which should be independently copied.
14. Explain how you would prevent accidental modification of the original profile.

---

## 16. Quick Revision

| Concept | Key point |
|---|---|
| Pass by value | Copies the argument value |
| Primitive argument | Primitive value is copied |
| Object argument | Reference value is copied |
| Object mutation | Can affect the shared object |
| Reference reassignment | Does not change caller's reference |
| Reference copy | Two references, one object |
| Shallow copy | New outer object, shared nested references |
| Deep copy | New outer and independent nested mutable objects |
| Copy constructor | Explicit object-copying approach |
| `clone()` | Default shallow copy |
| `Cloneable` | Marker interface |
| Immutable object | State cannot be changed after creation |

## 17. References

- [Oracle Java Tutorials — Passing Information to a Method](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html)
- [Oracle Java Object Class Documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html)
- [Oracle Java Cloneable Documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Cloneable.html)
- [GeeksforGeeks — Shallow Copy and Deep Copy](https://www.geeksforgeeks.org/difference-between-shallow-and-deep-copy-of-a-class/)
- [Baeldung — Java Clone](https://www.baeldung.com/java-deep-copy)