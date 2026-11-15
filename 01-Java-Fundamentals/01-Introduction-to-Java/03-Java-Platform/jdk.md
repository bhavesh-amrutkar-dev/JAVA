# Java Development Kit (JDK)

## 1. Introduction

The **Java Development Kit (JDK)** is a software development kit used to develop, compile, debug, package and run Java applications.

It provides the tools required by Java developers.

The JDK includes:
- Java compiler.
- Java runtime components.
- JVM.
- Java standard libraries.
- Development and diagnostic tools.

**In simple terms:** JDK provides everything required to develop Java applications and the runtime components needed to execute them.

---

## 2. Why do we need JDK?

Imagine you want to develop a Java application.

You need to:

1. Write Java source code.
2. Compile source code into bytecode.
3. Execute the compiled application.
4. Debug errors.
5. Package the application for distribution.

The JDK provides tools for these activities.

Without the compiler, you cannot compile Java source code using the standard Java development toolchain.

---

## 3. Components of JDK

Conceptually:

```text
             Java Development Kit
                       |
          +------------+------------+
          |                         |
          v                         v
      Runtime Components       Development Tools
          |                         |
          v                         v
         JVM                    javac
     Java Libraries              javadoc
                                  jar
                                  jdb
```

### 3.1 JVM

Executes Java bytecode.

It manages runtime memory, class loading, execution and garbage collection.

### 3.2 Java Runtime Libraries

Provide reusable classes and APIs used by Java applications.

Examples:
- `java.lang`
- `java.util`
- `java.io`
- `java.net`
- `java.time`

### 3.3 Java Compiler (`javac`)

Converts Java source code into bytecode.

Example:

```bash
javac Main.java
```

Output:

```text
Main.class
```

### 3.4 Java Launcher (`java`)

Launches a Java application.

Example:

```bash
java Main
```

### 3.5 Javadoc

Generates documentation from Java source code and documentation comments.

### 3.6 JAR Tool

Creates and manages Java Archive files.

Example:

```bash
jar --create --file app.jar Main.class
```

### 3.7 Java Debugger (`jdb`)

Provides debugging capabilities for Java applications.

---

## 4. Important JDK Tools

| Tool | Purpose |
|---|---|
| `javac` | Compiles Java source code |
| `java` | Launches Java applications |
| `javadoc` | Generates API documentation |
| `jar` | Creates and manages JAR files |
| `jdb` | Debugger |
| `javap` | Displays class-file information |
| `jshell` | Interactive Java REPL |
| `jlink` | Creates custom runtime images |
| `jdeps` | Analyzes class and module dependencies |

These tools are commonly available in modern JDK distributions, though exact tools can vary by version and vendor.

---

## 5. How JDK works in development

Consider a developer writing:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Learning JDK");
    }
}
```

### Step 1: Write code

Create:

```text
Main.java
```

### Step 2: Compile code

Use the compiler:

```bash
javac Main.java
```

This produces:

```text
Main.class
```

### Step 3: Execute code

Use the Java launcher:

```bash
java Main
```

Output:

```text
Learning JDK
```

### Complete flow

```text
          Developer
              |
              v
         Main.java
              |
              v
        javac Compiler
              |
              v
         Main.class
              |
              v
         java Launcher
              |
              v
             JVM
              |
              v
          Execution
              |
              v
           Output
```

---

## 6. JDK and JRE relationship

Traditionally, the JDK was distributed with a separate JRE directory.

Conceptually:

```text
                 JDK
                  |
                  v
                 JRE
                  |
                  v
                 JVM
```

The JDK provides development tools along with the runtime components.

**Modern Java note:** Since Java 9, the modular Java platform changed how runtime components are packaged. Since Java 11, a separate traditional JRE is no longer included as a standard separate Oracle download. The exact packaging depends on the vendor and distribution.

For learning purposes, the conceptual relationship remains useful:

- JVM executes bytecode.
- JRE represents the runtime environment.
- JDK provides the development toolchain and runtime capabilities.

---

## 7. How to check JDK installation

### Check Java version

```bash
java -version
```

### Check compiler version

```bash
javac -version
```

### Check Java installation path

Windows:

```powershell
where.exe java
where.exe javac
```

Linux/macOS:

```bash
which java
which javac
```

### Important distinction

If `java -version` works but `javac -version` does not, it may mean:
- Only a runtime is available.
- A JDK is installed but its `bin` directory is not configured in PATH.
- Multiple Java installations are conflicting.

Do not conclude that the JDK is missing until you check the installation and PATH configuration.

---

## 8. Why do developers use JDK?

The JDK is needed for:

1. Developing Java applications.
2. Compiling source code.
3. Debugging applications.
4. Generating documentation.
5. Packaging applications.
6. Inspecting bytecode and dependencies.
7. Building custom runtime images.

---

## 9. Interview Questions

### Basic

1. What is JDK?
2. Why do we need JDK?
3. What does `javac` do?
4. What does the `java` command do?
5. What is the purpose of the JAR tool?

### Intermediate

1. What are the main components of JDK?
2. Explain the relationship between JDK, JRE and JVM.
3. Can we develop Java applications without a JDK?
4. What is the difference between `java` and `javac`?
5. What is the purpose of `javap`?

### Advanced / Tricky

1. Does installing a JDK always mean a separate JRE directory will exist?
2. Can a JDK be used to run Java applications?
3. What happens if `java` works but `javac` does not?
4. What is the difference between a JDK and a custom runtime image?
5. Why did modern Java distributions move away from the traditional separate JRE packaging model?

---

## 10. Practice Questions

### Conceptual

1. Explain JDK from first principles.
2. List the important tools provided by JDK.
3. Explain how JDK helps developers build Java applications.
4. Explain the difference between compilation and execution.

### Practical

1. Check your installed Java version.
2. Check your installed compiler version.
3. Compile a simple Java program.
4. Execute the compiled program.
5. Inspect the generated bytecode using `javap -c Main`.
6. Find the location of `java` and `javac` on your system.

### Scenario-Based

**Scenario 1:**

You want to develop a Java application but have installed only a runtime environment without development tools.

- Can you compile the Java source code using `javac`?
- Which development kit do you need?

**Scenario 2:**

Your terminal recognizes `java` but does not recognize `javac`.

- What could be the reason?
- How would you troubleshoot it?

**Scenario 3:**

A production server only needs to execute a Java application.

- Does it necessarily need every JDK development tool?
- What runtime components does it need?

---

## 11. Quick Revision

| Question | Answer |
|---|---|
| What is JDK? | Java Development Kit |
| Main purpose? | Develop and run Java applications |
| Contains compiler? | Yes |
| Compiler command? | `javac` |
| Java launcher? | `java` |
| Documentation tool? | `javadoc` |
| Archive tool? | `jar` |
| Debugger? | `jdb` |
| Bytecode inspection? | `javap` |
| Interactive shell? | `jshell` |

---

## 12. My Understanding

**What is JDK?**

*Write your explanation here.*

**Why does a Java developer need JDK?**

*Write your explanation here.*

**What is the difference between `java` and `javac`?**

*Write your explanation here.*

---

## 13. Self-Assessment Checklist

- [ ] I understand what JDK is.
- [ ] I understand why developers need JDK.
- [ ] I know the purpose of `javac`.
- [ ] I know the purpose of `java`.
- [ ] I understand the important JDK tools.
- [ ] I can compile and execute a Java program.
- [ ] I can inspect bytecode.
- [ ] I understand the modern JDK packaging model.

---

## 14. Related Topics

- [JVM](./jvm.md)
- [JRE](./jre.md)
- [JVM vs JRE vs JDK](./jvm-vs-jre-vs-jdk.md)
- [Java Execution Flow](../04-Java-Execution/java-execution-flow.md)
- [Compile and Run](../06-Practical/compile-and-run.md)

---

**Last Revised:** YYYY-MM-DD

**Next Topic:** JVM vs JRE vs JDK