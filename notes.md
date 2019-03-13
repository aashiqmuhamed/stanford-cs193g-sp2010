# Some quick notes

## Lecture 1
Function qualifiers, variable qualifiers, execution configuration, built in variables

## Lecture 5 - Performance optimization
Parallelism, ordering and then low level issues

Think about bandwidth vs compute first, then bank access in shared memory and finally register access per thread for SM occupancy
Kernel launch overhead
Divergent iteration
Memory Coalescing - Use SoA as compared to AoS

clock() to find time for each thread

## Lecture 6 - Parallel patterns
High level parallelism

Reduce, split, expand
Primordial pattern - locking in CUDA processors

# Reduce
Contigious vs interleaved access patterns and their dependence on performance
Complexity : Step O(logN), Operation O(N), Time O(N/P + logN)
