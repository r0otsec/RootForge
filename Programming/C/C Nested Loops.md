---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Nested Loops", "Nested Loop", "Loop Nesting", "Multi-Dimensional Iteration", "2D Arrays"]
---

## Overview

A nested loop is a loop placed inside the body of another loop. The inner loop completes all its iterations for each single iteration of the outer loop. Nested loops can be of any combination (for inside for, while inside for, etc.) and can be nested to any depth, though deep nesting becomes difficult to maintain.

## Why Use It

Nested loops are fundamental for working with multi-dimensional data structures (like 2D arrays representing matrices or grids), generating combinations, and implementing algorithms that require examining every pair of elements. They're essential for pattern printing, image processing, game board traversal, and mathematical computations.

Any time you need to process data with two or more dimensions, or need to examine all combinations of elements, nested loops are the appropriate tool.

## Code Example

```c
#include <stdio.h>

int main(void) {
    int rows = 5;
    int cols = 4;

    printf("Multiplication table (5x4):\n\n");
    printf("     ");

    // Print column headers
    for (int col = 1; col <= cols; col++) {
        printf("%4d", col);
    }
    printf("\n     ");
    for (int col = 1; col <= cols; col++) {
        printf("----");
    }
    printf("\n");

    // Outer loop: iterates through rows
    for (int row = 1; row <= rows; row++) {
        printf("%2d | ", row);  // Print row label

        // Inner loop: iterates through columns
        // This loop completes ALL iterations for EACH row iteration
        for (int col = 1; col <= cols; col++) {
            // For each (row, col) position, print the product
            printf("%4d", row * col);
        }

        printf("\n");  // Move to next line after completing all columns
    }

    printf("\nTotal iterations: %d (rows) x %d (cols) = %d\n",
           rows, cols, rows * cols);

    return 0;
}
```

### Explanation

This program creates a multiplication table using nested loops. The outer loop controls rows (1-5), and for each row, the inner loop completes all column iterations (1-4). When row=1, the inner loop runs completely printing 1×1, 1×2, 1×3, 1×4, then row advances to 2.

The total number of times the innermost code executes is rows × cols = 20 iterations.

## Important Considerations

### Execution Order

The inner loop completes ALL its iterations before the outer loop advances once:
```c
for (int i = 0; i < 3; i++) {           // Outer loop
    printf("Outer: i = %d\n", i);

    for (int j = 0; j < 4; j++) {       // Inner loop
        printf("  Inner: i = %d, j = %d\n", i, j);
    }
}

/* Output:
Outer: i = 0
  Inner: i = 0, j = 0
  Inner: i = 0, j = 1
  Inner: i = 0, j = 2
  Inner: i = 0, j = 3
Outer: i = 1
  Inner: i = 1, j = 0
  Inner: i = 1, j = 1
  Inner: i = 1, j = 2
  Inner: i = 1, j = 3
Outer: i = 2
  Inner: i = 2, j = 0
  Inner: i = 2, j = 1
  Inner: i = 2, j = 2
  Inner: i = 2, j = 3
*/
```

### Time Complexity

Two nested loops result in O(n²) operations, which grows quickly:
```c
// O(n²) algorithm
int n = 1000;
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // This executes 1,000,000 times
    }
}

// Three nested loops: O(n³)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        for (int k = 0; k < n; k++) {
            // This executes 1,000,000,000 times!
        }
    }
}
```

Be mindful of performance with large datasets and nested loops.

### Variable Scope

Use different variable names for each loop counter to avoid confusion:
```c
// Good: distinct variable names
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        printf("(%d, %d) ", i, j);
    }
}

// Bad: reusing variable name (won't compile in C99+)
for (int i = 0; i < rows; i++) {
    for (int i = 0; i < cols; i++) {  // Error: i redeclared
        printf("%d ", i);
    }
}
```

### Break and Continue Only Affect Innermost Loop

Control flow statements only impact the loop they're directly in:
```c
for (int i = 0; i < 3; i++) {
    printf("i = %d: ", i);

    for (int j = 0; j < 5; j++) {
        if (j == 3) {
            break;  // Only breaks inner loop, outer continues
        }
        printf("%d ", j);
    }
    printf("\n");
}

/* Output:
i = 0: 0 1 2
i = 1: 0 1 2
i = 2: 0 1 2
*/
```

To break out of multiple loops, use a flag variable:
```c
int found = 0;
for (int i = 0; i < rows && !found; i++) {
    for (int j = 0; j < cols && !found; j++) {
        if (target_found) {
            found = 1;  // Will exit both loops
        }
    }
}
```

### Deep Nesting

Three or more levels of nesting often indicates need for refactoring:
```c
// Hard to maintain
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        for (int k = 0; k < p; k++) {
            for (int l = 0; l < q; l++) {  // Too deep!
                // Complex logic buried here
            }
        }
    }
}

// Better: extract inner loops to functions
void processLayer(int i, int j) {
    for (int k = 0; k < p; k++) {
        for (int l = 0; l < q; l++) {
            // Logic here
        }
    }
}

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        processLayer(i, j);
    }
}
```

### Cache Performance

Row-major access is cache-friendly in C (accessing columns in inner loop):
```c
int matrix[1000][1000];

// Good: cache-friendly (row-major order)
for (int row = 0; row < 1000; row++) {
    for (int col = 0; col < 1000; col++) {
        matrix[row][col] = row + col;
    }
}

// Bad: cache-unfriendly (column-major order)
for (int col = 0; col < 1000; col++) {
    for (int row = 0; row < 1000; row++) {
        matrix[row][col] = row + col;  // Jumps around in memory
    }
}
```

C stores arrays in row-major order, so accessing consecutive columns is faster.

### Common Patterns

**Pattern printing:**
```c
// Print a triangle
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        printf("* ");
    }
    printf("\n");
}
```

**2D array processing:**
```c
int grid[3][4] = {{1,2,3,4}, {5,6,7,8}, {9,10,11,12}};

for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 4; col++) {
        printf("%3d", grid[row][col]);
    }
    printf("\n");
}
```

**Combination generation:**
```c
// Find all pairs
int arr[] = {1, 2, 3, 4};
for (int i = 0; i < 4; i++) {
    for (int j = i + 1; j < 4; j++) {  // j starts at i+1 to avoid duplicates
        printf("(%d, %d) ", arr[i], arr[j]);
    }
}
```

---

## Related Topics

- [[For Loops]] - Commonly used for nested loops
- [[While Loops]] - Can also be nested
- [[Arrays]] - Often processed with nested loops
- [[Break and Continue]] - Only affect innermost loop
- [[Variable Scope]] - Loop counter visibility
