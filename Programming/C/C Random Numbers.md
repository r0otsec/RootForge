---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Random Numbers", "rand()", "srand()", "Random Number Generation", "Pseudo-Random Numbers"]
---

## Overview

C provides pseudo-random number generation through rand(), which returns a pseudo-random integer between 0 and RAND_MAX (at least 32767), and srand(), which seeds the random number generator. Using srand(time(NULL)) seeds it with the current time, producing different sequences on each program run.

## Why Use It

Random numbers are essential for simulations, games, testing algorithms with varied inputs, statistical sampling, and creating unpredictable behavior. Understanding how to properly seed and scale random numbers is crucial for avoiding predictable patterns and common range-mapping errors.

Without proper seeding, programs using rand() will generate the same "random" sequence every time they run, which is useful for debugging but undesirable for most applications.

## Code Example

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void) {
    // Seed the random number generator with current time
    // Without this, rand() produces the SAME sequence every run
    srand((unsigned int)time(NULL));

    printf("Five random numbers (full range 0 to RAND_MAX):\n");
    for (int i = 0; i < 5; i++) {
        printf("  %d\n", rand());
    }

    // Generate random numbers in a specific range [min, max]
    int min = 1, max = 6;  // Simulate dice rolls
    printf("\nFive dice rolls (1-6):\n");
    for (int i = 0; i < 5; i++) {
        // Formula: rand() % (max - min + 1) + min
        int dice = rand() % (max - min + 1) + min;
        printf("  Roll %d: %d\n", i + 1, dice);
    }

    // Generate random floating-point numbers between 0.0 and 1.0
    printf("\nFive random probabilities (0.0 to 1.0):\n");
    for (int i = 0; i < 5; i++) {
        // Cast to double before division to avoid integer division
        double probability = (double)rand() / RAND_MAX;
        printf("  %.4f\n", probability);
    }

    return 0;
}
```

### Explanation

The program seeds the generator with srand(time(NULL)) to ensure different results each run. It demonstrates generating raw random numbers, scaling them to a specific range (1-6 for dice), and creating floating-point values between 0.0 and 1.0. The modulo operator restricts the range, while division by RAND_MAX normalizes to [0.0, 1.0].

The formula `rand() % (max - min + 1) + min` maps the full range of rand() to any desired range [min, max] inclusive.

## Important Considerations

### Seeding is Crucial

Without srand(), rand() produces the same sequence every run:
```c
// Without seeding
int main(void) {
    printf("Run 1: %d %d %d\n", rand(), rand(), rand());
    // Every execution prints the SAME three numbers
    return 0;
}

// With proper seeding
int main(void) {
    srand((unsigned int)time(NULL));
    printf("Run 1: %d %d %d\n", rand(), rand(), rand());
    // Different numbers each time program runs
    return 0;
}
```

Always seed with srand(time(NULL)) unless you specifically want reproducible sequences.

### Seed Only Once

Call srand() once at program start, not in loops or before each rand() call:
```c
// Wrong: reseeding in loop
for (int i = 0; i < 5; i++) {
    srand((unsigned int)time(NULL));  // Don't do this!
    printf("%d ", rand());
    // May print same number multiple times if loop runs quickly
}

// Correct: seed once at start
srand((unsigned int)time(NULL));
for (int i = 0; i < 5; i++) {
    printf("%d ", rand());
}
```

Reseeding repeatedly, especially with the same seed value, defeats randomness.

### NOT Thread-Safe

rand() and srand() use global state and are not thread-safe:
```c
// In single-threaded programs: OK
srand((unsigned int)time(NULL));
int r = rand();

// In multi-threaded programs: use rand_r() instead
unsigned int seed = (unsigned int)time(NULL) + thread_id;
int r = rand_r(&seed);  // Thread-safe version
```

### NOT Cryptographically Secure

Never use rand() for security-sensitive applications:
```c
// NEVER use for passwords, encryption keys, tokens, etc.
int insecure_password = rand();  // Predictable!

// Use OS-specific secure random functions instead:
// - Windows: BCryptGenRandom()
// - Linux/Unix: /dev/urandom, getrandom()
// - C11: Use libraries like libsodium
```

rand() is designed for simulations and games, not security.

### Modulo Bias

Simple modulo can slightly favor some values when RAND_MAX+1 is not evenly divisible by the range:
```c
// Slight bias if RAND_MAX is not a multiple of 6
int dice = rand() % 6 + 1;  // Some values slightly more likely

// More uniform (but overkill for most purposes):
int uniform_dice() {
    int limit = RAND_MAX - (RAND_MAX % 6);
    int r;
    do {
        r = rand();
    } while (r >= limit);
    return r % 6 + 1;
}
```

For most applications (games, simulations), modulo bias is negligible.

### Integer Division

Cast to double before division for floating-point results:
```c
// Wrong: integer division
double bad = rand() / RAND_MAX;  // Always 0 (integer division!)

// Correct: cast to double first
double good = (double)rand() / RAND_MAX;  // 0.0 to 1.0
```

### Range Mapping Formulas

Common patterns for scaling random numbers:
```c
// Range [0, N-1]
int r = rand() % N;

// Range [min, max] inclusive
int r = rand() % (max - min + 1) + min;

// Range [0.0, 1.0]
double r = (double)rand() / RAND_MAX;

// Range [min, max] for doubles
double r = min + (double)rand() / RAND_MAX * (max - min);

// Random boolean (0 or 1)
int coin = rand() % 2;

// Random probability check (10% chance)
if ((double)rand() / RAND_MAX < 0.10) {
    printf("Rare event!\n");
}
```

### Reproducible Sequences

For debugging, use a fixed seed to get the same sequence:
```c
// For reproducible testing
srand(12345);  // Same seed = same sequence
printf("%d %d %d\n", rand(), rand(), rand());
// Always prints the same three numbers
```

---

## Related Topics

- [[C Math Functions]] - Mathematical operations on random numbers
- [[For Loops]] - Generating multiple random numbers
- [[C If Statements]] - Random probability checks
- [[C Functions]] - Creating random number utility functions
- [[Variables]] - Storing random values
