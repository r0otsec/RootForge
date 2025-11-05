---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Arrays", "Array", "Fixed-Size Array", "Array Declaration", "Array Indexing"]
---

## Overview

An array is a contiguous block of memory that stores a fixed number of elements of the same type, accessed via a zero-based index. Arrays are declared with a compile-time constant size: type name[size]. In C, arrays do not carry size information at runtime, and array names decay to pointers in most contexts.

## Why Use It

Arrays are fundamental for managing collections of related data. They provide O(1) random access to elements and cache-friendly memory layout. Arrays are the foundation for more complex data structures (strings, matrices, dynamic arrays) and are essential for any program dealing with multiple values.

Without arrays, you would need separate variables for each value, making code unmaintainable and preventing iteration over data.

## Code Example

```c
#include <stdio.h>

int main(void) {
    // Declaration and initialization
    int scores[5] = {85, 92, 78, 90, 88};  // Array of 5 integers

    // Partial initialization (remaining elements set to 0)
    int temperatures[7] = {72, 68, 70};  // temperatures[3-6] are 0

    // Size calculation: total bytes / bytes per element
    int num_scores = sizeof(scores) / sizeof(scores[0]);
    printf("Number of scores: %d\n\n", num_scores);

    // Accessing elements (zero-based indexing: 0 to size-1)
    printf("First score (index 0): %d\n", scores[0]);
    printf("Last score (index 4): %d\n", scores[4]);

    // Modifying elements
    scores[2] = 82;  // Change third element from 78 to 82
    printf("Modified score[2]: %d\n\n", scores[2]);

    // Iterating through array with a loop
    int sum = 0;
    printf("All scores:\n");
    for (int i = 0; i < num_scores; i++) {
        printf("  scores[%d] = %d\n", i, scores[i]);
        sum += scores[i];
    }

    // Calculate and display average
    double average = (double)sum / num_scores;
    printf("\nSum: %d\n", sum);
    printf("Average: %.2f\n", average);

    return 0;
}
```

### Explanation

This program demonstrates array fundamentals: declaration with initialization, calculating array size using sizeof, zero-based indexing (first element is [0], last is [size-1]), modifying elements, and iteration. The temperatures array shows partial initialization where remaining elements are automatically set to 0.

The sizeof technique `sizeof(scores) / sizeof(scores[0])` calculates the number of elements by dividing total array bytes by the size of one element.

## Important Considerations

### Fixed Size

Array sizes must be compile-time constants in standard C (before C99):
```c
// Valid: constant size
int arr1[10];           // OK
int arr2[5 + 3];        // OK (constant expression)

// C99+: Variable-length arrays (VLAs)
int n;
scanf("%d", &n);
int arr3[n];  // OK in C99+, but not all compilers support it

// Pre-C99: Not allowed
int size = 10;
int arr4[size];  // Error in C89/C90
```

For maximum compatibility, use constant sizes or dynamic allocation with malloc().

### No Bounds Checking

C doesn't check if array indices are valid - accessing out of bounds causes undefined behavior:
```c
int numbers[5] = {10, 20, 30, 40, 50};

printf("%d\n", numbers[0]);   // OK: first element
printf("%d\n", numbers[4]);   // OK: last element
printf("%d\n", numbers[5]);   // UNDEFINED BEHAVIOR!
printf("%d\n", numbers[-1]);  // UNDEFINED BEHAVIOR!

// Common bug: off-by-one error
for (int i = 0; i <= 5; i++) {  // Wrong: <= instead of <
    printf("%d ", numbers[i]);  // Undefined when i=5
}
```

Always ensure indices are in range [0, size-1].

### Arrays Not Assignable

Cannot use assignment operator to copy arrays:
```c
int arr1[5] = {1, 2, 3, 4, 5};
int arr2[5];

// This does NOT work:
arr2 = arr1;  // Error: arrays not assignable

// Must copy element by element:
for (int i = 0; i < 5; i++) {
    arr2[i] = arr1[i];
}

// Or use memcpy:
#include <string.h>
memcpy(arr2, arr1, sizeof(arr1));
```

### Size Information Lost

When passed to functions, arrays decay to pointers and lose size information:
```c
void printArray(int arr[]) {  // Same as: int *arr
    // sizeof(arr) returns size of POINTER, not array!
    int wrong_size = sizeof(arr) / sizeof(arr[0]);  // Wrong!

    printf("Inside function, sizeof(arr) = %zu\n", sizeof(arr));
    // Prints 8 on 64-bit systems (pointer size), not array size
}

int main(void) {
    int numbers[10] = {0};

    printf("In main, sizeof(numbers) = %zu\n", sizeof(numbers));
    // Prints 40 (10 elements × 4 bytes each)

    printArray(numbers);

    return 0;
}
```

Always pass array size as a separate parameter:
```c
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
}

int main(void) {
    int numbers[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    printArray(numbers, 10);
    return 0;
}
```

### Uninitialized Contains Garbage

Arrays not initialized contain unpredictable values:
```c
// Uninitialized: contains garbage
int arr1[5];
for (int i = 0; i < 5; i++) {
    printf("%d ", arr1[i]);  // Unpredictable values!
}

// Initialized to zero (all elements)
int arr2[5] = {0};  // {0, 0, 0, 0, 0}

// Partial initialization (rest become 0)
int arr3[5] = {1, 2};  // {1, 2, 0, 0, 0}
```

Always initialize arrays explicitly to avoid undefined behavior.

### Off-by-One Errors

The most common array bug - forgetting arrays use 0-based indexing:
```c
int scores[5] = {85, 92, 78, 90, 88};

// Valid indices: 0, 1, 2, 3, 4
// Array of size N has indices 0 to N-1

printf("%d\n", scores[0]);   // First element: 85
printf("%d\n", scores[4]);   // Last element: 88
printf("%d\n", scores[5]);   // ERROR: out of bounds!

// Correct loop
for (int i = 0; i < 5; i++) {  // i < 5, not i <= 5
    printf("%d ", scores[i]);
}
```

### Multi-Dimensional Arrays

Arrays can have multiple dimensions:
```c
// 2D array (3 rows, 4 columns)
int matrix[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// Access elements with [row][col]
printf("%d\n", matrix[0][0]);  // 1
printf("%d\n", matrix[2][3]);  // 12

// Iterate with nested loops
for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 4; col++) {
        printf("%3d", matrix[row][col]);
    }
    printf("\n");
}
```

### Array Initialization Shortcuts

```c
// All elements to 0
int zeros[100] = {0};

// All elements to same value (C99+)
int fives[5] = {5, 5, 5, 5, 5};  // Must list all

// Designated initializers (C99+)
int arr[10] = {[0] = 1, [4] = 5, [9] = 10};
// arr[0]=1, arr[4]=5, arr[9]=10, rest are 0

// Let compiler calculate size
int auto_size[] = {1, 2, 3, 4, 5};  // Size is 5
```

### Strings are Arrays

Strings in C are arrays of characters terminated with '\0':
```c
char name[10] = "Alice";
// Actually: {'A', 'l', 'i', 'c', 'e', '\0', garbage...}

printf("%s\n", name);  // Prints: Alice
printf("%c\n", name[0]);  // Prints: A
```

---

## Related Topics

- [[For Loops]] - Iterating through arrays
- [[C Nested Loops]] - Processing multi-dimensional arrays
- [[Variables]] - Array element types
- [[User Input]] - Reading data into arrays
- [[Arrays and User Input]] - Interactive array population
- [[C Functions]] - Passing arrays to functions
