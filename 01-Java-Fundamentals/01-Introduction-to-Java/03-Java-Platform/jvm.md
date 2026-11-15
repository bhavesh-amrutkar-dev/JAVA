# Java Virtual Machine (JVM)

## 1. Introduction

The **Java Virtual Machine (JVM)** is an abstract machine that provides the runtime environment required to execute Java bytecode.

It acts as an intermediary between Java bytecode and the underlying operating system and hardware.

The JVM is responsible for:
- Loading compiled Java classes.
- Verifying bytecode.
- Managing runtime memory.
- Executing bytecode.
- Managing threads.
- Performing garbage collection.
- Optimizing execution using JIT compilation.

**In simple terms:** JVM is the engine that runs Java bytecode.

---

## 2. Why do we need JVM?

Consider a Java program:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java!");
    }
}
```

When we compile it:

```bash
javac Main.java
```

The compiler generates:

```text
Main.class
```

This file contains bytecode.

However, the physical CPU does not directly understand Java bytecode.

The JVM provides the environment that loads and executes this bytecode.

### Without JVM

The CPU cannot directly execute Java bytecode.

### With JVM

```text
          Java Source Code
                 |
                 v
           Java Compiler
                 |
                 v
           Java Bytecode
              (.class)
                 |
                 v
                JVM
                 |
                 v
        Native Machine Instructions
                 |
                 v
                CPU
```

---

## 3. JVM Architecture

The JVM architecture consists of several important components.

```text
                    JVM
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
  Class Loader  Runtime Data   Execution Engine
                  Areas              |
       |             |         +-----+------+
       |             |         |            |
       |             |         v            v
       |             |    Interpreter   JIT Compiler
       |             |         |            |
       |             |         +-----+------+
       |             |               |
       |             |               v
       |             |         Native Code
       |             |
       +-------------+
                     |
              Garbage Collector

       JNI and Native Libraries
```

### 3.1 Class Loader Subsystem

The Class Loader loads compiled `.class` files into JVM memory.

It performs three major activities:

1. **Loading:** Finds and loads class bytecode.
2. **Linking:** Prepares the class for execution.
3. **Initialization:** Executes static initializers and assigns initial values to static fields.

Linking itself includes:
- Verification.
- Preparation.
- Resolution.

#### Types of Class Loaders

| Class Loader | Responsibility |
|---|---|
| Bootstrap Class Loader | Loads core Java platform classes |
| Platform Class Loader | Loads platform classes and modules |
| Application Class Loader | Loads application classes from the classpath or module path |

### 3.2 Runtime Data Areas

These are memory areas used while the program executes.

| Memory Area | Purpose |
|---|---|
| Heap | Stores objects and arrays |
| Java Stack | Stores method frames and local variables |
| Method Area | Stores class-level structures and runtime metadata |
| PC Register | Tracks the current JVM instruction for each thread |
| Native Method Stack | Supports execution of native methods |

**Important:**

- Heap is shared among JVM threads.
- Each thread has its own Java stack.
- Each thread has its own PC register.
- Method Area is a logical JVM specification concept; HotSpot implements its class metadata storage using Metaspace.

### 3.3 Execution Engine

The execution engine executes the bytecode loaded into the JVM.

It primarily uses:

**Interpreter**

- Executes bytecode instructions.
- Processes instructions as the program runs.
- Can begin execution without waiting for compilation of the entire method.

**JIT Compiler (Just-In-Time Compiler)**

- Identifies frequently executed code.
- Compiles hot code into native machine code.
- Applies runtime optimizations.
- Improves performance for frequently executed code paths.

### 3.4 Garbage Collector (GC)

The Garbage Collector automatically manages heap memory.

It identifies objects that are no longer reachable from GC roots and reclaims their memory when collection occurs.

Example:

```java
public class Main {
    public static void main(String[] args) {
        Main obj = new Main();

        obj = null;
    }
}
```

After `obj = null`, the object may become eligible for garbage collection if no other references exist.

**Important:** Becoming eligible for garbage collection does not mean the object is immediately removed.

### 3.5 Java Native Interface (JNI)

JNI allows Java code to interact with native code written in languages such as C and C++.

It is useful when Java applications need to access platform-specific functionality or existing native libraries.

---

## 4. How JVM executes a Java program

Consider:

```java
public class Main {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;

        int sum = a + b;

        System.out.println(sum);
    }
}
```

Execution process:

1. `javac` compiles `Main.java`.
2. The compiler generates `Main.class`.
3. The JVM starts and initializes its runtime.
4. The Class Loader loads the required classes.
5. The bytecode verifier checks bytecode validity.
6. Runtime memory areas are used during execution.
7. The execution engine executes bytecode.
8. The program produces output.

---

## 5. Is JVM platform-independent?

No. JVM implementations are platform-dependent.

The JVM specification defines how a JVM should behave, but the actual implementation must work with the underlying operating system and processor architecture.

For example:

```text
           Same Java Bytecode
                  |
       +----------+----------+
       |          |          |
       v          v          v
   Windows JVM Linux JVM  macOS JVM
       |          |          |
       v          v          v
   Windows OS  Linux OS   macOS
```

Each JVM implementation can execute compatible Java bytecode on its target platform.

**Remember:**
- Bytecode is platform-independent.
- JVM implementations are platform-dependent.
- JVM enables Java's platform independence.

---

## 6. JVM vs JVM Specification

These two terms are related but different.

| JVM Specification | JVM Implementation |
|---|---|
| Defines how a JVM should behave | Actual software implementing the specification |
| Describes class files and execution rules | Executes bytecode |
| Does not depend on one specific implementation | Built for a target environment |
| Defines requirements | Provides the actual runtime |

Examples of JVM implementations include HotSpot and OpenJ9.

---

## 7. Interview Questions

### Basic

1. What is JVM?
2. What is the main responsibility of JVM?
3. Can CPU directly execute Java bytecode?
4. What are the main components of JVM?
5. What is the role of the Class Loader?

### Intermediate

1. Explain JVM architecture.
2. What are Runtime Data Areas?
3. What is the difference between Heap and Stack?
4. What is the role of the Execution Engine?
5. What is JIT compilation?
6. What is Garbage Collection?
7. What is JNI?

### Advanced / Tricky

1. If Java is platform-independent, why is JVM platform-dependent?
2. Is JVM a physical machine?
3. Is Method Area the same as Metaspace?
4. Does every thread have its own Java stack?
5. Can an object be eligible for garbage collection while the program is still running?
6. What is the difference between JVM Specification and JVM implementation?

---

## 8. Practice Questions

### Conceptual

1. Explain JVM architecture using a diagram.
2. Explain how a `.class` file is executed.
3. Differentiate Heap and Stack memory.
4. Explain the Class Loader's three major activities.
5. Explain the role of the interpreter and JIT compiler.

### Scenario-Based

**Scenario 1:**

An object is created inside a method, and the method finishes execution.

- Where was the object stored?
- Where was its local reference stored?
- Is the object automatically removed when the method finishes?

**Scenario 2:**

A Java application executes a frequently called method thousands of times.

- How can the JVM optimize this method?
- What role does JIT compilation play?

**Scenario 3:**

A Java application runs on Windows and Linux using the same bytecode.

- Does both operating systems use the same JVM binary?
- Why are separate JVM implementations needed?

---

## 9. Quick Revision

| Component | Responsibility |
|---|---|
| Class Loader | Loads classes |
| Bytecode Verifier | Checks bytecode validity |
| Heap | Stores objects and arrays |
| Java Stack | Stores method frames |
| Method Area | Stores class-level runtime structures |
| PC Register | Tracks current instruction per thread |
| Execution Engine | Executes bytecode |
| Interpreter | Executes bytecode instructions |
| JIT Compiler | Compiles hot code into native code |
| Garbage Collector | Reclaims memory from unreachable objects |
| JNI | Connects Java with native code |

---

## 10. My Understanding

**What is JVM?**

*Write your explanation here.*

**Why is JVM platform-dependent?**

*Write your explanation here.*

**Explain JVM architecture.**

*Write your explanation here.*

---

## 11. Self-Assessment Checklist

- [ ] I understand what JVM is.
- [ ] I understand why JVM is required.
- [ ] I can explain JVM architecture.
- [ ] I understand Class Loader.
- [ ] I understand Runtime Data Areas.
- [ ] I can differentiate Heap and Stack.
- [ ] I understand the Execution Engine.
- [ ] I understand Interpreter and JIT.
- [ ] I understand Garbage Collection.
- [ ] I can explain JNI.

---

## 12. Related Topics

- [What is Java?](../01-Java-Basics/what-is-java.md)
- [Bytecode and WORA](../02-Java-Portability/bytecode-and-wora.md)
- [JRE](./jre.md)
- [JDK](./jdk.md)
- [JVM vs JRE vs JDK](./jvm-vs-jre-vs-jdk.md)
- [JVM Architecture](../05-JVM-Architecture/jvm-architecture.md)

---

**Last Revised:** YYYY-MM-DD

**Next Topic:** Java Runtime Environment (JRE)