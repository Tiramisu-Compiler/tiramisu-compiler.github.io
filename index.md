{% include includes.html %}

## Overview
Tiramisu is a compiler for expressing fast, portable and composable data parallel computations.  Tiramisu is the first compiler to generate code that matches Intel MKL gemm, one of the most highly optimized kernels for X86 CPUs.  The user can express his code in the Tiramisu intermediate representation (Tiramisu IR), he can use the Tiramisu API to perform different optimizations and finaly he can generate the IR of his compiler of generate directly highly optimized code (LLVM, Vivado HLS, ...) targeting multicore, GPUs or FPGAs.

Current optimizations include:
- Loop nest transformations: loop tiling, loop fusion/distribution, loop spliting, loop interchange, loop shifting, loop unrolling, ...
- Affine data mappings: storage reordering, modulo storage (storage folding), ...
- For shared memory systems: loop parallelization, loop vectorization, ...

Current code generators include (1) multicore X86 CPUs, (2) ARM CPUs, (3) Nvidia GPUs, (4) Xilinx FPGAs (Vivado HLS) and (5) distributed machines (using MPI).


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

### Paper

https://arxiv.org/abs/1804.10694

### How Does Tiramisu Work ?

The library calls create an object (Abstract Syntax Tree) representing the program and that is passed to the compiler which will lower it, generate code and call the generated code.  A CoLib can be used for just-in-time compilation or for ahead-of-time compilation.
