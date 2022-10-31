## Lesson 25: Work Allocation, Hardware, and GPUs

---
**Date:** 10/19/2022

**Relevant Resources:** Dive into Systems, section 
[11.4](https://diveintosystems.org/book/C11-MemHierarchy/caching.html)
and [11.6](https://diveintosystems.org/book/C11-MemHierarchy/coherency.html)

## Overview:

---
- Get cache information on Linux
- Layout of a cache
- Matrix-matrix multiplication algorithm


### Get cache information on Linux

---
To get the size of CPU caches on a Linux machine, we can type the 
following command in terminal: <br> 

`lscpu | grep cache` <br> 

Then, We may get result looks like the following: <br>

```commandline
L1d cache:                      384 KiB
L1i cache:                      256 KiB
L2 cache:                       4 MiB
L3 cache:                       16 MiB
```

To get more information about CPU, we can also type the following
command in terminal: <br>

`lscpu | less`

<br>

Moreover, the following command outputs information about caches
of a specific processor core. And we can type `type`, `level`,
and `shared_cpu_list` after `index*` directory to see the details of 
each cache.

Suppose we want information about `cpu0`:<br>

`cat /sys/devices/system/cpu/cpu0/cache/index*/ <type, level, or 
shared_cpu_list here>`

### Layout of a cache

---
Below is an example of the layout of a 4 sets 6 ways cache:<br>

![fig 1](Images/L25_cacheLayout.jpeg )
<div style="text-align: center;">A 4 sets 6 ways cache</div>

###**Assume <font color=orange>*w*</font> stand for amount of ways, <font color=orange>*s*</font> stand for amount of sets**

**The number of cache lines of a cache:**<br>
> number of cache lines = *w* • *s*

**The size of a cache:**
> size of cache = *w* • *s* • (size of a cache line)


###Ex.
**Number of cache lines of the cache above:**
> *w* • *s* = 4 • 6 = 24

**Suppose the size of a cache line is 50B, the size of the cache above is:**
> *w* • *s* • (size of cache line) =  4 • 6 • 50B = 1200B


### Matrix-matrix multiplication algorithm

---
This section is about an algorithm calculating multiplication of two *n* x *n* matrices
and the time complexity of the algorithm.

![fig 2](Images/L25_matrix.jpeg)
<div style="text-align: center;">Picture above is an example of a matrix multiplication where 
i = 1, j = 2, k = 2</div>

Suppose there are two *n* x *n* matrices *A* and *B*. Index *i*, *j*, and *k* stand for
row index, column index, and current index. Function *A(i, k)* and *B(k, j)* returns 
element at row *i* column *k* in matrix A and element at row *k* column *j* in matrix B
, and so on for *C(i, j)*.<br>

Below is the algorithm for updating multiplication results in matrix *C*:<br>
```
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            C(i, j) += A(i, k) * B(k, j)
```
The runtime for the algorithm is $O(n^3)$ because all three loops iterate $n$ times.
The number of cycle is $time / n^3$.

