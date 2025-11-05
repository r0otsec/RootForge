---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C 2D Arrays", "Two-Dimensional Arrays", "Matrices", "Array of Arrays", "Multi-Dimensional Arrays"]
---

## Overview

A 2D array in C is an array of arrays, conceptually organized as a table with rows and columns. Memory-wise, C stores 2D arrays in row-major order, meaning all elements of the first row are stored contiguously, followed by all elements of the second row. The declaration `int matrix[3][4]` creates an array of 3 elements, where each element is itself an array of 4 integers.

## Why Use It

2D arrays are essential for representing tabular data, mathematical matrices, game boards, image pixel data, and other grid-based structures. They provide a natural and efficient way to access elements using two indices, making algorithms for matrix operations, image processing, and dynamic programming more intuitive.

## Code Example

```c
#include <stdio.h>

int main(void) {
    /* Declare and initialize a 3x4 matrix (3 rows, 4 columns) */
    int matrix[3][4] = {
        {1, 2, 3, 4},      /* Row 0 */
        {5, 6, 7, 8},      /* Row 1 */
        {9, 10, 11, 12}    /* Row 2 */
    };

    /* Access individual elements using two indices: [row][column] */
    printf("Element at [1][2]: %d\n", matrix[1][2]);  /* Prints 7 */

    /* Iterate through all elements using nested loops */
    printf("\nMatrix contents:\n");
    for (int i = 0; i < 3; i++) {           /* Loop through rows */
        for (int j = 0; j < 4; j++) {       /* Loop through columns */
            printf("%3d ", matrix[i][j]);    /* %3d for aligned output */
        }
        printf("\n");  /* Newline after each row */
    }

    /* Modify an element */
    matrix[2][3] = 99;
    printf("\nAfter modification, matrix[2][3] = %d\n", matrix[2][3]);

    /* Calculate sum of all elements */
    int sum = 0;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            sum += matrix[i][j];
        }
    }
    printf("Sum of all elements: %d\n", sum);

    return 0;
}
```

### Explanation

The program demonstrates fundamental 2D array operations. It declares a 3×4 integer matrix and initializes it using nested braces (each inner brace represents one row). It accesses a specific element using double-bracket notation [row][column], iterates through the entire matrix using nested for loops, modifies an element, and calculates the sum of all elements.

## Important Considerations

### Memory Layout

2D arrays are stored in row-major order - all elements are contiguous in memory:
```c
int arr[2][3] = {{1, 2, 3}, {4, 5, 6}};
// Memory layout: [1][2][3][4][5][6]
// arr[0][0], arr[0][1], arr[0][2], arr[1][0], arr[1][1], arr[1][2]
```

Understanding this layout is crucial for pointer arithmetic and cache efficiency.

### Function Parameters

When passing 2D arrays to functions, all dimensions except the first must be specified:
```c
// Column size MUST be specified
void printMatrix(int matrix[][4], int rows) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%3d ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main(void) {
    int data[3][4] = {{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}};
    printMatrix(data, 3);
    return 0;
}
```

The compiler needs the column size to calculate memory offsets correctly.

### Pointer Arithmetic

Understanding how 2D arrays relate to pointers:
```c
int matrix[3][4];

// These are equivalent:
matrix[i][j]
*(matrix[i] + j)
*(*(matrix + i) + j)
*((int*)matrix + i * 4 + j)  // 4 is column count
```

`matrix[i]` is a pointer to the i-th row (an array of 4 integers).

### Index Bounds

Valid indices for `matrix[3][4]` are:
- Rows: 0 to 2 (3 rows)
- Columns: 0 to 3 (4 columns)

Accessing `matrix[3][0]` or `matrix[0][4]` causes undefined behavior.

### Partial Initialization

Partially initialized 2D arrays fill remaining elements with 0:
```c
int arr[3][4] = {{1, 2}};
// Becomes:
// {{1, 2, 0, 0},
//  {0, 0, 0, 0},
//  {0, 0, 0, 0}}

int zeros[3][4] = {0};  // All elements initialized to 0
```

### Variable-Length Arrays (VLAs)

C99 allows runtime-determined dimensions, but with limitations:
```c
int rows, cols;
scanf("%d %d", &rows, &cols);
int matrix[rows][cols];  // VLA - C99+

// Not all compilers support VLAs
// Consider dynamic allocation for portability
```

For maximum compatibility, use dynamic allocation with `malloc()` or fixed-size arrays.

### Common Matrix Operations

```c
// Matrix transposition (swap rows and columns)
void transpose(int src[3][4], int dest[4][3]) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            dest[j][i] = src[i][j];
        }
    }
}

// Find maximum element
int findMax(int matrix[3][4]) {
    int max = matrix[0][0];
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            if (matrix[i][j] > max) {
                max = matrix[i][j];
            }
        }
    }
    return max;
}
```

### Jagged Arrays (Alternative Approach)

For variable-length rows, use array of pointers:
```c
int *jagged[3];
jagged[0] = (int[]){1, 2, 3};         // 3 elements
jagged[1] = (int[]){4, 5};            // 2 elements
jagged[2] = (int[]){6, 7, 8, 9};      // 4 elements
```

This provides flexibility but requires careful memory management.

---

## Related Topics

- [[Arrays]] - Understanding single-dimensional arrays
- [[C Nested Loops]] - Iterating through 2D arrays
- [[Arrays of Strings]] - Special case of 2D character arrays
- [[For Loops]] - Loop constructs for array traversal
- [[C Functions]] - Passing 2D arrays to functions
