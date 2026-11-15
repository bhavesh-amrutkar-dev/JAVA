# Java Fundamentals – Practice Questions

## 1. Objective

This document contains practical and conceptual exercises covering the complete Introduction to Java section.

The goal is to check whether you can:

- Explain Java fundamentals.
- Understand platform independence.
- Use JDK tools.
- Explain JVM internals.
- Trace Java execution.
- Understand memory management.
- Inspect bytecode.
- Answer interview-style scenarios.

---

# Section 1: Java Fundamentals

### Q1. What is Java?

Explain Java in your own words.

Your explanation should cover:

- Programming language.
- Bytecode.
- JVM.
- Platform independence.

### Q2. Why is Java platform-independent?

Explain with a diagram how the same `.class` file can be executed on Windows and Linux.

### Q3. What is WORA?

Explain what Write Once, Run Anywhere means and mention two conditions required for it to work.

### Q4. Is Java purely object-oriented?

Explain why or why not.

### Q5. Explain five important features of Java.

For each feature, provide:

- Definition.
- Why it matters.
- One practical example.

### Q6. Java vs C++

Compare the two languages based on:

- Compilation.
- Memory management.
- Platform dependency.
- Pointers.
- Multiple inheritance.

---

# Section 2: JDK, JRE, and JVM

### Q7. Differentiate JDK, JRE, and JVM.

Draw a diagram showing their conceptual relationship.

### Q8. Why does JVM need to be platform-dependent if Java is platform-independent?

Explain with an example.

### Q9. Can we execute a compiled Java application without installing the full JDK?

Explain your answer.

### Q10. What is the difference between `java` and `javac`?

Write one command example for each.

### Q11. What are the responsibilities of the JDK?

Name at least five development tools.

---

# Section 3: Compilation and Execution

### Q12. Explain the complete Java execution flow.

Start from:

```text
Main.java
```

End at:

```text
Program Output
```

Your diagram should include:

- `javac`.
- Bytecode.
- JVM.
- Class Loader.
- Runtime Data Areas.
- Execution Engine.
- Interpreter.
- JIT Compiler.

### Q13. What happens internally when we execute?

```powershell
java Main
```

Explain the major steps.

### Q14. Does `java Main` compile the source file?

Explain the difference between normal class launching and Java source-file mode.

### Q15. What is the difference between a compiler and an interpreter?

Use Java as your example.

### Q16. Why does Java use both an Interpreter and JIT Compiler?

### Q17. What is JVM warm-up?

Explain why it matters when benchmarking Java applications.

---

# Section 4: Class Loader

### Q18. What is a Class Loader?

Explain its responsibility in JVM Architecture.

### Q19. Explain the three major Class Loaders.

Draw their hierarchy.

### Q20. Explain the Parent Delegation Model.

Why is it useful?

### Q21. Explain the following phases:

1. Loading.
2. Verification.
3. Preparation.
4. Resolution.
5. Initialization.

### Q22. Consider:

```java
class Student {

    static int count = 10;

    static {
        count = 20;
    }
}
```

Answer:

1. What value does `count` receive during preparation?
2. When does the explicit assignment to `10` happen?
3. What value does `count` have after successful initialization?

### Q23. Can two Class Loaders load classes with the same fully qualified name?

Explain why those classes may be treated as different types.

---

# Section 5: Runtime Data Areas

### Q24. Draw the complete JVM Runtime Data Areas diagram.

Divide it into:

- Shared memory.
- Per-thread memory.

### Q25. Differentiate Heap and Stack.

Explain using a Java program.

### Q26. Analyze the following code:

```java
class Student {
    String name;
}

public class Main {

    public static void main(String[] args) {

        int age = 25;

        Student student = new Student();

        student.name = "Alex";
    }
}
```

Answer:

1. Where is `age` maintained?
2. Where is the local reference `student` maintained?
3. Where is the Student object allocated?
4. Where does the `name` instance field belong?

### Q27. What is a Stack Frame?

Explain what happens when one method calls another method.

### Q28. What is the difference between Method Area and Metaspace?

### Q29. What is the purpose of the PC Register?

### Q30. What is the Native Method Stack?

---

# Section 6: Garbage Collection

### Q31. What is Garbage Collection?

Explain why Java needs it.

### Q32. When does an object become eligible for Garbage Collection?

Consider:

```java
Student s1 = new Student();

Student s2 = s1;

s1 = null;
```

Is the object eligible for GC? Explain.

### Q33. What happens here?

```java
Student student = new Student();

student = null;
```

Is the object immediately destroyed?

### Q34. What are GC Roots?

Name four examples of references or runtime structures that can act as GC roots.

### Q35. Differentiate:

- StackOverflowError.
- OutOfMemoryError.

### Q36. What is a memory leak in Java?

Give one practical example.

### Q37. Does `System.gc()` guarantee that Garbage Collection will happen?

Explain.

---

# Section 7: Execution Engine

### Q38. What is the responsibility of the Execution Engine?

### Q39. Differentiate Interpreter and JIT Compiler.

### Q40. What is hot code?

### Q41. What is runtime profiling?

### Q42. Explain method inlining with an example.

### Q43. What is deoptimization?

### Q44. Differentiate JIT and AOT compilation.

### Q45. Name two JIT optimizations and explain them.

---

# Section 8: JNI and Native Libraries

### Q46. What is JNI?

Why do we need it?

### Q47. What does the `native` keyword indicate?

Write a simple Java declaration.

### Q48. Differentiate:

```java
System.load();
```

and

```java
System.loadLibrary();
```

### Q49. Why are native libraries platform-dependent?

### Q50. What is the difference between JNI, JNA, and FFM?

---

# Section 9: Practical Exercises

## Exercise 1: Compile and Execute

Create:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Learning JVM Architecture");

    }
}
```

Tasks:

- Compile the program.
- Find the generated `.class` file.
- Execute it.
- Inspect the Java and compiler versions.

## Exercise 2: Multiple Classes

Create:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }
}

public class Main {

    public static void main(String[] args) {

        Calculator calculator = new Calculator();

        System.out.println(calculator.add(10, 20));
    }
}
```

Tasks:

- Compile both classes.
- Observe generated `.class` files.
- Execute the program.
- Explain which classes are loaded when required.

## Exercise 3: Inspect Bytecode

Use the Calculator example.

Run:

```powershell
javap -c Calculator
```

Find:

- Integer load instructions.
- `iadd`.
- Integer return instruction.

Explain what each instruction does.

## Exercise 4: Static Initialization

Write:

```java
public class Main {

    static int count = 10;

    static {
        System.out.println("Static block executed");
        count = 20;
    }

    public static void main(String[] args) {

        System.out.println(count);
    }
}
```

Tasks:

- Predict the output.
- Execute the program.
- Explain when class initialization happens.
- Explain preparation vs initialization.

## Exercise 5: Stack Frames

Write:

```java
public class Main {

    static void third() {
        System.out.println("Third");
    }

    static void second() {
        third();
    }

    static void first() {
        second();
    }

    public static void main(String[] args) {
        first();
    }
}
```

Tasks:

- Draw the Stack Frame sequence while `third()` is executing.
- Explain which frame gets removed first.
- Explain what happens when execution returns to `main()`.

## Exercise 6: Object Reachability

Write:

```java
class Student {
    String name;
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();

        Student s2 = s1;

        s1 = null;

        s2 = null;
    }
}
```

Tasks:

- Draw the references and object.
- Explain when the object becomes eligible for GC.
- Explain why eligibility does not mean immediate collection.

## Exercise 7: Infinite Recursion

Write:

```java
public class Main {

    static void test() {
        test();
    }

    public static void main(String[] args) {
        test();
    }
}
```

Tasks:

- Execute it carefully.
- Observe the error.
- Explain why it occurs.
- Relate it to Java Stack and Stack Frames.

## Exercise 8: Classpath

Create this structure:

```text
project/
├── src/
│   └── com/
│       └── example/
│           └── Main.java
└── out/
```

Tasks:

- Compile with `javac -d out`.
- Run using `java -cp out com.example.Main`.
- Explain why the fully qualified class name is required.

---

# Section 10: Scenario-Based Interview Questions

### Scenario 1: Java Works on Your Machine but Not on Another Machine

A Java application runs successfully on Windows but fails on Linux.

What could be the reason?

Consider:

- JVM availability.
- Java version compatibility.
- Classpath.
- Native libraries.
- OS-specific dependencies.

### Scenario 2: Application Memory Keeps Growing

A Java backend application has increasing memory consumption.

What would you investigate?

Consider:

- Object reachability.
- Static collections.
- Caches.
- Heap dumps.
- Garbage Collection logs.

### Scenario 3: Application Is Slow Initially

A Java service is slower immediately after startup but improves after processing requests.

What JVM behavior could explain this?

### Scenario 4: Class Loading Failure

An application compiles successfully but throws `NoClassDefFoundError` in production.

What would you investigate?

Consider:

- Missing dependencies.
- Runtime classpath.
- Class-loader behavior.
- Deployment packaging.

### Scenario 5: Application Throws StackOverflowError

What are the likely causes?

How would you investigate the issue?

### Scenario 6: Application Throws OutOfMemoryError: Metaspace

What does this error indicate?

What could cause excessive class metadata growth?

### Scenario 7: Application Uses Native Code

A Java application uses a C++ library.

What additional deployment and maintenance concerns should the team consider?

---

# Section 11: Draw JVM Architecture from Memory

Without opening your notes, draw the following diagrams:

### Diagram 1: Complete JVM Architecture

Include:

- Class Loader Subsystem.
- Runtime Data Areas.
- Execution Engine.
- JNI.
- Native Libraries.

### Diagram 2: Class Loading

Include:

- Loading.
- Linking.
- Verification.
- Preparation.
- Resolution.
- Initialization.

### Diagram 3: Runtime Data Areas

Include:

- Heap.
- Method Area.
- Java Stack.
- PC Register.
- Native Method Stack.

### Diagram 4: Execution Engine

Include:

- Interpreter.
- JIT Compiler.
- Garbage Collector.

### Diagram 5: Java Execution Flow

Start from `.java` and end at native machine execution.

---

# Section 12: Final Self-Assessment

Use this checklist after completing the entire Introduction to Java section.

## Java Basics

- [ ] What is Java?
- [ ] Features of Java.
- [ ] Platform dependency and portability.
- [ ] Bytecode and WORA.

## Java Platform

- [ ] JVM.
- [ ] JRE.
- [ ] JDK.
- [ ] JDK vs JRE vs JVM.

## Java Execution

- [ ] Compiler vs Interpreter.
- [ ] Java execution flow.
- [ ] JIT Compiler.

## JVM Architecture

- [ ] Complete JVM Architecture.
- [ ] Class Loader.
- [ ] Runtime Data Areas.
- [ ] Execution Engine.
- [ ] JNI and Native Libraries.

## Practical Knowledge

- [ ] Compile and execute a Java program.
- [ ] Compile multiple Java classes.
- [ ] Work with packages and classpath.
- [ ] Inspect bytecode using `javap`.
- [ ] Understand common JVM errors.

## Interview Readiness

- [ ] Answer basic questions without notes.
- [ ] Explain intermediate JVM concepts.
- [ ] Explain advanced JVM concepts.
- [ ] Solve scenario-based questions.
- [ ] Draw JVM diagrams from memory.

---

# Section 13: Suggested Completion Strategy

Do not attempt all 50 conceptual questions in one sitting.

Follow this order:

| Day | Activity |
|---|---|
| Day 1 | Java basics, features, portability |
| Day 2 | JDK, JRE, JVM |
| Day 3 | Compilation and execution |
| Day 4 | Class Loader and class-loading phases |
| Day 5 | Runtime Data Areas |
| Day 6 | Garbage Collection and Execution Engine |
| Day 7 | JNI, bytecode inspection, revision |
| Day 8 | Basic and intermediate interview questions |
| Day 9 | Advanced questions and scenario-based practice |
| Day 10 | Full JVM architecture explanation from memory |

Adjust the pace depending on your understanding.

---

# Final Goal

After completing this section, I should be able to explain the following without memorizing a script:

**"What happens internally when I execute a Java program?"**

I should be able to start with a `.java` file, explain compilation into bytecode, describe class loading and runtime memory, explain how the Execution Engine runs the program, and discuss Garbage Collection and native integration.

That is the real measure of understanding Java fundamentals.