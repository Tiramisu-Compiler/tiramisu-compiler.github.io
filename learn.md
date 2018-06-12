How To Use Tiramisu
----------------------
Tiramisu provides few classes to enable users to represent their program:
- The `tiramisu::computation` class: a computation is composed of an expression and an iteration space but is not associated with any memory location.
- The `tiramisu::function` class: a function is composed of multiple computations and a vector of arguments (functions arguments).
- The `tiramisu::buffer`: a class to represent memory buffers.

In general, in order to use Tiramisu to optimize, all what a user needs to do is the following:
- Represent the program that needs to be optimized
    - Instantiate a `tiramisu::function`,
    - Instantiate a set of `tiramisu::computation` objects for each function,
- Provide the list of optimizations (memory mapping and schedule)
    - Provide the mapping of each `tiramisu::computation` to memory (i.e. where each computation should be stored in memory),
    - Provide the schedule of each `tiramisu::computation` (a list of loop nest transformations and optimizations such as tiling, parallelization, vectorization, fusion, ...),
- Generate code
    - Generate an AST (Abstract Syntax Tree),
    - Generate target code (an object file),


