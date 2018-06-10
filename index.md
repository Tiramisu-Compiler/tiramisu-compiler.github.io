

### What is a CoLib ?
A CoLib is a compiler where the input is not expressed as a textual source code, but rather using library calls.

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

{% include includes.html %}

### How Does Tiramisu Work ?

The library calls create an object (Abstract Syntax Tree) representing the program and that is passed to the compiler which will lower it, generate code and call the generated code.  A CoLib can be used for just-in-time compilation or for ahead-of-time compilation.
