---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Structs", "Structures", "Struct Declaration", "User-Defined Types", "Composite Data Types"]
---

## Overview

A struct (structure) is a user-defined data type in C that allows you to group variables of different types under a single name. Each variable within a struct is called a member or field, and they are stored in contiguous memory locations. Structs enable you to create custom data types that represent real-world entities by bundling related data together.

## Why Use It

Structs are fundamental for organizing related data logically, making code more readable and maintainable. They allow you to model complex entities (like a Student with name, age, and grades) as a single unit rather than managing separate variables. This is crucial in larger programs for passing multiple related values to functions or creating arrays of complex data.

Without structs, you would need dozens of separate variables to represent complex entities, making it nearly impossible to pass related data around or maintain consistency between related fields.

## Code Example

```c
#include <stdio.h>
#include <string.h>

/* Define a struct to represent a student */
struct Student {
    char name[50];
    int age;
    float gpa;
    int studentID;
};

int main(void) {
    /* Method 1: Declare and initialize member by member */
    struct Student student1;
    strcpy(student1.name, "Alice Johnson");
    student1.age = 20;
    student1.gpa = 3.75;
    student1.studentID = 12345;

    /* Method 2: Initialize using designated initializers (C99+) */
    struct Student student2 = {
        .name = "Bob Smith",
        .age = 22,
        .gpa = 3.50,
        .studentID = 12346
    };

    /* Method 3: Initialize in order (older C style) */
    struct Student student3 = {"Charlie Brown", 19, 3.90, 12347};

    /* Access and print struct members using dot operator */
    printf("Student 1: %s, Age: %d, GPA: %.2f, ID: %d\n",
           student1.name, student1.age, student1.gpa, student1.studentID);

    /* Modify a member */
    student2.gpa = 3.65;
    printf("Student 2 updated GPA: %.2f\n", student2.gpa);

    return 0;
}
```

### Explanation

The program demonstrates three ways to declare and initialize structs. It defines a struct Student type with four members, then creates three instances using manual assignment, designated initializers (C99), and ordered initialization. It accesses struct members using the dot (.) operator to print values and modify the GPA field.

## Important Considerations

### Member Access Operators

Use the correct operator based on whether you have a struct variable or pointer:
```c
struct Student student;
struct Student *pStudent = &student;

student.age = 20;       // Dot operator for struct variables
pStudent->age = 20;     // Arrow operator for struct pointers
(*pStudent).age = 20;   // Equivalent to arrow operator
```

### String Assignment Limitation

Cannot use assignment operator for char arrays within structs:
```c
struct Student student;

// This does NOT work:
student.name = "Alice";  // Error: arrays not assignable

// Must use strcpy:
strcpy(student.name, "Alice");  // Correct

// Or initialize during declaration:
struct Student student = {.name = "Alice"};  // OK
```

### Uninitialized Members Contain Garbage

Declaring structs without initialization leaves members with unpredictable values:
```c
struct Student student1;  // Uninitialized - contains garbage

printf("%d\n", student1.age);  // Unpredictable value!

// Initialize to zero all members:
struct Student student2 = {0};  // All members set to 0

// Partial initialization (rest become 0):
struct Student student3 = {"Alice", 20};  // gpa=0.0, studentID=0
```

### Struct Comparison Not Supported

Cannot use == to compare structs directly:
```c
struct Student s1 = {"Alice", 20, 3.5, 1001};
struct Student s2 = {"Alice", 20, 3.5, 1001};

// This does NOT work:
if (s1 == s2) { ... }  // Error: invalid operands

// Must compare member by member:
if (strcmp(s1.name, s2.name) == 0 &&
    s1.age == s2.age &&
    s1.gpa == s2.gpa &&
    s1.studentID == s2.studentID) {
    printf("Students are identical\n");
}

// Or use memcmp (not always safe with padding):
if (memcmp(&s1, &s2, sizeof(struct Student)) == 0) {
    printf("Identical\n");
}
```

### Using typedef to Simplify Declarations

typedef creates an alias to avoid writing "struct" keyword:
```c
// Without typedef - must use "struct Student"
struct Student {
    char name[50];
    int age;
};

struct Student s1;  // Must include "struct"

// With typedef
typedef struct {
    char name[50];
    int age;
} Student;

Student s2;  // No "struct" keyword needed

// Or combine definition and typedef:
typedef struct Student {
    char name[50];
    int age;
} Student;
```

### Padding and Alignment

Struct size may be larger than the sum of member sizes due to padding:
```c
struct Example {
    char a;     // 1 byte
    int b;      // 4 bytes
    char c;     // 1 byte
};

// Expected: 1 + 4 + 1 = 6 bytes
// Actual: likely 12 bytes due to alignment padding

printf("Size: %zu\n", sizeof(struct Example));  // Prints 12 on most systems

// Memory layout (typical):
// [a][pad][pad][pad][b][b][b][b][c][pad][pad][pad]

// Reordering members can reduce padding:
struct Optimized {
    int b;      // 4 bytes
    char a;     // 1 byte
    char c;     // 1 byte
};
// Size: 8 bytes instead of 12
```

### Struct Scope and Declaration Placement

Struct definitions are typically placed before main() or in header files:
```c
// Global scope - accessible throughout file
struct Student {
    char name[50];
    int age;
};

int main(void) {
    struct Student s1;  // Can use struct here
    return 0;
}

// Local scope (less common)
int main(void) {
    struct LocalStruct {
        int x;
    };

    struct LocalStruct ls;  // Only accessible in main()
    return 0;
}
```

### Nested Structs

Structs can contain other structs as members:
```c
struct Date {
    int day;
    int month;
    int year;
};

struct Student {
    char name[50];
    struct Date birthdate;
    float gpa;
};

int main(void) {
    struct Student student = {
        .name = "Alice",
        .birthdate = {15, 3, 2005},
        .gpa = 3.75
    };

    printf("Born: %d/%d/%d\n",
           student.birthdate.day,
           student.birthdate.month,
           student.birthdate.year);

    return 0;
}
```

### Passing Structs to Functions

Structs are passed by value (copied) by default:
```c
void print_student(struct Student s) {
    printf("%s: %.2f\n", s.name, s.gpa);
    // s is a COPY - changes don't affect original
}

void modify_gpa(struct Student *s) {
    s->gpa = 4.0;  // Modifies original via pointer
}

int main(void) {
    struct Student student = {"Alice", 20, 3.5, 1001};

    print_student(student);  // Pass by value (copy)
    modify_gpa(&student);    // Pass by reference (pointer)

    printf("GPA: %.2f\n", student.gpa);  // Prints 4.00

    return 0;
}
```

For large structs, pass by pointer to avoid expensive copying.

---

## Related Topics

- [[Typedef]] - Creating struct aliases
- [[Arrays]] - Arrays of struct members
- [[C Pointers]] - Pointers to structs and arrow operator
- [[C Functions]] - Passing structs to functions
- [[Variables]] - Understanding member variables
- [[Arrays of Structs]] - Managing collections of structs
