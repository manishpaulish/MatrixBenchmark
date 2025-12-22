High-Performance Matrix Multiplication Benchmark
This project demonstrates Low-Latency C++ optimization techniques applied to matrix multiplication. It implements and benchmarks four different approaches, evolving from a standard naive implementation to a highly optimized, parallelized engine that is 24x faster.

Performance Results
Running on Apple M-Series Silicon (8 Cores) with 1024×1024 matrices:

Method	Execution Time	Speedup	Key Technique
1. Naive	1.335 s	1.0x	Baseline (O(N 
3
 ))
2. Transpose	0.911 s	1.4x	Improved Memory Layout
3. Tiled (IKJ)	0.235 s	5.7x	L1 Cache Blocking & Vectorization
4. Parallel	0.056 s	23.8x	Multithreading (Data Parallelism)
Optimization Levels Explained
1. Naive Implementation

The standard three-loop approach (i,j,k).

Bottleneck: Severe cache misses due to accessing Matrix B column-wise (jumping huge steps in memory).

2. Transpose Optimization

Matrix B is transposed before multiplication.

Improvement: Allows reading Matrix B row-by-row (Sequential Access).

Drawback: The overhead of copying data prevents maximum speedup.

3. Tiled + Loop Reordering (The "Cache Aware" Approach)

Tiling (Blocking): breaks the matrix into small 64×64 blocks that fit perfectly inside the CPU's L1 Cache, minimizing slow RAM access.

IKJ Loop Order: Swapping loops allows the inner loop to access memory sequentially, enabling the compiler to use SIMD (Auto-Vectorization) instructions (processing 4 numbers per cycle).

4. Parallel (Multithreading)

Uses Data Parallelism to split the matrix horizontally across all available CPU cores.

Implemented using modern C++ std::thread and lambda functions.

Race-Condition Free: Each thread writes to a distinct memory region of the result matrix, requiring no mutex locks.

Build & Run Instructions
Prerequisites

C++ Compiler (Clang, GCC, or MSVC) supporting C++17.

CMake (Version 3.10 or higher).

Steps

Clone the repository:

Bash
git clone https://github.com/yourusername/MatrixBenchmark.git
cd MatrixBenchmark
Build the project:

Bash
mkdir build
cd build
cmake ..
make
Run the benchmark:

Bash
./benchmark
📂 Project Structure
Plaintext
MatrixBenchmark/
├── CMakeLists.txt       # Build configuration
├── README.md            # Documentation
├── include/
│   └── matrix.h         # Header declarations
└── src/
    ├── main.cpp         # Benchmark runner
    └── matrix.cpp       # Implementation of all 4 algorithms
🎓 Key Learnings
This project highlights the importance of Hardware-Aware Programming. By understanding cache hierarchy (L1/L2/L3) and CPU vectorization (SIMD), we can achieve massive performance gains without changing the underlying mathematical algorithm (O(N 
3
 )).