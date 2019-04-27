{% include includes.html %}

## Overview
Tiramisu is a polyhedral compiler for expressing fast and portable data parallel algorithms.  It provides a simple C++ API for expressing algorithms and how these algorithms should be optimized by the compiler.

The Tiramisu compiler is based on the polyhedral model thus it can express a large set of loop optimizations and data layout transformations.  Currently it targets (1) multicore X86 CPUs, (2) Nvidia GPUs, (3) Xilinx FPGAs (Vivado HLS) and (4) distributed machines (using MPI).  It is designed to enable easy integration of code generators for new architectures.

## Where to Use Tiramisu?

<p align="center">
<div align="center">
<table style="margin: 0px auto;">
  <tr>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img src="https://user-images.githubusercontent.com/9944372/56697114-dfbaf580-66bb-11e9-8655-b9af8127cb83.jpg" alt="drawing" width="200"/>
        Image Processing</div>
    </th>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img src="https://user-images.githubusercontent.com/9944372/56697898-109c2a00-66be-11e9-858e-7ef46271bc27.png" alt="drawing" width="315"/>
        Deep Learning</div>
    </th>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img src="https://user-images.githubusercontent.com/9944372/56697914-1c87ec00-66be-11e9-86fe-dde11657ea41.gif" alt="drawing" width="185"/>
        Scientific Computing</div>
    </th>
  </tr>
</table>
</div>
</p>

## Why Tiramisu?
* Tiramisu is designed to target different hardware architectures (thanks to its multi-layer IR).
* Tiramisu generates efficient code (thanks to its scheduling language).
* Tiramisu is a polyhedral compiler, therefore:
    * It can perform complex loop transformations.
    * It can express programs with cycles in their data-flow graph (e.g., RNNs).
    * It supports naturally non-rectangular iteration spaces.
    * It uses dependence analysis to guarantee the correctness of optimizations.
* Tiramisu provides a platform for research on using machine learning for automatic optimization (it has an automatic code generator, examples of performance models, ...).

## Performance in Deep Learning

<p align="center">
<div align="center">
<table style="margin: 0px auto;">
  <tr>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img width="650" alt="CPU" src="https://user-images.githubusercontent.com/9944372/56835085-36057100-6841-11e9-9535-4925e0d6e9cb.png">
        Performance of a convolution implemented in Tiramisu compared to Intel MKL (CPU) for different input sizes (*).</div>
    </th>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img width="750" alt="CPU" src="https://user-images.githubusercontent.com/9944372/56835478-53870a80-6842-11e9-916b-00a3a709369a.png">
        Performance of LSTM implemented in Tiramisu compared to cuDNN (GPU) (**).</div>
    </th>
  </tr>
</table>
</div>
</p>

(*) The different sizes are extracted from the ResNet paper. CXY is the size of the layer X in ResNet and Y indicates the batch size (Y=0 for a batch size of 32, Y=1 for 64 and Y=2 for 100).

(**) Tensor Comprehensions and Halide cannot express LSTM because LSTM is a recurrent algorithm that creates a cycle in the data-flow graph.

## Example


The following is an example of a Tiramisu program specified using the C++ API.

```cpp
// C++ code with a Tiramisu expression.
#include "tiramisu/tiramisu.h"
using namespace tiramisu;

void generate_code()
{
    // Specify the name of the function that you want to create.
    tiramisu::init("foo");

    // Declare two iterator variables (i and j) such that 0<=i<100 and 0<=j<100.
    var i("i", 0, 100), j("j", 0, 100);

    // Declare a Tiramisu expression (algorithm) that is equivalent to the following C code
    // for (i=0; i<100; i++)
    //   for (j=0; j<100; j++)
    //     C(i,j) = 0;
    computation C({i,j}, 0);
    
    // Specify optimizations
    C.parallelize(i);
    C.vectorize(j, 4);

    // Generate code
    C.codegen({&C.get_buffer()}, "generated_code.o");
}
```

## Getting Started
- Build [Tiramisu](https://github.com/Tiramisu-Compiler/tiramisu/).
- Read the [Tutorials](https://github.com/Tiramisu-Compiler/tiramisu/blob/master/tutorials/README.md).
- Read the [Tiramisu Paper](https://arxiv.org/abs/1804.10694).
- Subscribe to Tiramisu [mailing list](https://lists.csail.mit.edu/mailman/listinfo/tiramisu).
- Read the compiler [internal documentation](https://tiramisu-compiler.github.io/doc/) (if you want to contribute to the compiler).


## Selected Publications

- [Tiramisu: A Polyhedral Compiler for Expressing Fast and Portable Code](https://arxiv.org/abs/1804.10694).<br/>
Riyadh Baghdadi, Jessica Ray, Malek Ben Romdhane, Emanuele Del Sozzo,  Abdurrahman Akkas, Yunming Zhang, Patricia Suriana, Shoaib Kamil, Saman Amarasinghe.
International Symposium on Code Generation and Optimization (CGO'19). Washington DC, USA. Feb., 2019.

- [A Unified Backend for Targeting FPGAs from DSLs](https://ieeexplore.ieee.org/document/8445108).<br/>
Emanuele Del Sozzo, Riyadh Baghdadi, Saman Amarasinghe, Marco D. Santambrogio.
2018 IEEE 29th International Conference on Application-specific Systems, Architectures and Processors (ASAP).
Milan, Italy. July, 2018.

- [Extending the Capabilities of Tiramisu](http://groups.csail.mit.edu/commit/papers/18/thesis_malek.pdf).<br/>
Malek Ben Romdhane. MEng Thesis, Massachusetts Institute of Technology. Cambridge, MA. Jun, 2018.

- [A Unified Compiler Backend for Distributed, Cooperative Heterogeneous Execution](http://groups.csail.mit.edu/commit/papers/18/jessica_master.pdf).<br/>
Jessica Morgan Ray. MEng Thesis, Massachusetts Institute of Technology. Cambridge, MA. Feb, 2018.


## Comparison with Polyhedral Compilers

<p align="center">
<div align="center">
<table style="margin: 0px auto;">
  <tr>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img width="600" alt="CPU" src="https://user-images.githubusercontent.com/9944372/56835016-06566900-6841-11e9-9e51-7fb25502ad9b.png">
        GEMM - Comparison with Polyhedral Compilers (CPU)</div>
    </th>
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img width="600" alt="GPU" src="https://user-images.githubusercontent.com/9944372/56835033-153d1b80-6841-11e9-90a6-4d42b34bc24d.png">
        GEMM - Comparison with Polyhedral Compilers (GPU)</div>
    </th>
  </tr>
</table>
</div>
</p>

Matrix dimension sizes for CPU: 1060x1060x1060. GPU: 3072x3072x3072.
