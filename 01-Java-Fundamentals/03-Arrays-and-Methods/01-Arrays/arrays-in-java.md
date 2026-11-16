# Arrays in Java

> **Interview definition:** An array in Java is a fixed-size object that stores multiple values of the same declared element type. Its elements are accessed using zero-based indexes.

## 1. Declaring and Creating an Array

```java
int[] rollnums = new int[3];
```

Breakdown:

| Part       | Meaning                              |
| ---------- | ------------------------------------ |
| `int`      | Element data type                    |
| `[]`       | Declares an array reference          |
| `rollnums` | Identifier / reference variable      |
| `new`      | Creates an array object              |
| `int[3]`   | Creates space for 3 integer elements |

**Important:** `int` is the element type. The array object is created at runtime, and the variable `rollnums` holds a reference to it.

## 2. Array Initialization

```java
// Declaration
int[] numbers;

// Creation
numbers = new int[3];

// Initialization
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;

// Declaration + creation + initialization
int[] values = {10, 20, 30};
```

Default values:

| Element type                   | Default value |
| ------------------------------ | ------------- |
| `byte`, `short`, `int`, `long` | `0`           |
| `float`, `double`              | `0.0`         |
| `char`                         | `'u0000'`    |
| `boolean`                      | `false`       |
| Reference type                 | `null`        |

## 3. Array Index and Length

- Array indexing starts from `0`.
- Last valid index = `length - 1`.
- Use `.length` to get the number of elements.
- `length` is a field, not a method.

```java
int[] nums = {10, 20, 30};

System.out.println(nums.length);    // 3
System.out.println(nums[0]);        // 10
System.out.println(nums[nums.length - 1]); // 30
```

```text
Index:       0       1       2
          ┌──────┬──────┬──────┐
Value:    │  10  │  20  │  30  │
          └──────┴──────┴──────┘
             ↑               ↑
          First           Last
```

## 4. How Does Array Access Work?

**Interview definition:** Array access is index-based. Java calculates or locates the requested element using the array reference and index, and checks that the index is within the valid range.

- Access by index: `nums[i]`.
- Array access is generally constant time: **O(1)**.
- Java checks array bounds at runtime.
- An invalid index throws `ArrayIndexOutOfBoundsException`.

```java
int[] nums = {10, 20, 30};

System.out.println(nums[1]); // 20
System.out.println(nums[3]); // Exception
```

### Why Does This Exception Exist?

- Prevents access outside the array's valid element range.
- Helps preserve memory safety.
- Catches programming mistakes such as incorrect loop conditions or indexes.
- It is a runtime exception.

### How to Avoid It

**Strategy 1: Correct loop condition**

```java
for (int i = 0; i < nums.length; i++) {
    System.out.println(nums[i]);
}
```

Do not use `i <= nums.length`.

**Strategy 2: Validate the index**

```java
int index = 2;

if (index >= 0 && index < nums.length) {
    System.out.println(nums[index]);
}
```

**Strategy 3: Use a safe API where appropriate**

```java
import java.util.Objects;

int index = 2;
Objects.checkIndex(index, nums.length);
System.out.println(nums[index]);
```

**Interview trap:** Catching `ArrayIndexOutOfBoundsException` is not the normal way to handle array access. Validate indexes and correct the logic instead.

## 5. How Are Arrays Stored in Memory?

An array is an object in Java. It is created on the heap, while a local variable that refers to it is typically stored in the current method's stack frame.

```java
public class ArrayMemory {
    public static void main(String[] args) {
        int[] nums = {10, 20, 30};
    }
}
```

Conceptual view:

```text
STACK                         HEAP
┌──────────────────┐          ┌────────────────────────┐
│ main() frame     │          │ Array object           │
│                  │          │                        │
│ nums ────────────┼─────────>│ [10] [20] [30]         │
│                  │          │                        │
└──────────────────┘          └────────────────────────┘
```

- `nums` is a reference variable.
- The array object is on the heap.
- The array has a fixed length after creation.
- See [Stack and Heap Memory](../02-Memory-Management/stack-and-heap-memory.md) for the complete memory model.

## 6. Are Array Elements Stored Contiguously?

**Interview answer:** Java arrays provide indexed, sequential element storage. In common JVM implementations, an array's element slots are laid out consecutively within the array object. However, the Java language specification does not promise a particular physical heap layout or address arrangement.

- Primitive array: stores primitive values in its element slots.
- Reference array: stores references in its element slots.
- Objects referenced by a reference array may be located elsewhere in the heap.
- Do not assume all referenced objects are physically adjacent.

```java
int[] values = {10, 20, 30};

String[] names = {"A", "B", "C"};
```

Conceptually:

```text
int[] values
┌────┬────┬────┐
│ 10 │ 20 │ 30 │
└────┴────┴────┘

String[] names
┌────────┬────────┬────────┐
│ ref A  │ ref B  │ ref C  │
└───┬────┴───┬────┴───┬────┘
    |        |        |
    v        v        v
  String   String   String
  object   object   object
```

## 7. Two-Dimensional Arrays

**Interview definition:** A two-dimensional array in Java is an array whose elements are references to other arrays. It is commonly used to represent rows and columns.

```java
int[][] matrix = new int[2][3];
```

- 2 rows.
- 3 columns per row.
- Total elements = 2 × 3 = 6.

```text
             Columns
           0    1    2
         ┌────┬────┬────┐
Row 0    │  1 │  2 │  3 │
         ├────┼────┼────┤
Row 1    │  4 │  5 │  6 │
         └────┴────┴────┘
```

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

System.out.println(matrix[0][1]); // 2
System.out.println(matrix[1][2]); // 6
```

### Traversing a 2D Array

```java
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

- `matrix.length` → number of rows.
- `matrix[i].length` → number of columns in row `i`.

## 8. Java 2D Arrays Are Jagged Arrays

Java does not require every row to have the same length.

```java
int[][] jagged = new int[3][];

jagged[0] = new int[2];
jagged[1] = new int[4];
jagged[2] = new int[1];
```

```text
Row 0:  [  ][  ]
Row 1:  [  ][  ][  ][  ]
Row 2:  [  ]
```

**Remember:** A Java 2D array is an array of arrays, not necessarily a rectangular grid.

## 9. Three-Dimensional and Multidimensional Arrays

```java
int[][][] data = new int[2][3][4];
```

- 2 groups.
- 3 rows per group.
- 4 columns per row.
- Total elements = 2 × 3 × 4 = 24.

```text
3D Array
   Group 0                 Group 1
  ┌───────────┐           ┌───────────┐
  │ 3 rows    │           │ 3 rows    │
  │ 4 columns │           │ 4 columns │
  └───────────┘           └───────────┘
```

Access an element:

```java
data[1][2][3] = 100;
```

Read it:

```java
System.out.println(data[1][2][3]);
```

## 10. Arrays vs ArrayList

| Array                         | ArrayList                                    |
| ----------------------------- | -------------------------------------------- |
| Fixed size                    | Dynamically resizable                        |
| Can store primitives directly | Stores objects; primitives use wrapper types |
| Uses `.length`                | Uses `.size()`                               |
| Can be multidimensional       | Can contain nested lists                     |
| Built-in language construct   | Part of Collections Framework                |

```java
int[] nums = new int[5];

java.util.ArrayList<Integer> list = new java.util.ArrayList<>();
list.add(10);
list.add(20);
```

## 11. Complexity Quick Revision

| Operation                     | Typical complexity |
| ----------------------------- | -----------------: |
| Access by index               |               O(1) |
| Update by index               |               O(1) |
| Linear search                 |               O(n) |
| Binary search on sorted array |           O(log n) |
| Traverse entire array         |               O(n) |

## 12. Interview Questions — Click to Reveal

<details> <summary><strong>1. What is an array in Java?</strong></summary>

An array is a fixed-size object that stores multiple values of the same declared element type and provides index-based access.

</details>

<details> <summary><strong>2. Why does array indexing start at zero?</strong></summary>

Zero-based indexing makes the first element's offset zero. Conceptually, an element's position can be calculated as the starting position plus the index multiplied by the element-slot size. This is a common implementation model, not a guarantee of Java's physical memory layout.

</details>

<details> <summary><strong>3. Is array length fixed in Java?</strong></summary>

Yes. Once an array is created, its length cannot change. To grow it, create another array or use a resizable collection such as `ArrayList`.

</details>

<details>
<summary><strong>4. Where are arrays stored in Java?</strong></summary>

Arrays are objects and are allocated on the heap. A local variable referring to an array is typically stored in the method's stack frame.

</details>

<details>
<summary><strong>5. What is ArrayIndexOutOfBoundsException?</strong></summary>

It is a runtime exception thrown when code accesses an array index below zero or greater than or equal to the array's length.

</details>

<details>
<summary><strong>6. Are Java 2D arrays rectangular?</strong></summary>

Not necessarily. A 2D array is an array of arrays, so individual rows can have different lengths. Such an arrangement is called a jagged array.

</details>

<details> <summary><strong>7. What is the difference between `length` and `length()`?</strong></summary>

Arrays use the `length` field. Strings use the `length()` method. Collections such as `ArrayList` use `size()`.

</details>

<details>
<summary><strong>8. Are array elements stored contiguously?</strong></summary>

Common JVM implementations lay out an array's element slots consecutively within its array object. However, Java does not specify the physical heap layout. In a reference array, the referenced objects can be elsewhere.

</details>

<details>
<summary><strong>9. Can an array store different data types?</strong></summary>

A primitive array stores one primitive type. A reference array stores references compatible with its declared component type, including instances of permitted subtypes.

</details>

<details> <summary><strong>10. Why is array access O(1)?</strong></summary>

The index identifies the requested element directly, without searching through preceding elements. This is the usual random-access model.

</details>

## 13. Quick Revision Checklist

- [ ] Array declaration and creation
- [ ] Default values
- [ ] `.length` and valid indexes
- [ ] ArrayIndexOutOfBoundsException
- [ ] Stack reference and heap array object
- [ ] Primitive arrays vs reference arrays
- [ ] 2D, jagged and 3D arrays
- [ ] Array vs ArrayList
- [ ] Time complexity

## Related Files

- [Stack and Heap Memory](../02-Memory-Management/stack-and-heap-memory.md)
- [Methods and Recursion](../03-Methods-and-Recursion/methods-recursion-and-overloading.md)
