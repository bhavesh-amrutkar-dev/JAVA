# JVM vs JRE vs JDK

## 1. Introduction

JVM, JRE and JDK are three fundamental concepts in the Java ecosystem.

They are closely related, but each has a different responsibility.

| Term | Full Form | Main Responsibility |
|---|---|---|
| JVM | Java Virtual Machine | Executes Java bytecode |
| JRE | Java Runtime Environment | Provides the environment to run Java applications |
| JDK | Java Development Kit | Provides tools to develop and run Java applications |

**Remember:**

- JVM = Execution engine.
- JRE = Runtime environment.
- JDK = Development kit.

---

## 2. Understanding their relationship

Traditionally, the relationship was represented as:

```text
                 JDK
                  |
                  v
                 JRE
                  |
                  v
                 JVM
```

A more detailed conceptual representation:

```text
+---------------------------------------+
|                  JDK                  |
|                                       |
|  +---------------------------------+  |
|  |              JRE                |  |
|  |                                 |  |
|  |  +---------------------------+  |  |
|  |  |            JVM            |  |  |
|  |  +---------------------------+  |  |
|  |                                 |  |
|  |       Java Runtime Libraries    |  |
|  +---------------------------------+  |
|                                       |
|  Development Tools:                   |
|  javac, javadoc, jar, jdb, javap      |
+---------------------------------------+
```

**Important modern Java distinction:**

This diagram represents the conceptual relationship, not necessarily the physical folder structure of a modern JDK.

Since Java 9, the Java platform uses a modular runtime architecture. Since Java 11, Oracle no longer provides a separate traditional JRE download as a standard JDK companion. Other vendors may package runtime distributions differently.

---

## 3. What does each component do?

### 3.1 JVM — Java Virtual Machine

JVM is responsible for executing Java bytecode.

Its responsibilities include:

- Loading classes.
- Verifying bytecode.
- Managing runtime memory.
- Executing instructions.
- Garbage collection.
- Runtime optimization.

Example:

```text
Main.class
    |
    v
   JVM
    |
    v
Program Execution
```

### 3.2 JRE — Java Runtime Environment

JRE represents the environment required to run Java applications.

Conceptually, it includes:

- JVM.
- Java standard libraries.
- Supporting runtime components.

It does not traditionally include development tools such as `javac`.

Example:

```text
Java Application
       |
       v
      JRE
       |
       v
      JVM
       |
       v
    Execution
```

### 3.3 JDK — Java Development Kit

JDK provides tools required to develop Java applications.

It includes:
- Java compiler (`javac`).
- Java launcher (`java`).
- Debugger (`jdb`).
- Documentation generator (`javadoc`).
- Archive tool (`jar`).
- Runtime components.

Example:

```text
Java Source Code
       |
       v
      JDK
       |
       v
     javac
       |
       v
   Bytecode
       |
       v
      JVM
       |
       v
    Execution
```

---

## 4. Detailed Comparison

| Feature | JVM | JRE | JDK |
|---|---|---|---|
| Full form | Java Virtual Machine | Java Runtime Environment | Java Development Kit |
| Primary purpose | Execute bytecode | Run applications | Develop and run applications |
| Executes bytecode | Yes | Through JVM | Through its runtime |
| Contains JVM | It is the JVM | Conceptually yes | Provides JVM/runtime |
| Java libraries | Uses them | Provides them | Provides them |
| Java compiler (`javac`) | No | Traditionally no | Yes |
| Debugging tools | No development toolkit | Traditionally no | Yes |
| Documentation tools | No | Traditionally no | Yes |
| Intended user | Runtime environment | Application execution | Developers |

---

## 5. Real-World Analogy

Imagine building a house.

| Java Concept | House Analogy |
|---|---|
| JVM | Machinery that operates the house's systems |
| JRE | The completed house with everything needed to live in it |
| JDK | The complete construction toolkit used to build the house |

The analogy is not exact, but it helps distinguish the responsibilities.

Another way to remember:

- JVM runs.
- JRE provides what is needed to run.
- JDK provides what developers need to build and run.

---

## 6. Practical Example

Suppose you want to develop a Java application.

### Step 1: Write source code

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java!");
    }
}
```

Save it as:

```text
Main.java
```

### Step 2: Compile using JDK

```bash
javac Main.java
```

The compiler generates:

```text
Main.class
```

### Step 3: Execute using the Java runtime

```bash
java Main
```

Output:

```text
Hello Java!
```

### What happened?

| Activity | Component |
|---|---|
| Writing source code | Developer |
| Compiling source code | JDK compiler (`javac`) |
| Generating bytecode | Java compiler |
| Providing runtime libraries | Java runtime |
| Loading and executing bytecode | JVM |

---

## 7. Common Interview Traps

### Question 1: Can we run Java applications without JDK?

Yes.

A compatible Java runtime is sufficient to execute an already compiled Java application.

The full development toolkit is not necessarily required.

### Question 2: Can we develop Java applications without JDK?

Not with the standard Java development toolchain.

The JDK provides tools such as `javac` to compile Java source code.

### Question 3: Is JVM part of JRE?

Conceptually, yes.

JVM is the core execution component of the Java runtime environment.

### Question 4: Is JRE part of JDK?

Traditionally, yes.

The JDK included the JRE and additional development tools.

However, modern Java distributions may not package a separate JRE directory.

### Question 5: Which one is platform-independent?

Java bytecode is designed to be platform-independent.

JVM implementations are platform-dependent.

JDK distributions are also platform-specific because they contain platform-specific runtime components and tools.

### Question 6: Does JVM compile Java source code?

No.

The standard Java compiler (`javac`) compiles Java source code into bytecode.

The JVM executes bytecode and may use JIT compilation to generate native machine code at runtime.

---

## 8. Interview Questions

### Basic

1. What is the difference between JVM, JRE and JDK?
2. What is the full form of JVM, JRE and JDK?
3. Which one is required to compile Java code?
4. Which one is responsible for executing bytecode?
5. Does JRE traditionally contain a compiler?

### Intermediate

1. Explain the relationship between JVM, JRE and JDK.
2. Can we run Java applications without installing the full JDK?
3. Why does JDK contain runtime components?
4. Why is JVM platform-dependent?
5. What is the difference between `java` and `javac`?

### Advanced / Tricky

1. Is the traditional JDK → JRE → JVM diagram an exact description of modern Java installations?
2. Why did modern Java distributions move away from the traditional standalone JRE model?
3. Can we create a custom runtime image without the complete JDK development toolkit?
4. Does the JVM specification require every JVM implementation to use the same internal architecture?
5. What is the difference between a JVM implementation and the JVM specification?

---

## 9. Practice Questions

### Conceptual

1. Explain JVM, JRE and JDK without referring to the notes.
2. Draw their relationship using a diagram.
3. Explain why a developer needs JDK but an application user may only need a runtime.
4. Explain the difference between compilation and execution.

### Scenario-Based

**Scenario 1:**

A developer has installed only a Java runtime and wants to compile a `.java` file.

- Will `javac` necessarily be available?
- Which package provides the compiler?

**Scenario 2:**

A production server needs to execute an already compiled Java application.

- Does it need the complete development toolkit?
- What runtime capabilities are required?

**Scenario 3:**

A developer says JVM, JRE and JDK are exactly the same because all are related to Java.

- Why is this incorrect?
- Explain the responsibility of each.

**Scenario 4:**

You install a modern JDK but cannot find a separate `jre` folder.

- Does this necessarily mean the installation is incomplete?
- How has modern Java packaging changed?

---

## 10. Quick Revision

<text weight="semibold">The easiest way to remember:</text>

| Component | Remember it as |
|---|---|
| JVM | Executes bytecode |
| JRE | Provides runtime environment |
| JDK | Provides development tools and runtime |

### One-line definitions

- **JVM:** Executes Java bytecode.
- **JRE:** Provides the components required to run Java applications.
- **JDK:** Provides the tools required to develop, compile and run Java applications.

### Commands to remember

```bash
# Check Java runtime version
java -version

# Check Java compiler version
javac -version

# Compile Java source code
javac Main.java

# Execute Java application
java Main

# Inspect bytecode
javap -c Main
```

---

## 11. My Understanding

Try explaining these without looking at the notes.

**What is JVM?**

*Write your explanation here.*

**What is JRE?**

*Write your explanation here.*

**What is JDK?**

*Write your explanation here.*

**How are all three connected?**

*Write your explanation here.*

---

## 12. Self-Assessment Checklist

- [ ] I understand JVM.
- [ ] I understand JRE.
- [ ] I understand JDK.
- [ ] I can explain the relationship between all three.
- [ ] I know which component compiles Java code.
- [ ] I know which component executes bytecode.
- [ ] I understand the modern Java packaging model.
- [ ] I can answer interview questions confidently.

---

## 13. Related Topics

- [JVM](./jvm.md)
- [JRE](./jre.md)
- [JDK](./jdk.md)
- [Java Portability Problem](../02-Java-Portability/portability-problem.md)
- [Bytecode and WORA](../02-Java-Portability/bytecode-and-wora.md)
- [Java Execution Flow](../04-Java-Execution/java-execution-flow.md)

---

**Last Revised:** YYYY-MM-DD

**Next Topic:** Compiler vs Interpreter