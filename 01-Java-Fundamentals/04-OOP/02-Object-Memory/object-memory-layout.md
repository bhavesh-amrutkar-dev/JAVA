# Object Memory Layout in Java — Stack, Heap, References and Object Size

## 1. Why do we need to understand Java memory?

Whenever we write:

```java
Student s1 = new Student();
```

Several things happen behind the scenes:

- A new object is created.
- Memory is allocated for the object.
- Instance variables receive default values.
- The constructor executes.
- A reference to the object is assigned to `s1`.

To understand this properly, we need to understand the JVM's memory areas.

**Interview definition:**

Java memory management is the process through which the JVM allocates, manages, and releases memory during program execution.

---

# 2. JVM Runtime Memory Areas

The JVM divides runtime memory into different areas.

```text
                  JVM RUNTIME MEMORY
                         |
       ┌─────────────────┴─────────────────┐
       |                                   |
       ▼                                   ▼
  THREAD-SHARED                       PER-THREAD
       |                                   |
  ┌────┴─────────────┐             ┌───────┼─────────┐
  |                  |             |       |         |
  ▼                  ▼             ▼       ▼         ▼
 Heap            Method Area      Stack   PC       Native
 Memory          (Logical)        Memory  Register Method
                                                 Stack
```

| Memory area | Purpose |
|---|---|
| Heap | Stores objects and arrays |
| Method Area | Stores class-level structures and runtime metadata |
| Java Stack | Stores method frames, local variables, and intermediate results |
| PC Register | Tracks the current JVM instruction for a thread |
| Native Method Stack | Supports execution of native methods |

**Important:**

- Heap is shared among application threads.
- Each thread has its own Java stack and PC register.
- The JVM specification defines the logical runtime areas; their physical implementation can differ between JVMs.
- In HotSpot JVM, class metadata is commonly stored in native memory called Metaspace.

---

# 3. Stack Memory

**Interview definition:**

The Java Stack is a per-thread memory area that stores method invocation frames. Each frame contains information needed to execute a method, including local variables, an operand stack, and other bookkeeping information.

### How does the stack work?

The stack follows the LIFO principle:

**Last In, First Out.**

Example:

```java
public class Main {

    static void methodA() {
        int a = 10;
        methodB();
    }

    static void methodB() {
        int b = 20;
    }

    public static void main(String[] args) {
        methodA();
    }
}
```

### Stack execution visualization

When `main()` starts:

```text
       JAVA STACK
    ┌──────────────────┐
    │                  │
    │    main()        │
    │                  │
    └──────────────────┘
```

When `methodA()` is called:

```text
       JAVA STACK
    ┌──────────────────┐
    │                  │
    │    methodA()     │
    │    a = 10        │
    │                  │
    ├──────────────────┤
    │                  │
    │    main()        │
    │                  │
    └──────────────────┘
```

When `methodB()` is called:

```text
       JAVA STACK
    ┌──────────────────┐
    │                  │
    │    methodB()     │
    │    b = 20        │
    │                  │
    ├──────────────────┤
    │                  │
    │    methodA()     │
    │    a = 10        │
    │                  │
    ├──────────────────┤
    │                  │
    │    main()        │
    │                  │
    └──────────────────┘
```

When `methodB()` finishes, its frame is removed.

```text
       JAVA STACK
    ┌──────────────────┐
    │                  │
    │    methodA()     │
    │    a = 10        │
    │                  │
    ├──────────────────┤
    │                  │
    │    main()        │
    │                  │
    └──────────────────┘
```

### Important points

- Each method invocation creates a new stack frame.
- A method's frame is removed when its execution completes.
- Every thread has its own Java stack.
- Local variables are associated with their method's execution frame.
- The JVM may optimize actual storage, so do not assume every source-level variable physically occupies a stack slot.

---

# 4. Heap Memory

**Interview definition:**

The Java Heap is the runtime memory area from which memory for objects and arrays is allocated. It is shared among threads and is managed by the garbage collector.

Example:

```java
class Student {
    String name;
    int age;
}

public class Main {
    public static void main(String[] args) {

        Student s1 = new Student();

    }
}
```

Conceptually:

```text
                 STACK                         HEAP
          ┌────────────────┐            ┌─────────────────────┐
          │                │            │                     │
          │   main()       │            │   Student Object    │
          │                │            │                     │
          │   s1 ──────────────────────►│   name = null       │
          │                │            │   age = 0           │
          │                │            │                     │
          └────────────────┘            └─────────────────────┘
```

### Important points

- Objects and arrays are allocated from the heap.
- The heap is shared among threads.
- Objects can remain alive beyond the method that created them.
- Garbage collection reclaims memory occupied by objects that are no longer reachable.
- Garbage collection does not guarantee immediate object removal.

---

# 5. Primitive vs Reference Variables — The Important Difference

A common beginner misconception is:

> Primitive variables are always stored in the stack, and objects are always stored in the heap.

This is an oversimplification.

The actual location depends on where the variable is declared and how the JVM executes the program.

### Case 1: Local primitive variable

```java
void display() {
    int age = 21;
}
```

`age` is a local variable associated with the method's execution frame.

### Case 2: Local reference variable

```java
void display() {
    Student s1 = new Student();
}
```

Conceptually:

```text
             STACK                         HEAP
       ┌────────────────┐          ┌────────────────────┐
       │                │          │                    │
       │  s1 ──────────────────────►│  Student Object    │
       │                │          │                    │
       └────────────────┘          └────────────────────┘
```

The local reference is associated with the method frame, while the object is allocated on the heap.

### Case 3: Primitive instance variable

```java
class Student {
    int age;
}
```

The `age` field is part of the Student object.

```text
                HEAP
       ┌──────────────────────┐
       │    Student Object    │
       │                      │
       │    age = 21          │
       │                      │
       └──────────────────────┘
```

### Case 4: Reference instance variable

```java
class Student {
    Address address;
}
```

The `address` reference is a field inside the Student object. It may point to another heap object.

```text
                HEAP
       ┌──────────────────┐
       │  Student Object  │
       │                  │
       │  address ────────────────┐
       │                  │       │
       └──────────────────┘       │
                                  ▼
                         ┌─────────────────┐
                         │ Address Object  │
                         │                 │
                         │ city = "Pune"   │
                         └─────────────────┘
```

### Revision table

| Variable | Conceptual storage |
|---|---|
| Local primitive | Method frame |
| Local reference | Method frame |
| Primitive instance field | Inside the heap object |
| Reference instance field | Inside the heap object |
| Primitive array element | Inside the heap array |
| Reference array element | Inside the heap array |

Remember: this is a conceptual model. JIT compilation can optimize variables and object allocations.

---

# 6. What Happens When We Create an Object?

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

public class Main {
    public static void main(String[] args) {

        Student s1 = new Student("Rahul", 21);

    }
}
```

### Object creation flow

```text
       Student s1 = new Student("Rahul", 21)
                         |
                         ▼
              Class initialization
                (when required)
                         |
                         ▼
                  Object allocation
                         |
                         ▼
              Default field initialization
                         |
                         ▼
               Constructor execution
                         |
                         ▼
              Assign constructor values
                         |
                         ▼
             Return object reference
                         |
                         ▼
                 Assign to s1
```

**Note:** Class initialization is a separate process from object allocation. A class is initialized when the JVM's initialization rules require it, not necessarily every time an object is created.

---

# 7. Understanding Object Memory Layout

Now let's understand a particularly important interview topic:

**How much memory does a Java object occupy?**

The exact object layout is JVM-implementation-dependent.

For HotSpot JVM, a simplified object layout can be represented as:

```text
             JAVA OBJECT LAYOUT
          (Simplified HotSpot Model)

       ┌──────────────────────────┐
       │                          │
       │       OBJECT HEADER       │
       │                          │
       │  Mark Word                │
       │  Class Pointer            │
       │                          │
       ├──────────────────────────┤
       │                          │
       │       INSTANCE FIELDS     │
       │                          │
       │  int age                  │
       │  double fee               │
       │  boolean active           │
       │  reference name           │
       │                          │
       ├──────────────────────────┤
       │                          │
       │          PADDING          │
       │                          │
       └──────────────────────────┘
```

An ordinary HotSpot object commonly has these logical parts:

1. Object header
2. Instance fields
3. Alignment padding, if needed

The exact layout depends on the JVM version, architecture, object alignment, and JVM options.

## 8. Object Header

The object header contains JVM metadata associated with the object.

For a typical HotSpot object, it includes:

### A. Mark Word

The Mark Word stores information that can include:

- Object identity hash code, when available
- Locking and synchronization state
- Garbage-collection-related age information
- Other JVM runtime state

The precise encoding can change between JVM versions and configurations.

### B. Class Pointer

The class pointer identifies the object's class metadata.

For example:

```java
Student s1 = new Student();
```

The object is associated with the `Student` class metadata.

### Simplified visual

```text
          OBJECT HEADER
       ┌────────────────────┐
       │                    │
       │     Mark Word      │
       │                    │
       ├────────────────────┤
       │                    │
       │    Class Pointer   │
       │                    │
       └────────────────────┘
```

### How large is the object header?

There is no universal fixed size across all JVMs.

For a common HotSpot configuration:

| Header component | Illustrative size |
|---|---:|
| Mark Word | 8 bytes |
| Class pointer | 4 bytes with compressed class pointers |
| Total | 12 bytes |

Without compressed class pointers, the class pointer may occupy 8 bytes in a typical 64-bit configuration.

These are examples, not guarantees for every HotSpot setup.

---

# 9. What are Compressed OOPs?

OOP means **Ordinary Object Pointer**.

In a 64-bit JVM, references can be represented using 64-bit values. However, HotSpot can use compressed object pointers to reduce the memory used by references.

### Without compressed object pointers

```text
Reference size = 8 bytes
```

### With compressed object pointers

```text
Reference size = commonly 4 bytes
```

The JVM uses an encoding scheme to represent heap references using smaller values.

This can reduce memory consumption, especially when an application contains many objects.

**Interview answer:**

Compressed OOPs are a HotSpot JVM optimization that allows object references to be represented using compressed values, commonly 32 bits instead of 64 bits, when the heap configuration permits it.

**Important:** Do not say every 64-bit JVM always uses 4-byte references. It depends on JVM configuration and heap constraints.

---

# 10. Instance Fields and Their Memory

Consider:

```java
class Student {

    int age;
    int rollNumber;
    double fee;
    boolean active;
}
```

These instance fields are part of each Student object.

A simplified logical representation:

```text
             STUDENT OBJECT
       ┌────────────────────────┐
       │                        │
       │     OBJECT HEADER      │
       │                        │
       ├────────────────────────┤
       │                        │
       │     int age            │
       │     int rollNumber     │
       │                        │
       ├────────────────────────┤
       │                        │
       │     double fee         │
       │                        │
       ├────────────────────────┤
       │                        │
       │     boolean active     │
       │                        │
       ├────────────────────────┤
       │       PADDING          │
       └────────────────────────┘
```

### Java primitive widths

| Primitive | Language-level width |
|---|---:|
| `byte` | 1 byte |
| `short` | 2 bytes |
| `int` | 4 bytes |
| `long` | 8 bytes |
| `float` | 4 bytes |
| `double` | 8 bytes |
| `char` | 2 bytes |
| `boolean` | Not specified as a fixed storage width by the JVM specification |

These widths describe Java primitive types, not necessarily the exact size or position of every field in an object.

For example, a reference field is not a primitive type and its representation depends on the JVM.

---

# 11. What is Memory Padding?

**Interview definition:**

Memory padding is unused space inserted into a data structure or object layout to satisfy alignment requirements.

Why is padding needed?

Modern processors and JVM implementations often benefit from memory alignment. HotSpot commonly aligns object sizes to an 8-byte boundary, though the alignment can be configured.

### Simple mathematical example

Suppose an illustrative object layout has:

```text
Header       = 12 bytes
Fields       = 16 bytes
------------------------
Total        = 28 bytes
```

Assume the required object alignment is 8 bytes.

The next multiple of 8 after 28 is 32.

Therefore:

```text
Padding = 32 - 28
        = 4 bytes
```

### Visual representation

```text
       ┌────────────────────────┐
       │      Header            │  12 bytes
       ├────────────────────────┤
       │      Fields            │  16 bytes
       ├────────────────────────┤
       │      Padding           │   4 bytes
       └────────────────────────┘
                  |
                  ▼
           Total = 32 bytes
```

**Important:** This is a simplified calculation, not a guarantee of the layout of a particular Java class.

HotSpot may reorder fields, insert internal gaps between fields, and apply different layout rules. You cannot reliably calculate the exact object size from source declarations alone.

---

# 12. How Much Memory Does a Reference Variable Occupy?

Consider:

```java
Student s1 = new Student();
Student s2 = new Student();
```

There are two references and two separate objects.

Conceptually:

```text
              STACK                         HEAP
       ┌────────────────┐          ┌────────────────────┐
       │                │          │                    │
       │   s1 ────────────────────►│   Student Object 1 │
       │                │          │                    │
       │   s2 ────────────────────►│   Student Object 2 │
       │                │          │                    │
       └────────────────┘          └────────────────────┘
```

A reference's representation may commonly be:

| JVM configuration | Typical reference representation |
|---|---:|
| 32-bit JVM | 4 bytes |
| 64-bit JVM without compressed OOPs | 8 bytes |
| 64-bit HotSpot with compressed OOPs | 4 bytes |

Again, the exact storage and location of local references can be affected by JIT optimization.

---

# 13. Does a Boolean Occupy 1 Byte or 4 Bytes?

This is a common interview question.

Java does not specify a fixed storage size for `boolean`.

There are different contexts:

| Context | Explanation |
|---|---|
| Java language | `boolean` represents `true` or `false` |
| Boolean field in an object | Layout is JVM-dependent |
| `boolean[]` in HotSpot | Commonly uses approximately one byte per element |
| CPU execution | Hardware works with registers and memory units; it does not mean every boolean must occupy a full integer |

### Example

```java
boolean[] values = new boolean[5];
```

Conceptually:

```text
       BOOLEAN ARRAY
    ┌──────┬──────┬──────┬──────┬──────┐
    │  T   │  F   │  T   │  F   │  T   │
    └──────┴──────┴──────┴──────┴──────┘
       0      1      2      3      4
```

A HotSpot `boolean[]` commonly stores each element in one byte, but the complete array also has an object header and alignment overhead.

**Interview answer:**

Java does not mandate the physical storage size of a boolean. HotSpot commonly uses one byte per boolean array element, but the exact object layout is implementation-dependent.

---

# 14. Arrays Also Occupy Heap Memory

Consider:

```java
int[] numbers = new int[5];
```

The array itself is an object allocated on the heap.

```text
             STACK                         HEAP
       ┌────────────────┐          ┌─────────────────────┐
       │                │          │                     │
       │  numbers ─────────────────►│     int[]           │
       │                │          │                     │
       └────────────────┘          │  [0] = 0             │
                                   │  [1] = 0             │
                                   │  [2] = 0             │
                                   │  [3] = 0             │
                                   │  [4] = 0             │
                                   │                     │
                                   └─────────────────────┘
```

For a reference array:

```java
Student[] students = new Student[3];
```

The array contains three reference slots. It does not automatically create three Student objects.

```text
              HEAP
       ┌──────────────────────┐
       │   Student[] Array    │
       │                      │
       │   [0] = null         │
       │   [1] = null         │
       │   [2] = null         │
       └──────────────────────┘
```

Only after:

```java
students[0] = new Student();
```

does a separate Student object get created.

---

# 15. What Happens When an Object Is No Longer Reachable?

Java uses garbage collection to reclaim memory occupied by unreachable objects.

Example:

```java
Student s1 = new Student();

s1 = null;
```

Before assigning `null`:

```text
      STACK                       HEAP
    ┌────────┐              ┌───────────────┐
    │ s1 ──────────────────►│ Student       │
    └────────┘              └───────────────┘
```

After assigning `null`:

```text
      STACK                       HEAP
    ┌────────┐              ┌───────────────┐
    │ s1=null│              │ Student       │
    └────────┘              └───────────────┘
                                  |
                                  ▼
                          Potentially unreachable
```

The object may now be eligible for garbage collection if no other reachable reference points to it.

**Important:**

- `null` does not immediately destroy an object.
- Garbage collection timing is not guaranteed.
- An object is eligible for collection when it is no longer reachable from GC roots.
- Other references may still point to the same object.

---

# 16. StackOverflowError vs OutOfMemoryError

These are two different memory-related errors.

## StackOverflowError

Usually occurs when a thread's stack cannot grow enough to accommodate additional method frames.

Example:

```java
public class Main {

    static void recursiveMethod() {
        recursiveMethod();
    }

    public static void main(String[] args) {
        recursiveMethod();
    }
}
```

Each recursive invocation creates another method frame.

```text
       JAVA STACK
    ┌──────────────────┐
    │ recursiveMethod()│
    ├──────────────────┤
    │ recursiveMethod()│
    ├──────────────────┤
    │ recursiveMethod()│
    ├──────────────────┤
    │ recursiveMethod()│
    ├──────────────────┤
    │       ...        │
    └──────────────────┘
             |
             ▼
     StackOverflowError
```

## OutOfMemoryError

Occurs when the JVM cannot allocate memory needed for an operation, or cannot obtain other required runtime memory.

Example:

```java
import java.util.ArrayList;

public class Main {

    public static void main(String[] args) {

        ArrayList<int[]> list = new ArrayList<>();

        while (true) {
            list.add(new int[1_000_000]);
        }
    }
}
```

The program continually allocates arrays and retains references to them.

Eventually, it may throw:

```text
java.lang.OutOfMemoryError: Java heap space
```

### Difference

| StackOverflowError | OutOfMemoryError |
|---|---|
| Stack capacity is exceeded | Required memory cannot be allocated |
| Often caused by deep or infinite recursion | Can result from excessive allocation or retained objects |
| Associated with thread stack | Can involve heap or other JVM/native memory |
| Error | Error |

---

# 17. How to Inspect Actual Object Memory Size

So far, we have discussed conceptual layouts.

But how do we inspect the actual size of an object?

A useful Java tool is **JOL — Java Object Layout**.

JOL helps inspect object layouts, headers, field offsets, and memory footprint in a running JVM.

### Maven dependency

Add this to your `pom.xml`:

```xml
<dependency>
    <groupId>org.openjdk.jol</groupId>
    <artifactId>jol-core</artifactId>
    <version>0.17</version>
</dependency>
```

### Java example

```java
import org.openjdk.jol.info.ClassLayout;

class Student {

    int age;
    int rollNumber;
    double fee;
    boolean active;
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        System.out.println(
            ClassLayout.parseInstance(s1).toPrintable()
        );
    }
}
```

JOL prints details such as:

- Object header
- Field offsets
- Field sizes
- Internal gaps
- Object alignment and total size

**Important:** The output depends on the JVM running the program. This is exactly why we should inspect actual layouts instead of assuming fixed sizes.

---

# 18. Quick Revision — Stack vs Heap

| Feature | Stack | Heap |
|---|---|---|
| Purpose | Method execution | Object and array allocation |
| Ownership | Per thread | Shared |
| Stores | Method frames and local execution data | Objects and arrays |
| Lifetime | Frames exist during method execution | Objects can outlive creating methods |
| Memory management | Frames are pushed and popped | Garbage collector manages object reclamation |
| Common error | `StackOverflowError` | `OutOfMemoryError` can occur |
| Access | Method execution uses frame data | Objects are accessed through references |

---

# 19. Interview Questions

<details>
<summary>1. What is the difference between stack and heap memory in Java?</summary>

Stack memory is per-thread and stores method invocation frames. Heap memory is shared among threads and is used for object and array allocation.
</details>

<details>
<summary>2. Are primitive variables always stored in the stack?</summary>

No. Local primitive variables are associated with method frames, but primitive instance fields and array elements are part of heap-allocated objects and arrays. JVM optimizations can also change actual storage.
</details>

<details>
<summary>3. Are objects always stored in heap memory?</summary>

Objects and arrays are conceptually allocated from the JVM heap. However, JIT optimizations such as escape analysis may eliminate an allocation or its materialization in some cases.
</details>

<details>
<summary>4. What happens when we write Student s1 = new Student()?</summary>

The JVM ensures the class is initialized when required, allocates memory for the object, initializes its fields, executes the constructor, and assigns the resulting reference to `s1`.
</details>

<details>
<summary>5. What is an object header?</summary>

In HotSpot, an object header contains metadata such as the Mark Word and a class pointer. Its exact representation depends on the JVM implementation and configuration.
</details>

<details>
<summary>6. What is a Mark Word?</summary>

It is part of the HotSpot object header. It can encode information such as identity hash code, locking state, and garbage-collection age information.
</details>

<details>
<summary>7. What are compressed OOPs?</summary>

Compressed Ordinary Object Pointers are a HotSpot optimization that can represent object references using compressed values, commonly 4 bytes rather than 8 bytes.
</details>

<details>
<summary>8. Why is padding required?</summary>

Padding helps satisfy memory alignment requirements. It can make object access more efficient and ensure object sizes follow the JVM's alignment rules.
</details>

<details>
<summary>9. Is the object header always 12 bytes?</summary>

No. That is a common illustrative size for certain HotSpot configurations with compressed class pointers. It is not a Java language guarantee.
</details>

<details>
<summary>10. Does Java specify that a boolean occupies one byte?</summary>

No. The JVM specification does not define a fixed physical storage size for boolean values. HotSpot commonly uses one byte per boolean array element.
</details>

<details>
<summary>11. What happens when a reference variable is assigned null?</summary>

The reference no longer points to its previous object. If no other reachable references point to that object, it may become eligible for garbage collection.
</details>

<details>
<summary>12. What is the difference between StackOverflowError and OutOfMemoryError?</summary>

StackOverflowError generally occurs when a thread's stack cannot accommodate more frames. OutOfMemoryError occurs when the JVM cannot allocate required memory.
</details>

<details>
<summary>13. Can an object outlive the method in which it was created?</summary>

Yes. If a reference to the object escapes the method and remains reachable elsewhere, the object can continue to exist after the method returns.
</details>

<details>
<summary>14. Is the heap shared between threads?</summary>

Yes. The JVM heap is shared among application threads, while each thread has its own Java stack and PC register.
</details>

<details>
<summary>15. How can we inspect an object's actual memory layout?</summary>

We can use Java Object Layout (JOL) to inspect object headers, field offsets, gaps, and total object size in a particular running JVM.
</details>

---

# 20. Practical Exercises

1. Create a `Student` class with `int`, `double`, `boolean`, and `String` fields.
2. Create two Student objects and draw their references and heap layout.
3. Assign one reference variable to another and observe how changes are shared.
4. Set one reference to `null`. Check whether another reference still points to the object.
5. Create a method with local primitive and reference variables.
6. Create a class with a primitive instance field and a reference instance field.
7. Create a primitive array and a reference array. Compare their contents after initialization.
8. Write an infinite recursive method and observe `StackOverflowError`.
9. Use JOL to inspect the object layout of your Student class.
10. Change JVM options where practical and observe how object layout changes.

---

# 21. Quick Revision Checklist

- [ ] Explain JVM runtime memory areas.
- [ ] Explain stack memory and method frames.
- [ ] Explain heap memory.
- [ ] Explain stack vs heap with a diagram.
- [ ] Differentiate local variables and instance fields.
- [ ] Explain object allocation and constructor execution.
- [ ] Explain object headers and Mark Word.
- [ ] Explain compressed OOPs.
- [ ] Explain field memory and alignment.
- [ ] Explain memory padding with an example.
- [ ] Explain boolean storage.
- [ ] Explain garbage-collection eligibility.
- [ ] Differentiate StackOverflowError and OutOfMemoryError.
- [ ] Inspect an actual object using JOL.

---

# 22. Further Reading

- [Oracle — JVM Runtime Data Areas](https://docs.oracle.com/javase/specs/jvms/se21/html/jvms-2.html#jvms-2.5)
- [Oracle — Java Garbage Collection](https://docs.oracle.com/en/java/javase/21/gctuning/)
- [OpenJDK — Java Object Layout (JOL)](https://openjdk.org/projects/code-tools/jol/)
- [HotSpot Virtual Machine Guide](https://docs.oracle.com/en/java/javase/21/vm/)

**Previous document:** [Classes, Objects, and Constructors](../01-Classes-and-Objects/classes-objects-and-constructors.md)

**Next document:** [Encapsulation and Access Modifiers](../03-Encapsulation/encapsulation-and-access-modifiers.md)