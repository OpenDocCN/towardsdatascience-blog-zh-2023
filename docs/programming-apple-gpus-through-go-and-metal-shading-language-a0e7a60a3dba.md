# 通过 Go 和 Metal Shading Language 编程苹果 GPU

> 原文：[https://towardsdatascience.com/programming-apple-gpus-through-go-and-metal-shading-language-a0e7a60a3dba?source=collection_archive---------2-----------------------#2023-12-04](https://towardsdatascience.com/programming-apple-gpus-through-go-and-metal-shading-language-a0e7a60a3dba?source=collection_archive---------2-----------------------#2023-12-04)

## 研究 Go、Cgo、Metal Shading Language、Metal Performance Shaders，以及对矩阵乘法的不同方法进行基准测试

[](https://mikecvet.medium.com/?source=post_page-----a0e7a60a3dba--------------------------------)[![Mike Cvet](../Images/93545a0c873515a599ba094ad51ee915.png)](https://mikecvet.medium.com/?source=post_page-----a0e7a60a3dba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0e7a60a3dba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0e7a60a3dba--------------------------------) [Mike Cvet](https://mikecvet.medium.com/?source=post_page-----a0e7a60a3dba--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbc23035f3073&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogramming-apple-gpus-through-go-and-metal-shading-language-a0e7a60a3dba&user=Mike+Cvet&userId=bc23035f3073&source=post_page-bc23035f3073----a0e7a60a3dba---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0e7a60a3dba--------------------------------) ·12分钟阅读·2023年12月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0e7a60a3dba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogramming-apple-gpus-through-go-and-metal-shading-language-a0e7a60a3dba&user=Mike+Cvet&userId=bc23035f3073&source=-----a0e7a60a3dba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0e7a60a3dba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogramming-apple-gpus-through-go-and-metal-shading-language-a0e7a60a3dba&source=-----a0e7a60a3dba---------------------bookmark_footer-----------)![](../Images/859d70c23112109e0b37aa4562897206.png)

图片由 [Etienne Martin](https://unsplash.com/@etiennemartin?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 在 [Unsplash](https://unsplash.com/photos/a-close-up-of-a-metal-diamond-plate-v6uiP2MD6vs?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 上拍摄

下面我将描述如何使用 [cgo](https://pkg.go.dev/cmd/cgo) 在 Go 和原生 C 之间进行接口，如何将其用于与 Apple 的 [Metal Performance Shaders](https://developer.apple.com/documentation/metalperformanceshaders) 框架的 Objective-C 绑定进行接口，如何与用 [Metal 着色语言](https://developer.apple.com/documentation/metal) 编写的 *自定义* GPU 代码（着色器）进行接口，最后将所有这些与手写的和 [OpenBLAS](https://www.openblas.net) 的基于 Go 的矩阵乘法操作进行基准测试。这是为我的 M2 MacBook 编写的。

源代码的布局，[在 GitHub 上可用](https://github.com/mikecvet/go-mm)，如下所示：

![](../Images/fce8e5410b9c2d3d5dd57587c6a14875.png)

高级源代码、库和设备布局

这量很大，所以我将其分解为这些部分，或者可以直接跳到 [基准测试](#4aba)。

+   [GPU 和浮点并行性](#cad4)

+   [Metal GPU 基础](#e19f)

+   [Metal 着色语言](#1a7c)

+   [Objective-C 绑定](#eb32)

+   [Metal Performance Shaders 框架](#890a)

+   [Go 和 cgo](#7de2)

+   [Go 实现基线和 OpenBLAS](#8aa1)

+   [结果](#4aba)

# GPU 和浮点并行性

我假设大多数人此时直观上已经熟悉 GPU 在某些计算任务中的强大性能，尤其是一些支持机器学习的任务。直到我开始尝试 Metal，我才亲身理解它们比 CPU *强大* 多少。

GPU 设计上极其高效于大规模并行浮点运算，这要求高内存带宽。我的 MacBook M2 有 8 个 CPU 核心和 8 个 GPU 核心，但为了对比，[Nvidia RTX 4090](https://www.nvidia.com/en-us/geforce/graphics-cards/40-series/rtx-4090/) 拥有 *16384 核心*，而 [H100](https://www.forbes.com/sites/janakirammsv/2022/03/27/10-interesting-facts-about-nvidia-hopper-h100-gpu/) 拥有 *16896* CUDA 核心及数百个额外的专用张量核心。GPU 通常支持 [SIMD](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data) 处理，使其能够在多个数据点上同时执行相同的指令。

除了图形处理外，矩阵乘法和线性代数任务一般都受益于这种并发，得益于其高度可并行的算法。这反过来支持了核心机器学习工作负载，如训练和推断 [[1](https://betterprogramming.pub/word2vec-embeddings-from-the-ground-up-for-the-ml-adjacent-8d8c484e7cb5)] [[2](https://betterprogramming.pub/convolutional-neural-networks-from-the-ground-up-for-the-ml-adjacent-35d530ab26f3)]。

[CUDA](https://developer.nvidia.com/about-cuda#:~:text=Since%20its%20introduction%20in%202006,workstations%2C%20compute%20clusters%20and%20supercomputers) 可能是最著名的 GPU 编程平台，专门针对 Nvidia 硬件。也有数学框架可用于 [OpenGL](https://glm.g-truc.net/0.9.8/index.html)。像 TensorFlow 和 PyTorch 这样的框架可以与 GPU 硬件 [轻松集成](https://www.tensorflow.org/guide/gpu)，且透明度相当高。[这篇文章](https://explosion.ai/blog/metal-performance-shaders) 对将支持 Metal 的 GPU 框架集成到 [spaCy NLP 库](https://spacy.io) 中的性能提升做了有趣的分析。

# Metal GPU 基础知识

直接编程 GPU 计算并不像编写设备 CPU 代码那样简单。当使用 Apple 的 Metal 框架时，执行 GPU 上代码的大致操作步骤如下：

+   寻找适当的 GPU 设备

+   创建一个用于执行命令的队列（即 [MTLCommandQueue](https://developer.apple.com/documentation/metal/mtlcommandqueue)）

+   将数据数组的指针封装到结构化缓冲区中；如果数据是可执行代码，则使用 [管道状态](https://developer.apple.com/documentation/metal/mtlcomputepipelinestate)，否则使用 [常规缓冲区](https://developer.apple.com/documentation/metal/mtlbuffer)。Apple 的 GPU 使用 [统一内存空间](https://developer.apple.com/documentation/metal/resource_fundamentals/choosing_a_resource_storage_mode_for_apple_gpus#)，这意味着我们不需要实际 *复制* 任何数据到特定于 GPU 的物理内存中。

+   [提交](https://github.com/mikecvet/go-mm/blob/main/metal.m#L224)命令缓冲区以进行执行，并等待结果或在完成时设置事件处理程序

+   从响应缓冲区中提取字节，并使用 CPU 程序代码在本地格式化

原始 GPU 编程使用异步模型。

# Metal 着色语言

[Metal 着色语言](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) 是 [C++14](https://en.cppreference.com/w/cpp/14) 的一种衍生语言，可用于编写自定义逻辑（称为“着色器”），以在兼容 Metal 的 GPU 上运行。一般来说，如果可能的话，使用 [MPS 框架](https://developer.apple.com/documentation/metalperformanceshaders)（[稍后讨论](#890a)）来实现等效功能可能更好——它通常针对常见的 GPU 对齐用例（如矩阵乘法或 [神经网络](https://developer.apple.com/documentation/metalperformanceshadersgraph/training_a_neural_network_using_mps_graph)）进行了高度优化。

MSL代码的调试相当困难。你可以通过Xcode使用[着色器调试器](https://developer.apple.com/documentation/xcode/debugging-the-shaders-within-a-draw-command-or-compute-dispatch/)，但如果你想在没有Xcode的情况下检查或打印中间值，你需要将数据写入响应调试缓冲区，并在你的C++或Objective-C包装器中解析这些原语。

MSL函数通过`kernel`标识公开为公共接口。Metal框架传递当前调用线程上下文或线程组的ID，这些ID可以用来确保非重叠写入。线程可以通过三维ID系统表示；这个线程空间的维度在包装器代码中配置。

以下是[原始矩阵乘法](https://github.com/mikecvet/go-mm/blob/main/mm.metal#L9)算法的实现，结合了一些循环展开，令人惊讶地显著提高了性能。这只是为了比较；通常，MPS的`MPSMatrixMultiplication`功能会更合适。

```py
kernel void matrix_multiply_naive(
  device const MatrixParams *params,
  constant float *A,
  constant float *B,
  device float *C,
  // Indicates the thread's unique position within the entire grid of 
  // threads being executed. The uint2 type is a 2D coordinate, with 
  // fields x and y representing its indices on each axis.
  // This parameter is not directly provided from the calling code, 
  // but provided by the Metal framework
  uint2 gid [[thread_position_in_grid]]
) {
  if (gid.x >= params->a_rows || gid.y >= params->b_cols) {
    return; // This thread is out of matrix dimensionality range, do nothing
  }

  float sum = 0.0;
  int k;

  // Loop unrolling; improves performance by a notable margin
  for (k = 0; k <= params->a_cols - 4; k += 4) {
    sum += A[gid.x * params->a_cols + k] 
       * B[k * params->b_cols + gid.y];
    sum += A[gid.x * params->a_cols + k + 1] 
       * B[(k + 1) * params->b_cols + gid.y];
    sum += A[gid.x * params->a_cols + k + 2] 
       * B[(k + 2) * params->b_cols + gid.y];
    sum += A[gid.x * params->a_cols + k + 3] 
       * B[(k + 3) * params->b_cols + gid.y];
  }

  // Handle any remaining elements
  for (; k < params->a_cols; ++k) {
    sum += A[gid.x * params->a_cols + k] * B[k * params->b_cols + gid.y];
  }

  C[gid.x * params->b_cols + gid.y] = sum;
}
```

我还在MSL中实现了一个[原始转置函数](https://github.com/mikecvet/go-mm/blob/main/mm.metal#L42)以供比较。给定一个转置矩阵，这是对上述逻辑的一个微不足道的调整，其内部循环遍历B的行而不是列：

```py
// Loop unrolling; improves performance by a notable margin
for (k = 0; k <= params->a_cols - 4; k += 4) {
  sum += A[gid.x * params->a_cols + k]     
    * B[gid.y * params->b_cols + k]; // Note this is gid.y * cols plus k
  sum += A[gid.x * params->a_cols + k + 1] 
    * B[gid.y * params->b_cols + k + 1];
  sum += A[gid.x * params->a_cols + k + 2] 
    * B[gid.y * params->b_cols + k + 2];
  sum += A[gid.x * params->a_cols + k + 3] 
    * B[gid.y * params->b_cols + k + 3];
}

// Handle any remaining elements
for (; k < params->a_cols; ++k) {
  sum += A[gid.x * params->a_cols + k] * B[gid.y * params->b_cols + k];
}
```

我在[早期的博客文章中讨论了这种方法](https://betterprogramming.pub/better-than-cubic-complexity-for-matrix-multiplication-in-rust-cf8dfb6299f6#740c)，这是一种相当简单的方法，可以提高原始算法的标量性能，至少在CPU上是如此。更多内容稍后会讨论。

# Objective-C绑定

Metal框架提供了[从Metal源代码编译库](https://developer.apple.com/documentation/metal/mtldevice/1433431-newlibrarywithsource)的能力。一旦文件内容被加载，绑定代码会按[名称](https://github.com/mikecvet/go-mm/blob/main/metal.m#L51)查找内核函数，并初始化一个新的`MTLComputePipelineState`，表示编译后的函数代码。

```py
id<MTLDevice> device = MTLCreateSystemDefaultDevice();

// Compile and initialize a new library located at the provided source path.
MTLCompileOptions *compileOptions = [MTLCompileOptions new];
compileOptions.languageVersion = MTLLanguageVersion3_0;

// Wrap input source path string
NSString *ss = [NSString stringWithUTF8String:source_path];

// Initialize new library containing compiled shader functions
id<MTLLibrary> lib = [device newLibraryWithSource:ss
  options:compileOptions
  error:&error];

// Create a representation of the naive multiplication public shader function in 
// the Metal library created above
id<MTLFunction> naiveFunction =
    [lib newFunctionWithName:@"matrix_multiply_naive"];

// Create the new compute pipeline state
id<MTLComputePipelineState> pipelineStateNaive = [device newComputePipelineStateWithFunction:naiveFunction
  error:&error];
```

为了实际调用原生Metal代码，线程配置[需要设置](https://github.com/mikecvet/go-mm/blob/main/metal.m#L200)，并初始化GPU缓冲区。

```py
[computeEncoder setComputePipelineState:pipelineStateNaive];

MTLSize threadsPerGrid = MTLSizeMake(params->a_cols, params->a_rows, 1);

// Calculate a threadgroup size.
// https://developer.apple.com/documentation/metal/calculating_threadgroup_and_grid_sizes?language=objc
NSUInteger w = pipelineStateNaive.threadExecutionWidth;
NSUInteger h = pipelineStateNaive.maxTotalThreadsPerThreadgroup / w;
MTLSize threadsPerThreadgroup = MTLSizeMake(w, h, 1);

// Encode kernel function inputs
[computeEncoder setBytes:params length:16 atIndex:0];
[computeEncoder setBuffer:bufferA offset:0 atIndex:1];
[computeEncoder setBuffer:bufferB offset:0 atIndex:2];
[computeEncoder setBuffer:bufferC offset:0 atIndex:3];

// Encode the compute command.
[computeEncoder dispatchThreads:threadsPerGrid 
  threadsPerThreadgroup:threadsPerThreadgroup];

// End the compute pass.
[computeEncoder endEncoding];

// Execute the command.
[commandBuffer commit];
```

这内容比较多，我在这里阐明一下关系：

![](../Images/b233737dc027f8a96e6a7e61a0e7d67b.png)

Objective-C包装器中的概念、类型和硬件的高级布局

# Metal性能着色器框架

[MPS框架](https://developer.apple.com/documentation/metalperformanceshaders)是苹果公司提供的高性能库，用于其[Metal GPU系列](https://support.apple.com/en-us/102894)。它提供从图像任务到[神经网络支持](https://developer.apple.com/documentation/metalperformanceshaders/training_a_neural_network_with_metal_performance_shaders)的功能。

API 主要通过 Swift 或 Objective-C 提供，尽管也有一个 [Metal-cpp](https://developer.apple.com/metal/cpp/) 库可供使用。

[MPSMatrixMultiplication API](https://developer.apple.com/documentation/metalperformanceshaders/mpsmatrixmultiplication) [相对容易使用](https://github.com/mikecvet/go-mm/blob/main/metal.m#L128)。与上述 MSL 代码一样，MPS 命令仍需编码到 `MTLCommandBuffer` 中，并异步提交执行。

```py
// Define Matrix "descriptions", accounting for matrix dimensionality and byte size
MPSMatrixDescriptor *descriptorA = [MPSMatrixDescriptor matrixDescriptorWithDimensions:a_rows
  columns:a_cols
  rowBytes:a_cols * sizeof(float)
  dataType:MPSDataTypeFloat32];

MPSMatrixDescriptor *descriptorB = [MPSMatrixDescriptor matrixDescriptorWithDimensions:b_rows
  columns:b_cols
  rowBytes:b_cols * sizeof(float)
  dataType:MPSDataTypeFloat32];

// Output matrix
MPSMatrixDescriptor *descriptorC = [MPSMatrixDescriptor matrixDescriptorWithDimensions:a_rows
  columns:b_cols
  rowBytes:b_cols * sizeof(float)
  dataType:MPSDataTypeFloat32];

// Initialize matrix representations using above descriptions and matrix buffers
MPSMatrix *matrixA = [[MPSMatrix alloc] initWithBuffer:bufferA descriptor:descriptorA];
MPSMatrix *matrixB = [[MPSMatrix alloc] initWithBuffer:bufferB descriptor:descriptorB];
MPSMatrix *matrixC = [[MPSMatrix alloc] initWithBuffer:bufferC descriptor:descriptorC];

// Creates the multiplication instance
MPSMatrixMultiplication *matrixMultiplication = [[MPSMatrixMultiplication alloc] initWithDevice:device
  resultRows:a_rows
  resultColumns:b_cols
  interiorColumns:a_cols];

// Encodes the multiplication command into the command buffer for the GPU
id<MTLCommandBuffer> commandBuffer = [commandQueue commandBuffer];
[matrixMultiplication encodeToCommandBuffer:commandBuffer
  leftMatrix:matrixA
  rightMatrix:matrixB
  resultMatrix:matrixC];
```

# Go 和 cgo

我不特别喜欢使用 Objective-C，这个程序的重点是运行源自 Go 程序的 GPU 代码。

[Cgo](https://pkg.go.dev/cmd/cgo) 是一种 Go 语言功能，允许 Go 编译器理解与本地 C 代码相关的注释中的编译指令。它支持一种 [外部函数接口](https://en.wikipedia.org/wiki/Foreign_function_interface)。

指令配置有点*脆弱*，但任何紧接着 `import “C”` 行的注释（称为“前言”）在编译引用的 C 代码时将被解释为头文件导入或编译参数。例如：

```py
/*
#cgo LDFLAGS: -framework Foundation -framework CoreGraphics -framework Metal -framework MetalPerformanceShaders -L/opt/homebrew/opt/openblas/lib -lopenblas
#include <stdlib.h>
#include "metal.h"
*/
import "C"
```

+   通过命令行 `LDFLAGS` 传递链接标志给链接器

+   使用标准头文件 `stdlib.h` 编译 C 代码

+   使用本地项目头文件 `metal.h` 编译 C 代码

需要一些反复试验才能找到适用于 MacOS 的正确链接器标志。

+   `Foundation`：基础库

+   `CoreGraphics`：在 MacOS 上与 GPU 接口时必需

+   `Metal`：用于 Metal 的库和语言支持，包括 MSL

+   `MetalPerformanceShaders`：上述讨论的 MPS 库

事实证明，Apple 在其 `Accelerate` 框架中捆绑了一个 BLAS 实现，因此除了通过 `brew` 安装 OpenBLAS 外，还需要在链接时提供库的位置：

`-L/opt/homebrew/opt/openblas/lib -lopenblas`

`go:embed` 指令允许 Go 程序在编译时包含文件，这在我们希望将 MSL 源文件（`mm.metal`）的内容传递给 Metal 框架时是 [非常有用的](https://github.com/mikecvet/go-mm/blob/main/main.go#L18)，如上所述，用于编译。

```py
//go:embed mm.metal
var source string

// Compile the shader source code and initialize pipelines. The metalSource
// param contains the contents of an embedded Metal Shading Language file.
func Compile (metalSource string) {
 // Wrap string in a C string
 src := C.CString(metalSource)

 // Free the above string after command queue is initialized
 defer C.free(unsafe.Pointer(src))

 // Compile the source, initialize pipelines and command queue
 C.initializePipelineAndCommandQueue(src)
}
```

上述对 `C` 的引用是通过 cgo 与 C API 接口，例如：

```py
// Calls initializeMTLBuffers from Obj-C bindings
C.initializeMTLBuffers(
 a_data,                  // Input opaque pointer for A
 b_data,                  // Input opaque pointer for B
 C.int(4),                // Converts 4 into C integer type
 C.int(a.Size()),         
 C.int(b.Size()),         
 C.int(a.Rows * b.Cols))

params := MatrixParams{
 a_rows: int32(a.Rows),
 a_cols: int32(a.Cols),
 b_rows: int32(b.Rows),
 b_cols: int32(b.Cols),
}

// Return an unsafe pointer to this MatrixParams struct, cast to 
// the native C representation defined in the shared header file
return (*C.MatrixParams)(unsafe.Pointer(&params));
```

注意，这意味着 `C` 是一个保留关键字，不能用作变量名。

# Go 实现基线和 OpenBLAS

我想将基于 GPU 的矩阵乘法性能与更高级的实现（如 [Gonum library](https://www.gonum.org)）以及直观的手写（且相对低效）实现进行比较。

我在 Go 中实现了几种不同的算法，包括这个并行转置的简单算法，它将乘法工作天真地划分到 N 个 goroutine 中：

```py
func (a Matrix[T]) TransposeMultParallel(b *Matrix[T]) *Matrix[T] {
 if a.Cols != b.Rows {
  panic("matrices are the wrong size for multiplication")
 }

 c_data := make([]T, a.Rows*b.Cols)
 t := b.Transpose()

 var wg sync.WaitGroup

 for i := 0; i < a.Rows; i++ {
  wg.Add(1) // Add a count to the WaitGroup for the new goroutine
  go func(i int) { // Kick off goroutine
   defer wg.Done() // Decrease the count when the goroutine completes
   ptr := i * b.Cols
   for j := 0; j < b.Cols; j++ {
    var sum T = 0.0
    for k := 0; k < a.Cols; k++ {
     sum += a.At(i, k) * t.At(j, k)
    }
    c_data[ptr+j] = sum
   }
  }(i)
 }

 wg.Wait() // Wait for all goroutines to complete
 return InitMatrixWithData(a.Rows, b.Cols, c_data)
}
```

`Gonum BLAS`是一个纯Go库，它[实现了BLAS接口](https://pkg.go.dev/gonum.org/v1/gonum/blas)。然而，它也可以配置为将代数运算转发到本地代码BLAS实现，例如通过[netlib](https://github.com/gonum/netlib)的[OpenBLAS](https://www.openblas.net)。

我上面展示了如何配置`cgo`以正确链接到MacOS上的OpenBLAS安装。在应用程序代码中，可以直接设置首选的BLAS实现。从基准测试代码：

```py
// Convert primitive arrays into gonum dense matrix types
gonum_a := mat.NewDense(a_rows, a_cols, a64_data)
gonum_b := mat.NewDense(b_rows, b_cols, b64_data)
gonum_c := mat.NewDense(a_rows, b_cols, nil)
gonum_d := mat.NewDense(a_rows, b_cols, nil)

// Configure Gonum to use Gonum-default Go implementation
blas64.Use(gonum.Implementation{})

// Run a multiplication using Gonum BLAS impl
start = time.Now()
gonum_c.Mul(gonum_a, gonum_b)
bdata.TimeGonumNative(start)

// Configure Gonum to use Netlib which forwards operations to a 
// native C-code BLAS implementation (OpenBLAS in our case)
blas64.Use(netlib.Implementation{})

// Run a multiplication using OpenBLAS impl through Gonum API
start = time.Now()
gonum_d.Mul(gonum_a, gonum_b)
bdata.TimeGonumOpenBLAS(start)
```

# 结果

我的[基准测试代码](https://github.com/mikecvet/go-mm/blob/main/benchmark.go)运行了几次以下矩阵乘法实现的试验，并报告了每次乘法两个逐渐增大的方阵所花费的平均时间：

```py
- Naive multiplication, in Go
- Transposed naive multiplication, in Go
- Goroutine-parallelized transposed naive multiplication, in Go
- Gonum pure Go-based BLAS multiplication
- Gonum-wrapped OpenBLAS multiplication, written in C
- Hand-implemented naive multiplication, in MSL, on GPU
- Hand-implemented transposed naive multiplication, in MSL, on GPU
- Metal Performance Shaders framework, called from Objective-C, on GPU
```

基准测试输出如下（浮点数为毫秒）：

```py
2023-12-01 11:12:51.644 go-mm[75818:22427382] Using default device Apple M2
elements naive transpose transpose_parallel metal_naive metal_transpose mps gonum openblas
160000 196.00 201.00 42.00 8.00 9.67 0.33 4.67 6.00
250000 381.33 387.67 80.67 11.00 11.67 0.00 8.33 21.00
360000 801.00 789.33 159.33 19.00 16.33 0.00 14.33 4.67
490000 1228.00 1075.00 411.00 23.67 24.33 1.00 26.67 16.33
...
```

[一些快速绘图](https://github.com/mikecvet/go-mm/blob/main/plot.py)通过`matplotlib`

![](../Images/b8774a41fe0cc0304901089327c9e37c.png)

所有方法的性能图

正如预期，我手写的Go实现相对失控。实际上，其他方法速度如此之快，以至于在图中无法区分它们。以下是这次运行的GPU使用滑动直方图

![](../Images/106a8c798394b1afc407f5e27746208c.png)

活动监视器GPU历史可视化 — 所有方法（Y轴为使用百分比）

你可以看到GPU并不是特别忙碌，因为时间主要花在了CPU操作上。以下是另一轮测试，排除了最慢的三种乘法技术：

![](../Images/6e3a2d0a1e5df20c4f6d9843234287ac.png)

排除我手写的Go变体的各种方法性能图

大约16M元素（4k x 4k），`Gonum`开始下降。可以清楚地看到，基于GPU的和`OpenBLAS`操作优于纯Go实现。仅看基于GPU的方法：

![](../Images/37744ae5c88393237a3dc3ecf77fec1d.png)

仅在GPU上运行的矩阵乘法操作性能图

这里有几个有趣的笔记：

+   Metal Performance Shaders库的速度惊人

+   天真的方法和转置天真的方法之间没有实际性能差异

对于第二点：这与上述Go基础的实现对比性能特性不同。结果表明，对CPU有利的缓存访问模式在GPU上效果不同，尤其是它们的SIMD组（[或warps](https://developer.apple.com/documentation/metal/compute_passes/creating_threads_and_threadgroups)）访问内存的方式。见GPU利用率以便比较：

![](../Images/8b1b8a86d76081114882d01182891873.png)

活动监视器GPU历史可视化 — 仅GPU操作

现在仅查看`OpenBLAS`和`MPS` — 这两种最快的方法：

![](../Images/9ea4490d60e353f04df49b3a5fd25d06.png)

OpenBLAS与Apple的Metal Performance Shaders MPSMatrixMultiplication API性能对比图

在大约35M元素时，`OpenBLAS` 实现开始下降，而 `MPS` 则保持稳定。这里的差异相当显著，后者完成相同的35M元素矩阵乘法操作的时间少于15%。可以合理地假设，随着矩阵规模的增长，这种差异会继续扩大。

当然，这两种方法之间可能存在算法差异，因此这不是一个公平的CPU与GPU比较。如果我绘制我两个手工编码实现的性能差异图，它看起来是这样的：

![](../Images/ea1d29dc0e64b1077fd68ad3517ceba8.png)

我的MSL编写的矩阵乘法代码与Go编写的代码的性能比率图

这意味着，基于MSL的简单实现完成5M *元素的乘法操作仅需我Go实现的1%时间*，而这种比率似乎随着时间的推移对GPU更有利。
