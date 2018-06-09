## Tiramisu CoLib
Tiramisu is a CoLib designed for expressing fast, portable and composable data parallel computations.

### What is a CoLib ?
A CoLib is a compiler where the input is not a textual source code, but rather a set of library calls that create an object (Abstract Syntax Tree) representing the program and that is passed to the compiler which will lower it, generate code and call the generated code.  A CoLib can be used for just-in-time compilation or for ahead-of-time compilation.

### Example

```markdown
// C++ code with a Tiramisu expression.
#include "tiramisu.h"

void foo(int N, int array_a[N], int array_b[N], int array_c[N])
{
    tiramisu::init();

    tiramisu::in   A(int32_t, {N}, array_a), B(int32_t, {N}, array_b);
    tiramisu::out  C(int32_t, {N}, array_c);
    
    tiramisu::var i;
    C(i) = A(i) + B(i);
    
    tiramisu::eval("CPU");
}
```


### Paper

https://arxiv.org/abs/1804.10694



{% <a href="https://github.com/Tiramisu-Colib/tiramisu"><img style="position: absolute; top: 0; left: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_left_red_aa0000.png" alt="Fork me on GitHub"></a> %}
