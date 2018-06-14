{% include includes.html %}

## Overview
Tiramisu is a compiler for expressing fast, portable and composable data parallel computations.  The user can express algorithms (`Tiramisu expressions`) using a simple C++ API and can automatically generate highly optimized code.  Tiramisu can be used in areas such as linear and tensor algebra, deep learning, image processing, stencil computations and machine learning.

The Tiramisu compiler is based on the polyhedral model thus it can express a large set of loop optimizations and data layout transformations.  It can also target (1) multicore X86 CPUs, (2) ARM CPUs, (3) Nvidia GPUs, (4) Xilinx FPGAs (Vivado HLS) and (5) distributed machines (using MPI) and is designed to enable easy integration of code generators for new architectures.

### Example

```cpp
// C++ code with a Tiramisu expression.
#include "tiramisu.h"

void foo(int N, int array_a[N], int array_b[N], int array_c[N])
{
    tiramisu::init();

    tiramisu::in A(int32_t, {N}, array_a), B(int32_t, {N}, array_b);
    tiramisu::out C(int32_t, {N}, array_c);
    
    tiramisu::var i;
    C(i) = A(i) + B(i);
    
    tiramisu::eval("CPU");
}
```

### Publications

[Tiramisu: A Code Optimization Framework for High Performance Systems](https://arxiv.org/abs/1804.10694).

Riyadh Baghdadi, Jessica Ray, Malek Ben Romdhane, Emanuele Del Sozzo, Patricia Suriana, Shoaib Kamil, Saman Amarasinghe.

ArXiv e-prints. February, 2018.


### How Does Tiramisu Work ?

The library calls create an object (Abstract Syntax Tree) representing the program and that is passed to the compiler which will lower it, generate code and call the generated code.  A CoLib can be used for just-in-time compilation or for ahead-of-time compilation.
