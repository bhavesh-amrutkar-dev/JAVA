# Type Conversion and Type Promotion in Java

> **Revision goal:** Understand widening, narrowing, casting, truncation and automatic type promotion in expressions.

## 1. What Is Type Conversion?

Converting a value from one data type to another.

```text
Type Conversion
      |
      ├── Widening (Implicit)
      |
      └── Narrowing (Explicit)
```

## 2. Widening Conversion — Implicit

Smaller compatible type → larger compatible type.

- Done automatically by Java.
- Generally preserves the value.
- No explicit cast is required.

```text
byte → short → int → long → float → double
                 ↑
               char
```

More precisely, `char` can widen to `int`, `long`, `float` and `double`, but not to `short` or `byte`.

Example:

```java
int number = 100;
long bigNumber = number;

double price = 25; // int literal widened to double
```

**Important:** Widening does not always preserve every bit of precision. For example, converting a large `long` to `float` can lose precision.

## 3. Narrowing Conversion — Explicit

Larger type → smaller type.

- Requires explicit casting in ordinary assignments.
- May lose information.
- Can cause overflow or truncation.

```java
double price = 99.99;
int wholePrice = (int) price;

System.out.println(wholePrice); // 99
```

```text
double (99.99)
      |
      | Explicit cast: (int)
      v
int (99)
```

## 4. Truncating Conversion

When a floating-point value is converted to an integer:
- The fractional part is removed.
- It does not round to the nearest integer.
- Truncation is toward zero.

```java
System.out.println((int) 9.8);   // 9
System.out.println((int) -9.8);  // -9
```

**Interview trap:** `(int) 9.8` is `9`, not `10`.

## 5. Boolean Conversion

Java does not allow conversion between `boolean` and numeric types.

```java
boolean active = true;

// Invalid:
// int value = (int) active;
// boolean result = 1;
```

Use conditions instead:

```java
int number = 1;
boolean result = number != 0;
```

## 6. Automatic Type Promotion in Expressions

Java promotes smaller integral types when performing arithmetic.

**Rule:**
- `byte`, `short` and `char` are promoted to `int` in most arithmetic expressions.
- If either operand is `double`, the operation is performed as `double`.
- Otherwise, if either is `float`, it is performed as `float`.
- Otherwise, if either is `long`, it is performed as `long`.
- Otherwise, it is performed as `int`.

```java
byte a = 10;
byte b = 20;

// byte result = a + b; // Error
int result = a + b;     // Valid
```

Why? `a + b` is evaluated as an `int`.

## 7. Type Promotion Hierarchy

```text
byte ─┐
short ├──────> int ──────> long ──────> float ──────> double
char ─┘
```

This is a useful revision model for arithmetic promotion, not a rule that every assignment between adjacent types is automatic.

## 8. Casting Examples

```java
public class TypeCasting {
    public static void main(String[] args) {

        // Widening
        int a = 100;
        double b = a;

        // Narrowing
        double x = 99.99;
        int y = (int) x;

        // Arithmetic promotion
        byte p = 10;
        byte q = 20;
        int sum = p + q;

        System.out.println(b);   // 100.0
        System.out.println(y);   // 99
        System.out.println(sum); // 30
    }
}
```

## 9. Special Cases to Remember

| Expression | Result / Rule |
|---|---|
| `byte + byte` | `int` |
| `short + short` | `int` |
| `char + char` | `int` |
| `int + long` | `long` |
| `int + float` | `float` |
| `long + double` | `double` |
| `double → int` | Explicit cast; fraction removed |
| `boolean → int` | Not allowed |

**Compound assignment exception:**

```java
byte number = 10;
number += 5; // Valid
```

This is allowed because compound assignment includes an implicit conversion back to the left-hand type.

Compare:

```java
byte number = 10;
// number = number + 5; // Error
number = (byte) (number + 5); // Valid
```

## 10. Quick Revision

- Widening → implicit conversion.
- Narrowing → explicit casting.
- Floating-point to integer → truncation toward zero.
- Boolean cannot convert to or from numeric types.
- `byte`, `short`, `char` → promoted to `int` in arithmetic.
- Expression result type can differ from the variable operand types.
- Compound assignment performs an implicit conversion to the left-hand type.

## 11. Interview Questions

1. What is widening and narrowing conversion?
2. Is widening always lossless?
3. Why does `byte + byte` return an `int`?
4. What is truncation?
5. Can boolean be converted to int?
6. What is automatic type promotion?
7. Why does `byte b = 10; b = b + 1;` fail?
8. Why does `b += 1` work?
9. What is the result of `(int) -9.8`?
10. What happens when a large integer is narrowed to a smaller type?

## Related Files

- [Data Types](../01-Data-Types-and-Variables/data-types-variables-and-keywords.md)
- [Number Representation](../02-Number-Representation/number-representation-in-memory.md)
- [Operators](../04-Operators/operators-in-java.md)