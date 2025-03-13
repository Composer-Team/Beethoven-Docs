# Accelerator Core Implementation

Let's look at what an accelerator core implementation might look like in Beethoven using [Chisel HDL](https://www.chisel-lang.org).
We'll implement a vector-addition accelerator for the kernel shown below.

```cpp
void vector_add(int *a, int *b, int *out, int len) {
    for (int i = 0; i < len; ++i)
        out[i] = a[i] + b[i];
}

int main() {
    int array_len = 1024;
    int *a   = (int*)malloc(sizeof(int) * array_len);
    int *b   = (int*)malloc(sizeof(int) * array_len);
    int *out = (int*)malloc(sizeof(int) * array_len);
    // initialize vectors
    vector_add(a, b, out, array_len);
    return 0;
}
```

First thing's first, let's get the boilerplate out of the way.
```java
class MyVectorAdder extends Module {

}
```
