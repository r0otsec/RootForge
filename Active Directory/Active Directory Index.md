---
tags:
  - Index
date: 2025-27-08
---

![[ADIndex.png]]

This is some python code inline - `print("Hello World")`

```python
print ("Hello!")
```

`{python icon title:"Print Statement"} print("This is inline code")`

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/mman.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<unistd.h>
#include<fcntl.h>


struct cipher {
  int v0;
  int v1;
  char buf[0x100];
};

typedef void (*GEN)(struct cipher*, char*);
typedef void (*ENC)(struct cipher*, char*, int);

int main(void)
{
  int fd = open("incident", O_RDONLY);
  char *ptr = mmap(0, 0x1000, PROT_EXEC | PROT_READ, MAP_SHARED, fd, 0);
  struct cipher *c = malloc(sizeof(struct cipher));

  GEN gen_cipher = (GEN)(ptr + 0x633);
  ENC encrypt = (ENC)(ptr + 0x6a3);

  //int urandom = open("/dev/urandom", O_RDONLY);
  int file = open("malcious", O_RDONLY);
  char buf[0x1000];
  read(file, buf, 0x20);
  gen_cipher(c, buf);

  int n = read(file, buf, 0x1000);
  encrypt(c, buf, n);
  fwrite(buf, n, 1, stdout);
  return 0;
}
```