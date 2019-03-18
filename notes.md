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

### Reduce
Contigious vs interleaved access patterns and their dependence on performance
Complexity : Step O(logN), Operation O(N), Time O(N/P + logN)

### Split, compact, expand
Separate key value pairs by key (Sorting, building trees)
Removing null elements

Reserve variable storage per thread (Binning = store in compact fashion)
Scan - inclusive vs exclusive (shifted inclusive, reduce+scan, last element)
2-Phase scan

## Lecture 7
Segmented scan/ Segmented reduce 
Scan - Hilles Steele (step efficient)  vs Belloch (work efficient)
Sorting - data access pattern and control flow (why rearranging helps make it more efficient)
 mapreduce
### Kernel fusion S
Saves on I/O, output[i] = g(f(input[i]))
Reduce and pass on values in each domain
Functor when using stencils

## Lecture 8 - Thrust
Generic programming, high performance

### Some containers
thrust::host_vector<T>
thrust::device_vector<T>

### Some algorithms
thrust::sort()
thrust::reduce()
thrust::inclusive_scan()

### Compatibility with STL
```
std::list<int> h_list;
h_list.push_pack(13);
h_list.push_back(27);

thrust::device_vector<int> d_vec(h_list.size());
thrust::copy(h_list.begin(),h_list.end(),d_vec.begin());

thrust::device_vector<int> d_vec(h_list.begin(),h_list.enf());
```
### Use iterators like pointer
```
thrust::device_vector <int> d_vec(4);
thrust::device_vector<int>::iterator <int> = d_vec.begin();

*begin = 13;
int temp = *begin;
begin++;

```
### Same algos host and device
```
thrust::generate(h_vec.begin(),h_vec.end(),rand);
int sum  = thrust::reduce(h_vec.begin(),h_vec.end());
```

### Convertible to raw pointers
```
int *ptr = thrust::raw_pointer_cast(&d_vec[0]);
```

### Wrap raw pointers with device_ptr
```
int N = 10;

int *raw_ptr;
cudaMalloc((void **)&raw_ptr,N*sizeof(int));

thrust::device_ptr<int> dev_ptr(raw_ptr);

thrust::fill(dev_ptr,dev_ptr+N,(int)0); //or thrust assumes host raw_ptr while filling
dev_ptr[0]=1; //etc.

```
### Functors (function objects)
```
template <typename T>
class add
{
    public:
        T operator() (T a, T b) 
        {
            return a+b;
        }
};

float x = 10; float y = 20; float z;
add<float> func;
z = func(x,y); 
```

### Standard algorithms
Transformations, reductions, prefix sums, sorting
Generic definitions

### Other algos
Take a look at the slides to implement reduce, sort, count_if

### Fancy iterators
constant_iterator - use to add constant value
counting_iterator - use to increment
transform_iterator - transform a sequence
zip_iterator
