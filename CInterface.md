---
title: Interacting with C
parent: Impala.md
weight: 2
excerpt: "This document describes how to interface a C program to Impala and vice-versa."
---

## Calling C functions from Impala

Functions defined in C (or C++ functions marked with the **extern "C"** qualifier) can be made visible from Impala by using the **extern** keyword:

```rust
extern "C" {
    fn rand() -> int; // Make the C function rand visible from Impala
}
```

It is the programmer's responsibility to ensure that the C signature matches the Impala signature.

## Calling Impala functions from C

Impala functions can also be made available to C, using the **extern** keyword:

```rust
extern fn get_42() -> int {
    42
}
```

The C prototype corresponding to the function in the previous example would be:

```cpp
int get_42(void);
```

## Automatically generating a C interface

The Impala compiler contains an automatic C interface generator.
It works by inspecting all functions marked **extern** (visible from C), and creating C signatures for them.
If a structure type is used in the Impala function, the compiler will emit the equivalent structure in C.

As an example, if the file `blob.impala` contains the following lines:

```rust
struct Blob {
    i: int,
    f: float
}

extern fn sum_blob(blob: &Blob) -> float {
    blob.f + (blob.i as float)
}
```

The result of running the command:

```shell
$ impala blob.impala -emit-c-interface -o blob
```

Is the file `blob.h` containing:

```cpp
/* blob.h : Impala interface file generated by impala */
#ifndef BLOB_H
#define BLOB_H

#ifdef __cplusplus
extern "C" {
#endif

struct Blob {
    int i;
    float f;
};

float sum_blob(struct Blob const* blob);

#ifdef __cplusplus
}
#endif

#endif /* BLOB_H */
```

Note that not all Impala types are supported in the interface generation tool.

## Passing pointers to and from C

In general, it is safe to pass C pointers to Impala or the other way around.
However, please note that some functions (in particular the runtime functions interacting with device memory) may require that a pointer is aligned to page boundary.
In any case, the runtime functions can be called from both C or Impala to obtain aligned memory.
