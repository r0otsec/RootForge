---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Functions", "Functions", "Function Declaration", "Function Definition", "Function Prototypes"]
---

## Overview

A function is a self-contained block of code that performs a specific task, has a name, can accept input parameters, and can return a result value to the caller. Functions encapsulate logic into reusable units that can be called multiple times from different parts of a program without rewriting the code.

## Why Use It

Functions are fundamental to structured programming and essential for creating maintainable, organized code. They enable code reuse, eliminating duplication and reducing bugs. Functions break complex problems into smaller, manageable pieces, making programs easier to understand, test, debug, and modify.

Without functions, even moderately complex programs become unwieldy, with repeated code scattered throughout and logic that's difficult to follow or maintain.

## Function Anatomy

A function in C consists of four parts:

1. **Return type**: The data type of the value the function returns (or `void` if it returns nothing)
2. **Function name**: The identifier used to call the function
3. **Parameters**: Input values the function receives (enclosed in parentheses)
4. **Function body**: The code that executes when the function is called (enclosed in braces)

## Code Example

```c
#include <stdio.h>

/* Function prototype (declaration) */
int add(int a, int b);
void print_banner(void);
double calculate_area(double radius);

int main(void) {
    int sum;
    double area;

    print_banner();

    /* Calling function with arguments, storing return value */
    sum = add(10, 20);
    printf("Sum is: %d\n", sum);

    /* Function call with double parameter */
    area = calculate_area(5.0);
    printf("Area: %.2f\n", area);

    return 0;
}

/* Function definitions */
int add(int a, int b) {
    return a + b;
}

void print_banner(void) {
    printf("=== Function Demo ===\n");
}

double calculate_area(double radius) {
    const double PI = 3.14159265359;
    return PI * radius * radius;
}
```

### Explanation

The program demonstrates function prototypes, definitions, and calls. The `main()` function orchestrates calls to demonstration functions: `print_banner()` shows a void function with no parameters or return value, `add()` returns an integer sum of two integers, and `calculate_area()` works with double parameters and returns a double result.

Function prototypes at the top tell the compiler about functions before they're defined, allowing `main()` to call them even though their full definitions appear later.

## Important Considerations

### Pass-by-Value

C uses pass-by-value—functions receive copies of arguments, not the original variables:
```c
void try_to_modify(int x) {
    x = 100;  // Only modifies the local copy
}

int main(void) {
    int num = 5;
    try_to_modify(num);
    printf("%d\n", num);  // Still prints 5
}
```

To modify variables, you must use pointers (covered in advanced topics).

### Function Prototypes Are Essential

Always provide prototypes (declarations) before use, typically at the top of the file or in header files:
```c
// Prototype tells compiler the function signature
int multiply(int a, int b);

int main(void) {
    int result = multiply(5, 3);  // Can call before definition
    return 0;
}

// Definition provides the actual implementation
int multiply(int a, int b) {
    return a * b;
}
```

Without a prototype, the compiler may make incorrect assumptions about parameter types.

### Matching Declarations

Prototype and definition must match exactly in return type, name, and parameters:
```c
// Prototype
int calculate(int x, int y);

// Definition must match
int calculate(int x, int y) {  // Correct
    return x + y;
}

// This would be an error:
// double calculate(int x, int y) { ... }  // Wrong return type
```

### Return Statement Requirements

Non-void functions must return a value on all possible execution paths:
```c
int absolute_value(int x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
    // Good: all paths return a value
}

int bad_function(int x) {
    if (x > 0) {
        return x;
    }
    // Bad: no return if x <= 0 (undefined behavior)
}
```

### Local Variable Scope

Variables declared inside functions are local—they exist only during function execution and are destroyed when the function returns:
```c
void demo_function(void) {
    int local_var = 10;  // Created when function is called
    printf("%d\n", local_var);
}  // local_var is destroyed here

// local_var doesn't exist outside the function
```

### Never Return Address of Local Variables

Returning a pointer to a local variable causes undefined behavior because the variable is destroyed when the function returns:
```c
int* bad_function(void) {
    int x = 5;
    return &x;  // DANGER: x is destroyed after return
}

int main(void) {
    int* ptr = bad_function();
    printf("%d\n", *ptr);  // Undefined behavior - accessing freed memory
}
```

---

## Related Topics

- [[Variables]] - Understanding function parameters and local variables
- [[User Input]] - Functions often work with user input
- [[C If Statements]] - Control flow within functions
- [[Arithmetic Operators]] - Operations commonly performed in functions
- [[C Format Specifiers]] - Displaying function results
