# Stack and Heap Memory in Java

> **Interview definition — Stack:** Stack memory stores method-call frames, including local variables, parameters and intermediate execution information.
>
> **Interview definition — Heap:** Heap memory is the runtime area from which objects and arrays are allocated. It is managed by the garbage collector.

## 1. JVM Runtime Memory — Big Picture

```text
                    JVM MEMORY
                        |
        ┌───────────────┴────────────────┐
        |                                |
   Per-thread areas                 Shared areas
        |                                |
   ┌────┴────────┐               ┌───────┴────────┐
   |             |               |                |
Java Stack   PC Register        Heap          Method Area
   |                            |
Native Method Stack        Objects / Arrays
```

- Each thread has its own Java stack and PC register.
- Heap is shared among threads.
- Method Area is a JVM specification concept for class-level structures and runtime constants.
- JVM implementations decide the exact physical layout.

## 2. Stack Memory — Visual Representation

Every method invocation creates a stack frame.

```java
public class Main {
    static void greet() {
        int age = 25;
        System.out.println(age);
    }

    public static void main(String[] args) {
        int count = 10;
        greet();
    }
}
```

Conceptual stack while `greet()` is running:

```text
             THREAD STACK
        ┌────────────────────┐
        │ greet() frame      │  ← Current method
        │                    │
        │ age = 25           │
        ├────────────────────┤
        │ main() frame       │
        │                    │
        │ count = 10         │
        │ args = reference   │
        └────────────────────┘
```

When `greet()` finishes:
- Its frame is popped.
- Execution returns to `main()`.
- `main()` continues from where it called `greet()`.

**Remember:** Stack frames follow Last In, First Out (LIFO).

## 3. Heap Memory — Visual Representation

```java
public class Main {
    public static void main(String[] args) {
        int[] numbers = new int[3];
        numbers[0] = 10;
        numbers[1] = 20;
        numbers[2] = 30;
    }
}
```

Conceptual view:

```text
              STACK                         HEAP
       ┌─────────────────┐          ┌─────────────────────┐
       │ main() frame    │          │ int[] array object  │
       │                 │          │                     │
       │ numbers ────────┼─────────>│ [10] [20] [30]      │
       │                 │          │                     │
       └─────────────────┘          └─────────────────────┘
```

- The local reference `numbers` belongs to the method frame in this conceptual model.
- The array object is allocated on the heap.
- The array's elements are part of the array object.

## 4. Primitive vs Reference — Correct Mental Model

Do not memorize this incorrect shortcut:

```text
Incorrect:
Primitive  → Stack
Object     → Heap
```

Use this instead:

```text
Where the variable is declared and used matters.

Local primitive variable
       |
       v
Method frame (typically stack)

Object field / array element
       |
       v
Part of an object or array (typically heap)

Local reference variable
       |
       v
Method frame (typically stack)
       |
       v
Referenced object (heap)
```

Example:

```java
class Student {
    int age = 25;
}

public class Main {
    public static void main(String[] args) {
        int count = 10;
        Student student = new Student();
    }
}
```

Conceptual view:

```text
STACK                              HEAP
┌──────────────────────┐           ┌───────────────────┐
│ main() frame         │           │ Student object    │
│                      │           │                   │
│ count = 10           │           │ age = 25           │
│ student ─────────────┼──────────>│                   │
└──────────────────────┘           └───────────────────┘
```

- `count` is a local primitive.
- `student` is a local reference.
- The `Student` object contains the `age` field.
- The object and its field are on the heap in this normal object-allocation model.

## 5. What Happens When One Object References Another?

```java
class Address {
    String city = "Ahmedabad";
}

class Student {
    Address address = new Address();
}
```

Conceptually:

```text
STACK                       HEAP
┌──────────────┐            ┌──────────────────┐
│ student ─────┼───────────>│ Student object   │
└──────────────┘            │ address ─────────┼────┐
                            └──────────────────┘    |
                                                    v
                                           ┌────────────────┐
                                           │ Address object │
                                           │ city = ...     │
                                           └────────────────┘
```

An object can contain references to other objects. This forms an object graph.

## 6. What Happens When a Method Calls Another Method?

```java
static void first() {
    int a = 10;
    second();
}

static void second() {
    int b = 20;
}
```

```text
Before second() call:

┌──────────────────┐
│ first() frame    │
│ a = 10           │
└──────────────────┘

While second() runs:

┌──────────────────┐
│ second() frame   │ ← Top of stack
│ b = 20           │
├──────────────────┤
│ first() frame    │
│ a = 10           │
└──────────────────┘

After second() returns:

┌──────────────────┐
│ first() frame    │
│ a = 10           │
└──────────────────┘
```

## 7. Recursion and Stack Overflow

Every recursive method call creates another invocation frame.

```java
static void countDown(int n) {
    if (n == 0) {
        return;
    }

    countDown(n - 1);
}
```

```text
countDown(3)
    |
    v
countDown(2)
    |
    v
countDown(1)
    |
    v
countDown(0) → return
```

If recursion continues without a valid stopping condition, the stack can run out of space.

```text
Too many method calls
        |
        v
Stack capacity exhausted
        |
        v
StackOverflowError
```

**Interview point:** `StackOverflowError` is an `Error`, not an ordinary exception.

## 8. What Happens to Objects When They Are No Longer Used?

Java uses garbage collection to reclaim memory occupied by objects that are no longer reachable.

```java
Student student = new Student();
student = null;
```

Conceptually:

```text
Before:
student ──────────> Student object

After:
student = null

STACK                 HEAP
┌────────────┐        ┌─────────────────┐
│ student    │        │ Student object  │
│ null       │        │ unreachable     │
└────────────┘        └─────────────────┘
                             |
                             v
                      Eligible for GC
```

**Important:**
- Setting a reference to `null` does not immediately destroy the object.
- An object becomes eligible for garbage collection when it is no longer reachable through GC roots.
- Garbage collection timing is not guaranteed.

## 9. Stack vs Heap

| Feature | Stack | Heap |
|---|---|---|
| Main purpose | Method execution and frames | Objects and arrays |
| Ownership | Per thread | Shared |
| Stores | Local variables, parameters, frame data | Objects and arrays |
| Lifetime | Usually tied to method invocation | Until unreachable and reclaimed |
| Management | Frames are pushed/popped | Garbage collector manages objects |
| Typical error | `StackOverflowError` | `OutOfMemoryError` |
| Access | Method-local execution | Through references |

## 10. Boolean Memory — What Is Its Actual Size?

**Interview answer:** Java's language specification does not define a fixed storage size for a `boolean`. Its storage representation depends on the JVM implementation and context.

### Important distinctions

| Context | What to remember |
|---|---|
| Java language | Only defines `true` and `false` values |
| JVM specification | Does not mandate a universal memory size for boolean variables |
| JVM bytecode | Uses integer-like computational handling for boolean values |
| HotSpot `boolean[]` | Commonly uses one byte per element, not one bit |
| Object fields / local variables | Physical storage and layout depend on implementation and optimization |

### Why Is It Commonly One Byte Rather Than One Bit?

- CPUs and memory systems operate on addressable units, commonly bytes.
- A byte is a practical unit for storing and addressing a single boolean value.
- Bit-packing multiple booleans would require extra bit extraction and update operations.
- JVM implementations may choose simpler byte-based storage for boolean arrays.

### A Useful CPU Clarification

It is not accurate to say that a CPU always fetches exactly one byte at a time.

- Modern processors use cache lines, often much larger than one byte.
- Memory systems move data in blocks.
- A single boolean read can involve fetching a larger cache line.
- The JVM and JIT compiler can optimize how values are stored and accessed.

**Interview trap:** Do not claim that every Java boolean always occupies exactly 1 byte. Say that HotSpot commonly represents boolean array elements using one byte, but Java does not guarantee a universal boolean storage size.

## 11. Common Memory Terms

| Term | Simple definition |
|---|---|
| Stack frame | Memory structure created for a method invocation |
| Heap | Memory area for objects and arrays |
| Reference | A value used to access an object |
| Garbage Collection | Automatic reclamation of unreachable objects |
| GC Root | A starting point used to determine object reachability |
| Stack Overflow | Stack runs out of available space |
| Heap exhaustion | JVM cannot allocate more heap memory |
| Object graph | Network of objects connected through references |

## 12. Interview Questions — Click to Reveal

<details>
<summary><strong>1. What is stack memory?</strong></summary>

Stack memory stores method invocation frames, including local variables, parameters and execution-related data. Each thread has its own Java stack.
</details>

<details>
<summary><strong>2. What is heap memory?</strong></summary>

Heap is the shared JVM runtime area from which objects and arrays are allocated. The garbage collector manages the reclamation of unreachable objects.
</details>

<details>
<summary><strong>3. Are all primitive variables stored on the stack?</strong></summary>

No. Local primitive variables are typically represented in a method frame, but primitive instance fields and array elements are part of their containing objects or arrays, which are normally allocated on the heap.
</details>

<details>
<summary><strong>4. Where is an array stored?</strong></summary>

An array is an object and is allocated on the heap. A local variable referring to it is typically part of the method's stack frame.
</details>

<details>
<summary><strong>5. What happens when a method is called?</strong></summary>

A new method invocation frame is created on the current thread's stack. When the method returns, that frame is removed.
</details>

<details>
<summary><strong>6. What is StackOverflowError?</strong></summary>

It occurs when a thread's Java stack cannot accommodate further method invocation frames, often because of excessive or unbounded recursion.
</details>

<details>
<summary><strong>7. Does setting an object reference to null immediately free memory?</strong></summary>

No. It removes that reference. If the object is no longer reachable from any GC root, it becomes eligible for garbage collection, but collection timing is not guaranteed.
</details>

<details>
<summary><strong>8. Is heap memory shared between threads?</strong></summary>

Yes. The heap is shared by JVM threads, while each thread has its own Java stack.
</details>

<details>
<summary><strong>9. What is the size of boolean in Java?</strong></summary>

Java does not specify a universal storage size for boolean. The implementation and context determine its physical representation. HotSpot commonly uses one byte per boolean array element.
</details>

<details>
<summary><strong>10. Does the CPU fetch only one byte for a boolean?</strong></summary>

No. Modern processors commonly transfer data in cache-line-sized blocks. The size of a Java boolean and the amount of data fetched from memory are different concepts.
</details>

## 13. Quick Revision Checklist

- [ ] JVM runtime memory overview
- [ ] Stack frames and LIFO
- [ ] Heap and object allocation
- [ ] Primitive vs reference storage
- [ ] Method calls and recursion
- [ ] Garbage collection and reachability
- [ ] StackOverflowError vs OutOfMemoryError
- [ ] Boolean storage nuance
- [ ] Thread-local vs shared memory

## Related Files

- [Arrays in Java](../01-Arrays/arrays-in-java.md)
- [Methods and Recursion](../03-Methods-and-Recursion/methods-recursion-and-overloading.md)