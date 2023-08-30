### General Computer Architecture Principles  

Stages of execution: 
- Fetch (IF) 
- Decode (ID) 
- Execute (EX) 
- Memory (MEM): Load/Store 
- Writeback (WB): Update register 

### GPU Computation Architecture 

SIMT vs SIMD: 
- SIMD: 
    - Single instruction Multiple Data - Applies one instruction to multiple data lanes 
- SIMT: 
    - Single instruction Multiple threads
    - Applies one instruction to multiple independent threads (like a single thread of SIMD instructions)
    - enables thread level parallel code, as well as data parallel code for coordinated threads (shared memory?)

A "warp": 
- Smallest set of threads that are coordinated i.e. they are running the same instruction at the same time (CTA - cooperative thread array)
- comprised of 32 threads (architecture based)

Parts of Streaming Multiprocessor (SM):
- GPU is an array of SMs
- Warp Scheduler: Thread within SM scheduling 
- Scalar Processors: integer and float processing 
- Registers can be partitioned among Scalar processors (SP) within a SM
- Caches: Shared Memory, Constant Cache, Texture Cache, L1 Cache 
- Special Function Units (SFUs): for single precision floating point transcendental functions (sin, cos, exp etc..)

Memory Hierarchy:
- Local Memory: per-thread, private, temporary (present in external DRAM, for spills from register files)
- Shared Memory: memory shared between all threads of a single SM (Streaming Multiprocessor)
- Global Memory: memory shared between all threads of the GPU, implemented in external DRAM 

Mapping Blocks to Hardware: 
- Blocks don't map one to one with SMs
- A single SM can contain run multiple blocks depending on resource (ex: SP) availability 
- Each thread uses a single SP unit to work on it's data
- SM hardware scheduler decides which threads map to a collection of SPs in SIMD fashion (this is SIMT)
- They are guaranteed to execute in parallel, The unit of such a collection is a "warp", They have consecutive thread IDs
- In case of muliple blocks in a single SM, and if SPs are the bottle neck (i.e. SPs/SM <= Blocks/SM) the thread block scheduler shares time on the SPs
- Thread block scheduler takes care of this is believed to use a round robin policy.  

Warp Scheduling:
- Warp Scheduler checks which warp is ready with which PTX instruction and tags it as "READY" 
- tagging involves hardware implemented scoring sytems to check if everything necessary for the execution is ready (registers, FUs, etc..)
- So, depending on the SPs available in a SM, multiple warps can run in parallel or they needs to serialized! 
- i.e. warps might have to wait if there are not enough SPs

Possible Reasons for warp stalling: 
- Pipeline busy: Warp is waiting for some resources to be freed 
- Constant: Constant Cache miss 
- Memory Throttling: Large number of memory ops are pending (maybe application is memory transfer heavy)
- Synchronization: waiting for other threads due to a user defined sync call 
- Instruction Stalls: Due to instruction fetching/executing/loading delays
- Example: Row based naive transpose vs Column based naive transpose:
    - Row based have less memory throttling because loads are fast and warps can make some progress with the loaded data
    - In Column based, loading is itself slow, so all the other resources need to wait and cannot make any progress 
    - Overall throughput is going to be reduced due to this  
- Shared Memory Bank Conflicts: 
    - Modern shared memory stores data in "banks", places of storage which can be accessed in parallel
    - when number of elements stored is more than the number of banks, warps have to wait for other warps pulling data from the same bank 
- Global memory is also partitioned:
    - At once, GPU is able to read memoryBusWidth*numberOfPartitions amont of data from DRAM 

Ways to increase speed: 
- Launching thread blocks with lesser number of threads than elements to be processed. This will amortize the cost of index calculation 
- Using shared memory. Shared memory access is orders of magnitude faster than global memory, store things in shared memory process and store back to DRAM 
- Avoiding bank conflicts: set shared memory data size according to the number of banks in shared memory to reduce conflicts. 

Control Flow Divergence: 
- for branches, PTX assembler sets a "branch synchronization marker" on a stack containing a "mask" with bit values for each thread (in a warp?)
- all threads are run once with this marker, and then again with the marker flipped to run the remaning part of the threads (else condition) 
- why do you need a stack? -- for nested if else statements 
- Warp Hardware scheduler checks this stack to define the control flow 

Memory Access from Global Memory:
- Memory Transactions with Global Memory are "coalesced" 
- A warp typically requests 32 aligned 4 byte words in one global memory transaction 
- This is easy to implement for consecutive memory locations that is 4 byte aligned 
- but for strided access, larger the stride, more the memory transactions 

Shared Memory - A way to avoid multiple calls to Global Memory:
- "Tiling" approach: Data small enough to be put in shared memory and which is accessed by multiple threads is put inside the shared memory 


### CUDA Programming 

CUDA Launch Parameters and Indexing 
```C
gridDim, blockIdx, blockDim, threadIdx 

// most "oustide" dimension is z 
blockNum = blockIdx.z*(gridDim.x*gridDim.y) + blockIdx.y*(gridDim.x) + blockIdx.x; 
threadNum = threadIdx.z*(blockDim.x*blockDim.y) + threadIdx.y*(blockDim.x) + threadIdx.x; 
globalThreadIdx = blockNum*(blockDim.x*blockDim.y*blockDim.z) + threadNum; 

// max dimensions for grid and blocks [2]: 
// number of blocks in a grid:   x.max=2^31-1, y.max=2^16-1, z.max=2^16-1 
// number of threads in a block: x.max=1024, y.max=1024, z.max=64, x*y*z <= 1024 
// amount of threads in a block is bounded by the Device having to "remember" the context for all (1024) threads, also SPs available inside the SM 
``` 

Memory Types and Scope: 
```C
automatic variable other than arrays  // Memory: register, Scope: thread, Lifetime: Kernel
array variables                       // Memory: Local, Scope: Thread, Lifetime: Kernel [NOTE: Local here is far away in DRAM!]
__device__ __shared__ int sharedVar;  // Memory: Shared, Scope: Block, Lifetime: Kernel 
__device__ int globalVar;             // Memory: Global, Scope: Grid, Lifetime: Application 
__device__ __constant__ int constVar; // Memory: Constant, Scope: Grid, Lifetime: Application 
```

Function Types and Scope:
```C
__device__ float // executed on device, only callable from device   
__global__ void // executed on device, only callable from host 
__host__   float // executed on device, only callable from host  [default for all CUDA functions]

``` 

Synchronization:
```C
// within a block, threads can be synchronized. 
// Outside of a block, block execution order is not promised by CUDA, as well as synchronization 

// synchronization within a block: 
// this statement makes sure that all the threads in a block wait until everyone has reached this stage. 
__syncthreads(); 

// On the host, all CUDA function calls are asynchronous, if one has a need to synchronize this from the host (CPU): 
cudaDeviceSynchronize();  
// one usecase if one has to track performance of a bunch of kernels separately. 
```

Using Shared Memory: 
```C
cudaDeviceSetCacheConfig(); // Device level 
cudaFuncSetSetCacheConfig(); // Kernel level 
```

CUDA Device Properties API: 
```C
cudaGetDeviceCount(); 
cudaGetDeviceProperties(); 
// sample output 
/* 
    deviceCount:1
    name:NVIDIA GeForce RTX 3060
    totalGlobalMem:12884246528
    sharedMemPerBlock:49152
    regsPerBlock:65536
    maxThreadsPerBlock:1024
    clockRate:1807000
    maxThreadsDim:(1024,1024,64)
    maxGridSize:(2147483647,65535,65535)
    memoryBusWidth:192
    l2CacheSize:2359296
    persistingL2CacheMaxSize:1769472
    memoryClockRate:7501000
    globalL1CacheSupported:1
    localL1CacheSupported:1
    sharedMemPerMultiprocessor:102400
    major:8
    minor:6
*/ 
```

### Sources
1. [GPU Programming Course](https://www.youtube.com/playlist?list=PLbRMhDVUMngfj_NXI7jqMYLnhcRhRKAGq)
2. [Technical Specifications](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#features-and-technical-specifications)
