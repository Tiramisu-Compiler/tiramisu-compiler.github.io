{% include includes.html %}

## Overview
Tiramisu is a compiler for expressing fast, portable and composable data parallel computations.  Tiramisu provides a simple C++ API for the user to express algorithms (`Tiramisu expressions`) and how to optimize these algorithms using a set of simple commands.  Tiramisu can be used in areas such as linear and tensor algebra, deep learning, image processing, stencil computations and machine learning.

The Tiramisu compiler is based on the polyhedral model thus it can express a large set of loop optimizations and data layout transformations.  It can also target (1) multicore X86 CPUs, (2) Nvidia GPUs, (3) Xilinx FPGAs (Vivado HLS) and (4) distributed machines (using MPI) and is designed to enable easy integration of code generators for new architectures.

### Example

```cpp
// C++ code with a Tiramisu expression.
#include "tiramisu.h"

void foo(int N, int array_a[N], int array_b[N], int array_c[N])
{
    tiramisu::init();

    // Declare an iterator and inputs
    tiramisu::iter i;
    tiramisu::in A(i), B(i);

    // Declare the Tiramisu expression (algorithm)
    tiramisu::comp C(i) = A(i) + B(i);

    // Realize, compile and run the expression
    C.realize(int, {N});
    C.compile({(A, array_a), (B, array_b), (C, array_c)});
    C.run();
}
```

### Getting Started
- Build [Tiramisu](https://github.com/Tiramisu-Compiler/tiramisu/).
- Read the [Tutorials](https://github.com/Tiramisu-Compiler/tiramisu/blob/master/tutorials/README.md).
- Read the [Tiramisu Paper](https://arxiv.org/abs/1804.10694).
- Subscribe to Tiramisu [mailing list](https://lists.csail.mit.edu/mailman/listinfo/tiramisu).
- Read the compiler [internal documentation](https://tiramisu-compiler.github.io/doc/) (if you want to contribute to the compiler).


### Publications

[Tiramisu: A Code Optimization Framework for High Performance Systems](https://arxiv.org/abs/1804.10694).<br/>
Riyadh Baghdadi, Jessica Ray, Malek Ben Romdhane, Emanuele Del Sozzo, Patricia Suriana, Shoaib Kamil, Saman Amarasinghe.
ArXiv e-prints. February, 2018.
