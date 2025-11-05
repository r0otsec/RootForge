---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Pointers", "Pointers", "Memory Addresses", "Pointer Arithmetic", "Indirection"]
---

## Overview

A pointer is a variable that stores the memory address of another variable rather than a direct value. Pointers are declared using the asterisk (*) symbol and are typed to point to specific data types. They enable indirect access to data by "pointing to" the location in memory where the data resides.

## Why Use It

Pointers are arguably the most powerful and essential feature of C, enabling dynamic memory allocation, efficient array manipulation, function parameter passing by reference, and implementation of complex data structures. They allow functions to modify variables in the caller's scope, enable working with large data without copying, and provide direct memory access for systems programming.

Without pointers, C would lose much of its power—you couldn't allocate memory dynamically, modify function arguments, create linked lists, or implement many fundamental algorithms efficiently.

## Code Example

```c
#include <stdio.h>

int main(void) {
    /* Regular variables */
    int age = 25;
    float salary = 50000.0;

    /* Pointer declarations - type* pointerName */
    int *pAge;
    float *pSalary;

    /* & (address-of operator) - gets the address of a variable */
    pAge = &age;
    pSalary = &salary;

    /* Display addresses and values */
    printf("age variable:\n");
    printf("  Value: %d\n", age);
    printf("  Address: %p\n", (void*)&age);
    printf("  Pointer pAge stores: %p\n\n", (void*)pAge);

    /* * (dereference operator) - accesses the value at the address */
    printf("Using pointer pAge:\n");
    printf("  *pAge (dereferenced): %d\n", *pAge);

    /* Modify variable through pointer (indirection) */
    *pAge = 30;
    printf("  After *pAge = 30:\n");
    printf("  age is now: %d\n", age);
    printf("  *pAge is now: %d\n\n", *pAge);

    /* Pointer arithmetic with arrays */
    int numbers[5] = {10, 20, 30, 40, 50};
    int *pNumbers = numbers;

    printf("\nArray access using pointers:\n");
    for (int i = 0; i < 5; i++) {
        printf("  numbers[%d] = %d, *(pNumbers + %d) = %d\n",
               i, numbers[i], i, *(pNumbers + i));
    }

    return 0;
}
```

### Explanation

The program demonstrates fundamental pointer concepts. It creates variables and pointers, uses the address-of operator (&) to make pointers store addresses, and shows how to dereference pointers using * to access and modify values indirectly. It demonstrates that changing a value through a pointer affects the original variable and introduces pointer arithmetic for array access.

## Important Considerations

### Declaration vs Dereferencing

The asterisk (*) has different meanings in different contexts:
```c
int *p;      // Declaration: p is a pointer to int
*p = 10;     // Dereferencing: access the value p points to

int x = 5;
int *ptr = &x;   // Declaration and initialization
printf("%d\n", *ptr);  // Dereferencing to read value

// Common confusion:
int* p1, p2;  // p1 is pointer, p2 is regular int (not pointer!)
int *p1, *p2;  // Both are pointers - better style
```

### Always Initialize Pointers

Uninitialized pointers contain garbage addresses (wild pointers):
```c
int *p;  // Uninitialized - points to random memory
*p = 10;  // UNDEFINED BEHAVIOR - writing to random address!

// Safe initialization options:
int *p1 = NULL;  // Points to nothing (safe)
int x = 5;
int *p2 = &x;    // Points to valid variable
```

### NULL Pointer Checking

Always check pointers before dereferencing:
```c
int *p = NULL;

// Unsafe - dereferencing NULL crashes program
*p = 10;  // CRASH: segmentation fault

// Safe - check before use
if (p != NULL) {
    *p = 10;
} else {
    printf("Error: NULL pointer\n");
}

// Common with malloc
int *arr = malloc(10 * sizeof(int));
if (arr == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    return 1;
}
// Safe to use arr now
```

### Dangling Pointers

Pointers to destroyed or freed memory:
```c
int *p;
{
    int x = 5;
    p = &x;  // p points to x
}  // x is destroyed here - p is now dangling!

*p = 10;  // UNDEFINED BEHAVIOR - x no longer exists

// Also with malloc/free:
int *ptr = malloc(sizeof(int));
free(ptr);
*ptr = 10;  // UNDEFINED BEHAVIOR - memory was freed

// Solution: set to NULL after free
ptr = NULL;  // Can safely check if (ptr != NULL)
```

### Memory Leaks

Forgetting to free malloc'd memory:
```c
void leak_example(void) {
    int *p = malloc(100 * sizeof(int));
    // ... use p ...
    // Forgot to free(p) - MEMORY LEAK!
}  // p goes out of scope, memory address lost forever

// Correct:
void no_leak(void) {
    int *p = malloc(100 * sizeof(int));
    if (p == NULL) return;

    // ... use p ...

    free(p);  // Release memory
    p = NULL;  // Optional: prevent dangling pointer
}
```

### Type Safety

Don't assign addresses of one type to pointer of another:
```c
int x = 5;
float *p = &x;  // WARNING: incompatible pointer type

*p = 3.14;  // Undefined behavior - type mismatch

// Correct:
int *pInt = &x;
float y = 3.14;
float *pFloat = &y;

// Void pointers can hold any type (advanced):
void *generic = &x;
void *generic2 = &y;  // OK

// But must cast to use:
printf("%d\n", *(int*)generic);
```

### Never Return Address of Local Variables

Local variables are destroyed when function returns:
```c
int* bad_function(void) {
    int x = 5;
    return &x;  // DANGER: x destroyed after return
}

int main(void) {
    int *ptr = bad_function();
    printf("%d\n", *ptr);  // UNDEFINED BEHAVIOR
    return 0;
}

// Solutions:
// 1. Return allocated memory
int* good_function1(void) {
    int *p = malloc(sizeof(int));
    *p = 5;
    return p;  // OK - caller must free()
}

// 2. Modify caller's variable
void good_function2(int *result) {
    *result = 5;  // Modifies caller's variable
}

// 3. Return static variable (persists)
int* good_function3(void) {
    static int x = 5;  // Persists after return
    return &x;  // OK but shared across calls
}
```

### Pointer Arithmetic

Adding/subtracting moves by sizeof(type) bytes, not 1 byte:
```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;  // Points to arr[0]

printf("%d\n", *p);      // 10 (arr[0])
printf("%d\n", *(p+1));  // 20 (arr[1])
printf("%d\n", *(p+2));  // 30 (arr[2])

// p+1 doesn't add 1 byte - it adds sizeof(int) bytes (4)
// If p is at address 1000:
//   p+0 points to 1000
//   p+1 points to 1004 (1000 + 1*sizeof(int))
//   p+2 points to 1008 (1000 + 2*sizeof(int))

p++;  // Move to next element (same as p = p + 1)
printf("%d\n", *p);  // 20

// Pointer subtraction gives element count
int *end = &arr[4];
int *start = &arr[0];
int count = end - start;  // 4 (not bytes, but elements)
```

### Double Free Error

Never call free() twice on same pointer:
```c
int *p = malloc(sizeof(int));
free(p);
free(p);  // UNDEFINED BEHAVIOR - double free!

// Solution:
free(p);
p = NULL;  // Set to NULL after free
free(p);   // Now safe (freeing NULL is no-op)
```

### Buffer Overruns

Pointer arithmetic can go beyond array bounds:
```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;

printf("%d\n", *(p+0));  // OK: arr[0]
printf("%d\n", *(p+4));  // OK: arr[4]
printf("%d\n", *(p+5));  // UNDEFINED BEHAVIOR: out of bounds
printf("%d\n", *(p+100));  // UNDEFINED BEHAVIOR: way out of bounds

// C has NO bounds checking - programmer's responsibility
```

### Const Pointers

Different ways to use const with pointers:
```c
int x = 5, y = 10;

// Pointer to const int - can't modify value
const int *p1 = &x;
*p1 = 6;   // ERROR: can't modify value
p1 = &y;   // OK: can change what it points to

// Const pointer to int - can't change pointer
int *const p2 = &x;
*p2 = 6;   // OK: can modify value
p2 = &y;   // ERROR: can't change what it points to

// Const pointer to const int - can't modify either
const int *const p3 = &x;
*p3 = 6;   // ERROR: can't modify value
p3 = &y;   // ERROR: can't change what it points to
```

### Pointer to Pointer

Pointers can point to other pointers:
```c
int x = 5;
int *p = &x;      // p points to x
int **pp = &p;    // pp points to p (pointer to pointer)

printf("%d\n", x);     // 5
printf("%d\n", *p);    // 5 (dereference p)
printf("%d\n", **pp);  // 5 (dereference pp twice)

**pp = 10;  // Modify x through double indirection
printf("%d\n", x);  // 10
```

---

## Related Topics

- [[Variables]] - Understanding what pointers point to
- [[Arrays]] - Arrays and pointer relationship
- [[C Functions]] - Passing pointers to functions
- [[Structs]] - Pointers to structs (arrow operator)
- [[Arrays of Structs]] - Dynamic allocation of struct arrays
- [[Typedef]] - Creating pointer type aliases
