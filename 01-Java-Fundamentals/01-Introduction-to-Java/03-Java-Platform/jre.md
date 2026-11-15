# Java Runtime Environment (JRE)

## 1. Introduction

The **Java Runtime Environment (JRE)** provides the environment and components required to run Java applications.

Conceptually, it consists of:

- JVM.
- Java platform class libraries.
- Other supporting runtime components and resources.

The JRE provides what is needed to run Java applications, rather than the complete set of tools needed to develop them.

**In simple terms:** JRE is the runtime environment required to execute Java applications.

---

## 2. Why do we need JRE?

Imagine a developer has already developed and compiled a Java application.

The developer provides the compiled application to another person.

Does that person need the Java compiler to run the application?

No.

The person needs a compatible Java runtime environment.

The JRE conceptually provides:
- JVM to execute bytecode.
- Java libraries required by the application.
- Supporting runtime components.

### Basic architecture

```text
          Java Runtime Environment
                     |
          +----------+----------+
          |                     |
          v                     v
         JVM             Java Libraries
          |
          v
    Executes Bytecode
```

---

## 3. Components of JRE

### 3.1 JVM

The JVM loads, verifies and executes Java bytecode.

It also manages runtime memory, threads and garbage collection.

See [JVM](./jvm.md) for the detailed explanation.

### 3.2 Java Class Libraries

Java provides standard libraries that developers use instead of implementing everything from scratch.

Examples:

| Package | Purpose |
|---|---|
| `java.lang` | Fundamental language classes |
| `java.util` | Collections and utility classes |
| `java.io` | Input and output |
| `java.nio` | Modern file and buffer operations |
| `java.net` | Networking |
| `java.time` | Date and time API |

Example:

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Java");
        names.add("Python");

        System.out.println(names);
    }
}
```

The `ArrayList` class comes from Java's standard library.

### 3.3 Supporting Runtime Components

A Java runtime distribution also contains supporting resources and components required by the JVM and standard libraries.

The exact contents depend on the Java distribution and version.

---

## 4. How does JRE work?

Suppose we have a compiled Java application.

```text
             Main.class
                  |
                  v
           Java Runtime
                (JRE)
                  |
         +--------+--------+
         |                 |
         v                 v
        JVM          Java Libraries
         |
         v
    Executes Bytecode
         |
         v
       Output
```

The runtime provides the JVM and the libraries needed by the application.

---

## 5. Does JRE contain a compiler?

Traditionally, a JRE did not include development tools such as the Java compiler.

Its purpose was to provide a runtime environment.

| Tool | Traditionally included in JRE? |
|---|---|
| JVM | Yes |
| Java standard libraries | Yes |
| `javac` compiler | No |
| `javadoc` | No |
| `jar` development tool | No |

The JDK provides the development tools.

**Modern Java note:** Since Java 11, Oracle no longer provides a separate traditional JRE download as part of its standard JDK distribution. Other vendors and distributions may package runtimes differently. The JRE remains useful as a conceptual component of the Java platform.

---

## 6. Real-World Example

Imagine a company develops a Java-based banking application.

The development team:
1. Writes Java source code.
2. Compiles the source code.
3. Packages the application.
4. Deploys it to a server.

The production server needs a compatible Java runtime to execute the application.

It does not necessarily need the full development tools used by the developers.

This separation helps distinguish development requirements from runtime requirements.

---

## 7. JRE vs JVM

These terms are related but not interchangeable.

| JVM | JRE |
|---|---|
| Executes bytecode | Provides the runtime environment |
| Handles memory and execution | Includes JVM and runtime libraries conceptually |
| Core execution component | Broader runtime environment |
| Part of the JRE concept | Contains the JVM conceptually |

---

## 8. Interview Questions

### Basic

1. What is JRE?
2. Why do we need JRE?
3. What are the main components of JRE?
4. Does JRE traditionally contain the Java compiler?
5. Can we run Java applications using a runtime without development tools?

### Intermediate

1. Explain the relationship between JVM and JRE.
2. What is the role of Java class libraries?
3. Why does a production server not necessarily need the full JDK?
4. What is the difference between JRE and JVM?

### Advanced / Tricky

1. Is JRE a separate downloadable product in modern Java distributions?
2. Can a JRE execute bytecode without a JVM?
3. Does the JRE itself execute Java instructions?
4. Why is the JRE considered a runtime environment rather than a compiler?

---

## 9. Practice Questions

### Conceptual

1. Explain JRE from first principles.
2. Explain the relationship between JVM and JRE.
3. Why are Java class libraries required?
4. Differentiate runtime and development environments.

### Scenario-Based

**Scenario 1:**

A developer sends a compiled Java application to a production server.

- Does the server need the Java compiler?
- What does the server need to execute the application?

**Scenario 2:**

A Java application uses `ArrayList` and `LocalDate`.

- Where do these classes come from?
- Why does the runtime need Java libraries?

**Scenario 3:**

Someone says JRE and JVM are the same.

- Is the statement correct?
- Explain their relationship.

---

## 10. Quick Revision

| Question | Answer |
|---|---|
| What is JRE? | Java Runtime Environment |
| Main purpose? | Running Java applications |
| Contains JVM conceptually? | Yes |
| Contains Java libraries? | Yes |
| Traditionally contains `javac`? | No |
| Is it required for execution? | A compatible Java runtime is required |
| Is a separate JRE download always available? | No; modern distributions vary |

---

## 11. My Understanding

**What is JRE?**

*Write your explanation here.*

**Why does JRE need JVM?**

*Write your explanation here.*

**Why does a production server not necessarily need JDK?**

*Write your explanation here.*

---

## 12. Self-Assessment Checklist

- [ ] I understand what JRE is.
- [ ] I understand its purpose.
- [ ] I know the relationship between JRE and JVM.
- [ ] I understand the role of Java class libraries.
- [ ] I know why a runtime does not need all development tools.
- [ ] I understand the modern Java distribution distinction.

---

## 13. Related Topics

- [JVM](./jvm.md)
- [JDK](./jdk.md)
- [JVM vs JRE vs JDK](./jvm-vs-jre-vs-jdk.md)
- [Java Execution Flow](../04-Java-Execution/java-execution-flow.md)

---

**Last Revised:** YYYY-MM-DD

**Next Topic:** Java Development Kit (JDK)