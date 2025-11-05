---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Arrays of Structs", "Struct Arrays", "Structure Arrays", "Collections of Structs"]
---

## Overview

An array of structs is a collection of multiple struct variables stored in contiguous memory, where each element is a complete struct instance. This allows you to manage multiple records of the same type (like multiple students, employees, or products) using array indexing and iteration.

## Why Use It

Arrays of structs are essential for managing collections of related complex data in real-world applications like databases, student management systems, inventory tracking, and game development. Instead of creating separate variables for each record, arrays allow efficient handling of hundreds or thousands of records using loops.

Without arrays of structs, managing even a small dataset becomes unwieldy—you'd need separate struct variables for each record, making it impossible to process data systematically or scale to larger datasets.

## Code Example

```c
#include <stdio.h>
#include <string.h>

struct Book {
    char title[100];
    char author[50];
    int year;
    float price;
};

void displayBook(struct Book b) {
    printf("Title: %s\n", b.title);
    printf("Author: %s\n", b.author);
    printf("Year: %d\n", b.year);
    printf("Price: $%.2f\n\n", b.price);
}

int main(void) {
    /* Declare an array of 3 books */
    struct Book library[3];

    /* Initialize first book using member access */
    strcpy(library[0].title, "The C Programming Language");
    strcpy(library[0].author, "Kernighan & Ritchie");
    library[0].year = 1978;
    library[0].price = 45.99;

    /* Initialize remaining books inline */
    library[1] = (struct Book){
        "Expert C Programming",
        "Peter van der Linden",
        1994,
        39.99
    };

    library[2] = (struct Book){
        "C Programming: A Modern Approach",
        "K.N. King",
        2008,
        89.99
    };

    /* Iterate through array to display all books */
    printf("Library Catalog:\n");
    for (int i = 0; i < 3; i++) {
        printf("Book %d:\n", i + 1);
        displayBook(library[i]);
    }

    /* Find most expensive book */
    int maxIndex = 0;
    for (int i = 1; i < 3; i++) {
        if (library[i].price > library[maxIndex].price) {
            maxIndex = i;
        }
    }

    printf("Most expensive book:\n");
    displayBook(library[maxIndex]);

    return 0;
}
```

### Explanation

The program manages a collection of books using an array of structs. It declares library[3] that holds three struct Book instances, showing three initialization methods. It iterates through the array to display all books and searches to find the most expensive book by comparing the price member.

## Important Considerations

### Access Syntax

Use arrayName[index].memberName - array indexing before member access:
```c
struct Student students[5];

// Correct syntax: array index THEN member access
students[0].age = 20;
students[0].gpa = 3.75;

// Access within loops
for (int i = 0; i < 5; i++) {
    printf("Student %d: GPA %.2f\n", i, students[i].gpa);
}

// Common mistake: wrong order
students.age[0] = 20;  // ERROR: students is not a struct
```

### Array Bounds Checking

Accessing beyond array size causes undefined behavior:
```c
struct Book library[3];

library[0].price = 45.99;  // OK: valid index
library[2].price = 89.99;  // OK: last valid index
library[3].price = 29.99;  // UNDEFINED BEHAVIOR: out of bounds!

// Safe iteration - use < not <=
for (int i = 0; i < 3; i++) {  // Correct
    printf("%s\n", library[i].title);
}

for (int i = 0; i <= 3; i++) {  // BUG: accesses library[3]
    printf("%s\n", library[i].title);  // Undefined when i=3
}
```

### Initialization Rules

Different initialization behaviors based on storage class:
```c
// Automatic (local) - uninitialized contains garbage
void function(void) {
    struct Student students[3];  // Contains garbage values
    printf("%d\n", students[0].age);  // Unpredictable!
}

// Static/Global - uninitialized becomes zero-initialized
struct Student global_students[3];  // All members set to 0

// Explicit initialization to zero
struct Student students[3] = {0};  // All members of all elements = 0

// Partial initialization
struct Student students[3] = {
    {"Alice", 20, 3.5},
    {"Bob", 22, 3.8}
    // students[2] is zero-initialized
};

// Fewer initializers than size - rest become zero
struct Student students[5] = {
    {"Alice", 20, 3.5}  // students[1-4] are zero-initialized
};
```

### String Assignment After Declaration

Cannot use = operator for char array members after declaration:
```c
struct Student students[2];

// This does NOT work after declaration:
students[0].name = "Alice";  // ERROR: arrays not assignable

// Must use strcpy:
strcpy(students[0].name, "Alice");  // Correct

// Assignment works during initialization:
struct Student students[2] = {
    {.name = "Alice", .age = 20},  // OK during initialization
    {.name = "Bob", .age = 22}
};
```

### Function Passing

Arrays decay to pointers when passed to functions - must pass size separately:
```c
void print_students(struct Student arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%s: %.2f\n", arr[i].name, arr[i].gpa);
    }
}

int main(void) {
    struct Student students[5];

    // sizeof trick only works in same scope as declaration
    int count = sizeof(students) / sizeof(students[0]);

    print_students(students, count);  // Must pass size

    return 0;
}

// Inside function, sizeof doesn't give array size:
void print_students(struct Student arr[], int size) {
    // sizeof(arr) returns POINTER size (8 bytes on 64-bit)
    // NOT array size - this is why we pass size parameter
    int wrong = sizeof(arr) / sizeof(arr[0]);  // Wrong!
}
```

### Size Calculation

Calculate number of elements using sizeof:
```c
struct Student students[10];

// Calculate number of elements
int count = sizeof(students) / sizeof(students[0]);
printf("Array has %d elements\n", count);  // Prints 10

// Also works with:
int count2 = sizeof(students) / sizeof(struct Student);

// Define size as constant for maintainability
#define MAX_STUDENTS 10
struct Student students[MAX_STUDENTS];

for (int i = 0; i < MAX_STUDENTS; i++) {
    // Process students
}
```

### Memory Efficiency

Consider dynamic allocation for variable-sized collections:
```c
#include <stdlib.h>

int main(void) {
    int num_students;
    printf("How many students? ");
    scanf("%d", &num_students);

    // Dynamic allocation - size determined at runtime
    struct Student *students = malloc(num_students * sizeof(struct Student));

    if (students == NULL) {
        fprintf(stderr, "Memory allocation failed\n");
        return 1;
    }

    // Access same as array
    for (int i = 0; i < num_students; i++) {
        strcpy(students[i].name, "Student");
        students[i].age = 20;
    }

    // Don't forget to free
    free(students);

    return 0;
}
```

### Sorting and Searching

Common operations on struct arrays:
```c
// Bubble sort by GPA
void sort_by_gpa(struct Student arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (arr[j].gpa < arr[j + 1].gpa) {
                // Swap entire structs
                struct Student temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// Linear search by name
int find_student(struct Student arr[], int size, const char *name) {
    for (int i = 0; i < size; i++) {
        if (strcmp(arr[i].name, name) == 0) {
            return i;  // Found at index i
        }
    }
    return -1;  // Not found
}
```

### Compound Literals for Initialization

Use compound literals (C99+) for inline initialization:
```c
struct Book library[3];

// Compound literal - temporary struct
library[0] = (struct Book){
    "The C Programming Language",
    "K&R",
    1978,
    45.99
};

// Useful for function calls
displayBook((struct Book){"Title", "Author", 2024, 29.99});
```

---

## Related Topics

- [[Structs]] - Understanding struct fundamentals
- [[Arrays]] - Array basics and indexing
- [[For Loops]] - Iterating through struct arrays
- [[C Functions]] - Passing struct arrays to functions
- [[C Pointers]] - Dynamic allocation of struct arrays
- [[Typedef]] - Simplifying struct array declarations
