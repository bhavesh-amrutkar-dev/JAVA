# Data Types, Variables and Keywords in Java

> **Revision goal:** Know Java's data types, their sizes and ranges, how variables store values, and how identifiers, literals and keywords differ.

## 1. Data Types in Java

A data type defines:
- What kind of value a variable can store.
- The operations that can be performed on that value.
- The range or structure of the value.

Java data types are divided into two categories:

```text
                 Java Data Types
                 /             \
          Primitive          Non-Primitive
          (8 types)           (Reference)
          /       \                |
       Numeric   Boolean        String
        /   \                   Arrays
   Integral Floating           Classes
                               Interfaces
                               Enums
```

## 2. Primitive Data Types — Size and Range

| Type | Width | Range / Values |
|---|---:|---|
| `byte` | 8 bits | -128 to 127 |
| `short` | 16 bits | -32,768 to 32,767 |
| `int` | 32 bits | -2³¹ to 2³¹ - 1 |
| `long` | 64 bits | -2⁶³ to 2⁶³ - 1 |
| `float` | 32 bits | Approx. 6–7 decimal digits of precision |
| `double` | 64 bits | Approx. 15–16 decimal digits of precision |
| `char` | 16 bits | 0 to 65,535 (UTF-16 code unit) |
| `boolean` | JVM-dependent storage | `true` or `false` |

**Remember:**
- Integral: `byte`, `short`, `int`, `long`
- Floating-point: `float`, `double`
- Character: `char`
- Logical: `boolean`
- `int` is the default type for integer literals.
- `double` is the default type for decimal literals.
- Use `L` for a long literal when needed: `100L`.
- Use `F` for a float literal: `10.5F`.
- Java does not specify a universal memory size for a `boolean` variable.

## 3. Primitive vs Non-Primitive

| Primitive | Non-Primitive |
|---|---|
| Stores a primitive value | Stores a reference to an object |
| Eight built-in types | Includes classes, arrays, interfaces, etc. |
| Cannot be `null` | Can be `null` |
| Has a defined value range (where applicable) | Depends on the referenced type |
| Stack m/m | Heap m/m |
| Examples: `int`, `char` | Examples: `String`, arrays |


```java
int age = 25;
String name = "Java";
int[] numbers = {10, 20, 30};
```

**Important:** A reference variable stores a reference, not the entire object itself.

## 4. Why Unicode Instead of ASCII?

| ASCII | Unicode |
|---|---|
| Originally 7-bit standard | Universal character encoding standard |
| 128 basic character codes | Supports characters from many writing systems |
| Limited language support | Supports multilingual text |
| Insufficient for global applications | Suitable for international applications |

**Why Java uses Unicode:**
- Java was designed for portability and international use.
- `char` represents a 16-bit UTF-16 code unit.
- Some Unicode characters require two `char` values (a surrogate pair).
- A Java `String` can represent characters beyond the basic ASCII range.

```java
char letter = 'A';
char hindi = 'अ';

System.out.println(letter);
System.out.println(hindi);
```

**Interview point:** Unicode is not the same thing as UTF-8 or UTF-16. Unicode defines characters and code points; UTF-8 and UTF-16 are encoding schemes.

## 5. Why Is `boolean` Not 0 or 1 Like C++?

Java:
```java
boolean isActive = true;
// boolean isActive = 1; // Compilation error
```

C++:
```cpp
bool isActive = 1; // Valid; converted to true
```

**Java rule:**
- `boolean` accepts only `true` or `false`.
- It is not implicitly convertible to an integer.
- `if` conditions must be boolean expressions.

```java
int count = 10;

// Invalid in Java:
// if (count) { }

// Valid:
if (count > 0) {
    System.out.println("Positive");
}
```

**Why?** Java enforces strict type checking and avoids treating numeric values as logical conditions.

## 6. Variable, Literal, Identifier and Container

| Term | Meaning | Example |
|---|---|---|
| Variable | Named storage location with a declared type | `age` |
| Literal | Fixed value written directly in code | `25`, `"Java"` |
| Identifier | Name given to a program element | `age`, `calculateTotal` |
| Container | Informal analogy for a variable holding a value | `int age = 25;` |

```java
int age = 25;
```

Breakdown:
- `int` → data type
- `age` → identifier and variable name
- `25` → integer literal
- `=` → assignment operator

**Important:** A variable is not always a physical box containing the complete value. For reference types, it holds a reference.

## 7. Keywords in Java — Is the Count 168?

**No. 168 is not the number of Java reserved keywords.**

- Java has a fixed set of reserved keywords defined by its language specification.
- The commonly quoted count is **50 reserved keywords**; `_` became a keyword in Java 9, making the count **51** under that counting convention.
- Contextual keywords such as `var`, `record`, `sealed`, `permits`, `yield` and `module` have special meaning in particular contexts, but are not all reserved keywords.
- `true`, `false` and `null` are literals, not keywords.
- `const` and `goto` are reserved but unused.

Examples:
```java
int class = 10; // Invalid: class is a keyword
int age = 25;   // Valid
```

**Interview trap:** Do not mix reserved keywords, contextual keywords and literals when answering the keyword-count question. The exact count depends on the language version and what category is being counted.

## 8. Quick Revision

| Question | Remember |
|---|---|
| Number of primitive types? | 8 |
| Default integer literal type? | `int` |
| Default decimal literal type? | `double` |
| `char` size? | 16 bits |
| `boolean` size? | Not fixed by the JVM specification |
| Can boolean store 0 or 1? | No |
| Is String primitive? | No |
| Does a reference variable store the full object? | No, it stores a reference |
| Is `true` a keyword? | No, it is a boolean literal |
| Is `_` a Java keyword? | Yes, since Java 9 |

## 9. Practical Checks

Try predicting the output or compilation result:

```java
public class DataTypeCheck {
    public static void main(String[] args) {
        byte age = 127;
        // byte nextAge = 128; // Error: out of range

        char letter = 'A';
        boolean active = true;

        float price = 10.5F;
        double salary = 45000.75;

        System.out.println(age);
        System.out.println(letter);
        System.out.println(active);
        System.out.println(price);
        System.out.println(salary);
    }
}
```

## 10. Interview Questions

1. Why are there eight primitive data types in Java?
2. Why is `char` 16 bits?
3. Is Java completely Unicode-based?
4. Why does Java not allow `if (1)`?
5. What is the difference between a primitive variable and a reference variable?
6. Is `String` primitive or non-primitive?
7. What is the difference between a keyword and an identifier?
8. Are `true`, `false` and `null` keywords?
9. Why do we write `10.5F` instead of `10.5` when assigning to a `float`?
10. Is the memory size of a `boolean` fixed?

## Related Files

- [Number Representation](../02-Number-Representation/number-representation-in-memory.md)
- [Type Conversion](../03-Type-Conversion/type-conversion-and-promotion.md)
- [Operators](../04-Operators/operators-in-java.md)