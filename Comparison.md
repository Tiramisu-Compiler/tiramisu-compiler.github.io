# Difference between Tiramisu, TVM and Halide

Tiramisu has fundamental differences compared to TVM and Halide. This post shows some of them.

## Tiramisu is a Polyhedral Compiler

The first difference is that Tiramisu uses the extended polyhedral model internaly while TVM and Halide use intervals. This allows Tiramisu to naturally express many code patterns and transformations that are difficult to express in TVM and Halide. This has many implications:

#### Tiramisu can Express and Optimize RNNs
Tiramisu can optimize RNNs (recurrent neural networks) and generate code that is 2x faster than TVM and Halide.
This is mainly because Tiramisu can apply a transformation known as iteration space skewing while TVM does not. This transformation allows Tiramisu to exploit wave-front parallelism and parallelize multi-layer LSTMs to increase GPU occupancy. This optimization is used in cuDNN and is described [here](https://devblogs.nvidia.com/optimizing-recurrent-neural-networks-cudnn-5/) (check step 3).

The following figure shows a comparison between Tiramisu, cuDNN, TVM, Halide and Tensor Comprehensions on LSTM-GPU. In the next section we will explain why Halide does not support dynamic RNNs (and programs with cyclic-dataflow graph in general).

<p align="center">
    <th>
        <div style="width:image width px; font-size:80%; text-align:center;">
        <img src="https://user-images.githubusercontent.com/9944372/66329232-1614c600-e926-11e9-8434-fdb6601caa4b.jpeg" alt="LSTM GPU" width="300"/>
        LSTM GPU</div>
    </th>
</p>
</center>

#### Tiramisu can Express Programs with Cyclic DataFlow Graphs

While frameworks Halide can implement a simple version of LSTMs (Long Short Term Memory), it is limited to LSTMs where the number of recurrence steps (i.e., cells) is known at compile time. Supporting a dynamic number of recurrence steps is important in case the numbr of steps is unkonw at compile time (which is usually the case if the size of the input is unknown at compile time such as in graph and tree LSTMs).
The inability to express RNNs (and programs that have cyclic data flow graphs in general) is mainly because Halide has a restricted language that prevents the user from writing a program that has a cycle in its data flow graph since it cannot guarantee the correctness of loop optimizations in that case.
Unlike Halide, Tiramisu performs polyhedral dependence analysis and uses transformation legality checks to guarantee the correctness of code transformations. This gives the compiler more flexibility and removes an important restriction on the Tiramisu language.

#### Tiramisu Represents Non-rectangular Loop Naturally

Tiramisu, TVM and Halide all need to represent code (iteration space of loops, and data accesses) before being able to apply any transformation.
Compilers that use intervals (such as TVM and Halide) can only represent loops that are rectangular (i.e., do not have any condition that depends on the loop iterator). They do support certain non-rectangular cases, but not in a natural way. Therefore if the user starts doing complicated transformations, that support is usually not sufficient.
Tiramisu can represent loop that are non-rectangular naturally.

#### Tiramisu Can Apply Any Affine Transformation

Tiramisu, being polyhedral, can support any affine transformation on your loop nest or data layout. No matter how the affine transformation is complicated, Tiramisu can do it. TVM does not.

## Tiramisu Supports Sparse DNNs

, while TVM does not.

## Tiramisu is More General

The third difference is that Tiramisu is more general that just DNNs, while TVM is designed mainly for DNNs. Tiramisu can generate efficient code for linear algebra, tensor algebra, driverless car applications (SLAM, ...),  image processing, ... so it is not tight to DNNs only.
