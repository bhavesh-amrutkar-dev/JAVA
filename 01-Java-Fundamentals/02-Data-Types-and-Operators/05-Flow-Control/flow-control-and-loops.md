# Flow Control in Java

> **Revision goal:** Understand selection statements, switch, loops, jump statements and methods.

## 1. What Is Flow Control?

Flow control determines the order in which program statements execute.

```text
              Flow Control
                   |
       ┌───────────┼───────────┐
       |           |           |
   Selection   Iteration    Jump
   (Decision)   (Loops)    (Transfer)
       |           |           |
   if / else    for / while  break
   switch       do-while     continue
                            return
```

## 2. Selection Statements

Used when a program must make a decision.

### A. `if`

```java
int age = 20;

if (age >= 18) {
    System.out.println("Adult");
}
```

### B. `if-else`

```java
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

### C. `if-else-if` Ladder

```java
int marks = 85;

if (marks >= 90) {
    System.out.println("A");
} else if (marks >= 75) {
    System.out.println("B");
} else if (marks >= 50) {
    System.out.println("C");
} else {
    System.out.println("Fail");
}
```

**Remember:**
- Conditions must evaluate to `boolean`.
- Conditions are checked from top to bottom.
- The first matching branch executes.
- Remaining branches are skipped.

## 3. Switch Statement

Useful when one expression is compared against multiple discrete alternatives.

```java
int day = 2;

switch (day) {
    case 1:
        System.out.println("Monday");
        break;

    case 2:
        System.out.println("Tuesday");
        break;

    default:
        System.out.println("Invalid day");
}
```

### Why Does Switch Exist If We Already Have If-Else?

| If-Else Ladder                            | Switch                                    |
| ----------------------------------------- | ----------------------------------------- |
| Good for ranges and complex conditions    | Good for discrete choices                 |
| Supports different boolean conditions     | Matches one selector against alternatives |
| Can become lengthy for many exact choices | Often clearer for many exact choices      |
| Conditions can vary                       | Cases share one selector                  |

**Example:**

Use `if-else` for ranges:
```java
if (marks >= 90) {
    System.out.println("A");
} else if (marks >= 75) {
    System.out.println("B");
}
```

Use `switch` for exact values:
```java
switch (role) {
    case "ADMIN" -> System.out.println("Full access");
    case "USER"  -> System.out.println("Limited access");
    default      -> System.out.println("Unknown role");
}
```

**Interview points:**
- Traditional switch statements can fall through to the next case if `break` is omitted.
- `default` runs when no case matches.
- Modern switch supports arrow labels (`->`), which do not fall through.
- Switch expressions can return a value.
- Supported selector types include integral types (except `long`), `char`, their wrappers, `String` and enums; newer Java versions also support pattern matching in switch.

## 4. Switch Fall-Through vs Arrow Syntax

Traditional syntax:

```java
int choice = 1;

switch (choice) {
    case 1:
        System.out.println("One");
    case 2:
        System.out.println("Two");
        break;
    default:
        System.out.println("Other");
}
```

Output:
```text
One
Two
```

Why? No `break` after case 1, so execution falls through.

Arrow syntax:

```java
switch (choice) {
    case 1 -> System.out.println("One");
    case 2 -> System.out.println("Two");
    default -> System.out.println("Other");
}
```

Only the matching branch executes; no fall-through.

## 5. Iteration — Loops

Loops execute a block repeatedly while a condition or iteration rule allows it.

Java provides:
1. `for`
2. `while`
3. `do-while`
4. Enhanced `for` (for-each)

### A. For Loop

Best when the iteration count or update rule is known.

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

```text
Initialization
      |
      v
  Condition? <─────────────┐
   /     \                 |
 true    false             |
  |        |               |
  v        v               |
 Execute   Exit            |
  |                        |
  v                        |
 Update ───────────────────┘
```

### B. While Loop

Best when the number of iterations is not known in advance.

```java
int i = 1;

while (i <= 5) {
    System.out.println(i);
    i++;
}
```

- Checks condition before executing.
- May execute zero times.

### C. Do-While Loop

```java
int i = 1;

do {
    System.out.println(i);
    i++;
} while (i <= 5);
```

- Executes the body first.
- Checks the condition afterward.
- Always executes at least once.

### D. Enhanced For Loop

Used to iterate over arrays and iterable collections.

```java
int[] numbers = {10, 20, 30};

for (int number : numbers) {
    System.out.println(number);
}
```

**Limitation:** It is not designed for direct index-based access or arbitrary index updates.

## 6. Loop Comparison

| Loop           | Condition check | Minimum executions | Typical use                 |
| -------------- | --------------- | -----------------: | --------------------------- |
| `for`          | Before body     |                  0 | Known iteration count       |
| `while`        | Before body     |                  0 | Condition-based repetition  |
| `do-while`     | After body      |                  1 | Execute at least once       |
| Enhanced `for` | Per iteration   |                  0 | Traverse arrays/collections |

## 7. Jump Statements

Jump statements alter normal control flow.

### A. `break`

Terminates the nearest enclosing loop or switch.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        break;
    }

    System.out.println(i);
}
```

Output:
```text
1
2
```

### B. `continue`

Skips the remaining statements in the current iteration and moves to the next iteration.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;
    }

    System.out.println(i);
}
```

Output:
```text
1
2
4
5
```

### C. `return`

Exits the current method. It can optionally return a value.

```java
static int add(int a, int b) {
    return a + b;
}
```

```text
break     → Exit loop/switch
continue  → Skip current iteration
return    → Exit current method
```

**Extra:** Java supports labelled `break` and `continue` for controlling an enclosing labelled loop.

## 8. Functions in Java — Methods

In Java, functions declared inside a class are called **methods**.

```java
public class Calculator {

    static int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        int result = add(10, 20);
        System.out.println(result);
    }
}
```

### Method Components

```java
static int add(int a, int b) {
    return a + b;
}
```

| Component      | Meaning                |
| -------------- | ---------------------- |
| `static`       | Belongs to the class   |
| `int`          | Return type            |
| `add`          | Method name            |
| `int a, int b` | Parameters             |
| `return`       | Sends result to caller |

**Remember:**
- `void` means no value is returned.
- Parameters are variables in the method declaration.
- Arguments are actual values passed during a call.
- A static method can be called using the class name.
- An instance method is called using an object.
- Method overloading means same method name with different parameter lists.

## 9. Common Interview Traps

| Question                                                  | Answer                                                                      |
| --------------------------------------------------------- | --------------------------------------------------------------------------- |
| Can an `if` condition use an integer directly?            | No                                                                          |
| Does `while` always execute once?                         | No                                                                          |
| Does `do-while` execute at least once?                    | Yes                                                                         |
| What happens if `break` is missing in traditional switch? | Fall-through                                                                |
| Does arrow switch fall through?                           | No                                                                          |
| Can `continue` be used in a switch by itself?             | No; it applies to loops                                                     |
| Does `return` terminate only a loop?                      | No; it exits the method                                                     |
| Can a method return multiple values directly?             | One return value; use an object/array/collection to package multiple values |

## 10. Quick Revision

- Selection → `if`, `if-else`, `switch`.
- Iteration → `for`, `while`, `do-while`, enhanced `for`.
- Jump → `break`, `continue`, `return`.
- `for` → commonly used for known iteration patterns.
- `while` → condition checked before body.
- `do-while` → body runs at least once.
- `break` → terminates nearest loop or switch.
- `continue` → skips current loop iteration.
- `return` → exits current method.
- Switch → clearer for multiple discrete choices.

## 11. Interview Questions

1. Why do we need switch when if-else exists?
2. Difference between `while` and `do-while`?
3. Difference between `break` and `continue`?
4. Can we use `break` inside nested loops?
5. What is switch fall-through?
6. Difference between traditional switch and arrow switch?
7. What is an enhanced for loop?
8. Can an enhanced for loop access an array index directly?
9. What is the difference between a parameter and an argument?
10. What is method overloading?
11. What happens when a non-void method does not return a value on every required path?
12. Can `return` be used inside a loop?

## Related Files

- [Operators](../04-Operators/operators-in-java.md)
- [Type Conversion](../03-Type-Conversion/type-conversion-and-promotion.md)