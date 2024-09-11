# 批量 K-Means 与 Python Numba 和 CUDA C

> 原文：[https://towardsdatascience.com/batched-k-means-with-python-numba-and-cuda-c-3d4946c587b9?source=collection_archive---------2-----------------------#2023-11-27](https://towardsdatascience.com/batched-k-means-with-python-numba-and-cuda-c-3d4946c587b9?source=collection_archive---------2-----------------------#2023-11-27)

## 如何加速你的数据分析 1600x 与 Scikit-Learn（附代码！）

[](https://medium.com/@alex.w.shao?source=post_page-----3d4946c587b9--------------------------------)[![Alex Shao](../Images/07a2cd8de23969f5732995b0fda2a25e.png)](https://medium.com/@alex.w.shao?source=post_page-----3d4946c587b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d4946c587b9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3d4946c587b9--------------------------------) [Alex Shao](https://medium.com/@alex.w.shao?source=post_page-----3d4946c587b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F723362c2a30f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbatched-k-means-with-python-numba-and-cuda-c-3d4946c587b9&user=Alex+Shao&userId=723362c2a30f&source=post_page-723362c2a30f----3d4946c587b9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d4946c587b9--------------------------------) ·15分钟阅读·2023年11月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d4946c587b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbatched-k-means-with-python-numba-and-cuda-c-3d4946c587b9&user=Alex+Shao&userId=723362c2a30f&source=-----3d4946c587b9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d4946c587b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbatched-k-means-with-python-numba-and-cuda-c-3d4946c587b9&source=-----3d4946c587b9---------------------bookmark_footer-----------)![](../Images/79476c6455fbebdbb31e6dad7cb9a6ee.png)

图像由[Midjourney](https://www.midjourney.com/)基于作者的绘图生成

并行化数据分析工作负载可能是一个令人畏惧的任务，尤其是当你的特定用例没有有效的现成实现时。在本教程中，我将讲解如何用 C 和 Python Numba 编写 CUDA 内核的原则，以及这些原则如何应用于经典的 K-means 聚类算法。到文章结束时，你将能够用 C 和 Python 编写自定义的并行化批处理 K-means 实现，与标准的 scikit-learn 实现相比，实现高达 1600 倍的加速。如果你想直接跳到代码，它可以在 [Colab](https://colab.research.google.com/drive/1Il_OyESH92iVapFas0oTarF74IQcM3sq?usp=sharing) 中找到。

## 介绍

我们将研究在 NVIDIA GPU 上进行并行化的两个框架：Python Numba 和 CUDA C。然后，我们将在这两者中实现相同的批处理 K-means 算法，并将它们与 scikit-learn 进行基准测试。

Numba 对于那些更喜欢 Python 而不是 C 的人来说，是一个更为温和的学习曲线。Numba 将你的代码部分编译成称为**内核**的专用 CUDA 函数。CUDA C 是一个更低层次的抽象层，提供更细致的控制。Colab 示例设计得非常紧密地反映算法实现，因此一旦你理解了其中一个，就能轻松理解其他的。

本教程来源于我必须编写的自定义批处理 **k-means** 实现，我将以此作为说明示例。通常，k-means 库是针对在极大数据集上运行算法的单实例进行优化的。然而，在我们的项目中，我们需要并行处理数百万个独立的小数据集，这不是我们能找到现成的库的。

本文分为四个主要部分：CUDA 基础、并行化 K-means 算法、Python Numba 和 CUDA C。我将尽量使材料相对自包含，并在需要时简要回顾概念。

# CUDA 基础

**CPU** 设计用于快速的顺序处理，而 **GPU** 设计用于大规模并行处理。对于我们的算法，我们需要在数百万个小型独立数据集上运行 K-means，这非常适合 GPU 实现。

我们的实现将使用 **CUDA**，即计算统一设备架构，是 NVIDIA 开发的 C 库和计算平台，用于利用 GPU 进行并行计算。

要理解 GPU 内核和设备函数，这里有一些定义：

**线程** 是可以独立执行的单个指令序列。

**核心** 是可以执行单个线程的处理单元。

**warp** 是最小的线程调度单元。每个 warp 由 32 个线程组成，并将它们调度到核心上。

**多处理器**，也称为流式多处理器（SM），由固定数量的核心组成。

**GPU 内核** 是一种设计为在多个 GPU 线程上并行执行的函数。内核函数由 CPU 调用，并在 GPU 上执行。

**设备函数** 是一种可以被内核函数或另一个设备函数调用的函数。此函数在 GPU 上执行。

这些内核被组织成 **网格**、**块** 和 **线程** 的层次结构。在 GPU 的上下文中，每个线程连接到一个核心，并执行内核的一个副本。

![](../Images/7ca4bde115664dba1490f8d5a3508a04.png)

GPUFunction[(x, y), z](数据结构)。图片来自作者。

**块** 是在一个多处理器上运行的一组线程。

**网格** 是一种抽象数组，用于组织线程块。网格用于通过索引将内核实例映射到线程。

在 GPU 上，有两种类型的内存：全局内存和共享内存。

**全局内存** 存储在 DRAM（动态随机存取内存）中。所有线程都可以访问其内容，但代价是较高的内存延迟。

**共享内存** 存储在缓存中，且对同一块内的线程是私有的。它的访问速度远快于全局内存。

# 并行化 K-Means 算法

现在让我们介绍 k-means 算法。K-means 是一种无监督的聚类算法，它将数据集划分为 k 个不同且不重叠的簇。给定一组数据点，我们首先初始化 k 个质心，即起始的簇中心：

![](../Images/4a070de7a2e4b4e264a3e5d204301933.png)

质心初始化（k=3）。图片来自作者。

然后，在选择初始质心后，迭代执行两个步骤：

1.  **分配步骤：** 每个数据点根据欧几里得距离被分配给离它最近的质心。

1.  **更新步骤：** 质心位置重新分配为上一步骤中分配给该质心的所有点的均值。

这些步骤会重复进行，直到收敛，或当质心位置不再显著变化时。输出的是一组 k 个簇质心坐标，以及一个标记簇索引（称为**标签**）的数组，用于每个原始数据点。

![](../Images/afea9c8c722a794cc07e166dd1149978.png)

K-means 算法（k = 3）。图片来自作者。

对于大型数据集，质心初始化可能会显著影响算法的输出。因此，程序会尝试多个初始质心，称为初始种子，并返回最佳种子的结果。每个种子从初始数据集中选择，且不重复——这意味着没有初始质心会被重复。我们算法的最佳种子数量是数据点数量的三分之一。在我们的程序中，因为我们对每一行100个数据点运行 k-means，最佳的种子数量为33。

在我们的 k-means 函数中，一百万行由块表示，而线程代表种子。块中的线程被组织成 warps，这是硬件架构中最小的线程调度单元。每个 warp 包含 32 个线程，因此将块大小设置为 32 的倍数是最佳的。每个块然后输出具有最小惯性的种子数据，惯性由簇中心与其分配点之间的欧几里得距离之和来衡量。

![](../Images/3adcb0e40e317278702305fbf5431809.png)

并行化 K-means — GPU 端。图像由作者提供。

[Colab](https://colab.research.google.com/drive/1Il_OyESH92iVapFas0oTarF74IQcM3sq?usp=sharing) 在这里链接，你可以在 Python 或 C 中跟随。

```py
global variables: numRows, lineSize, numClusters
def hostKMeans:
    inputData = initializeInputData(numRows, lineSize)
    outputCentroids = createEmptyArray(numRows, numClusters)
    outputLabels = createEmptyArray(numRows, lineSize)

    sendToDevice(inputData, outputCentroids, outputLabels)
    cuda_kmeans[blockDimensions, threadDimensions](inputData, outputCentroids, outputLabels)
    waitForKernelCompletion()
    copyFromDevice(outputCentroids, outputLabels)
```

在我们的 k-means 算法中，我们开始时会设置全局变量。我们需要从 CPU 和 GPU 中引用这些变量。

尽管全局变量可以从内核访问，但内核不能直接将值返回给主机。为了绕过这一限制，我们的代码的 CPU 部分将两个空数组传递给内核，并附带输入数据。这些数组将用于将最终的质心和标签数组复制回 CPU。

在实例化了我们的数据结构后，我们调用内核，定义网格和块的维度作为一个元组。对内核的调用包括传递我们的数据、质心和标签的内存分配，以及初始随机质心初始化的状态。

```py
def KMeansKernel(data, outputCentroids, outputLabels)
    row = currentBlock()
    seed = currentThread()

    sharedInputRow = sharedArray(shape=(lineSize))
    sharedInertia = sharedArray(shape=(numSeeds))
    sharedCentroids = sharedArray(shape=(numSeeds, numClusters))
    sharedLabels = sharedArray(shape=(numSeeds, lineSize))

    sharedInputRow = data[row]

    synchronizeThreads()
    if seed == 0
        centroids = initializeCentroids(data)
    synchronizeThreads()

    KMeansAlgorithm(sharedInputRow, sharedCentroids, sharedLabels)

    sharedInertia[Seed] = calculateInertia(sharedInputRow, sharedCentroids, sharedLabels)

    synchronizeThreads()
    if seed == 0
        minInertiaIndex = findMin(Inertia)
    sharedOutputCentroids = centroids[minInertiaIndex]
    sharedOutputLabels = labels[minInertiaIndex]
```

一旦在**设备**（即 GPU）上，我们的代码现在同时存在于整个网格中。为了确定我们在网格上的位置，我们访问块和线程索引。

在 GPU 上，内存默认为**全局内存**，存储在**DRAM**上。**共享内存**对同一块中的线程是私有的。

通过将数据的单行（所有线程必须读取的行）转移到共享内存中，我们减少了内存访问时间。在 cuda_kmeans 函数中，我们创建了共享内存来存储数据行、质心、标签和每个种子的准确性度量，称为惯性。

在我们的程序中，每个线程对应一个种子，所有线程在一个块中处理相同的数据行。对于每个**块**，一个线程按顺序创建 32 个种子，并将它们的结果聚合到一个数据结构中，以供块中的其他线程使用。

当算法中的后续步骤依赖于此聚合已完成时，线程必须使用内置的 CUDA syncthreads() 函数进行**同步**。**注意：** 必须对 syncthreads() 调用的位置**非常小心**，因为尝试在不是所有线程都已完成时同步线程可能会导致**死锁**和整个程序的挂起。

我们的内核函数在下面的伪代码中描述，名为cuda_kmeans。这个函数负责安排上述过程，为所有32个种子结果留出空间，并选择最佳种子以生成质心和标签的最终输出。

```py
def KMeansDevice(dataRow, centroids, labels)
    seed = currentThread()
    centroidsRow = centroids[seed]
    labelsRow = labels[seed]

    centroidsRow = sort(centroidsRow)
    yardStick = computeYardstick(sortedCentroids)

    oldCentroids = localArray(shape=(numSeeds, numClusters))

    for iteration in range(100):
        if converged(oldCentroids, centroidsRow)
            break
        oldCentroids = copy(centroidsRow)
        assignLabels(dataRow, centroidsRow, labelsRow)
        updateCentroids(dataRow, centroidsRow, labelsRow)
```

从cuda_kmeans我们调用实际的k-means算法，传递新实例化的共享内存。在我们的k-means算法中，我们首先选择初始质心，然后将它们从小到大排序。我们迭代地将数据点分配给最近的质心，并更新质心位置，直到收敛。

为了确定是否已经达到了收敛，我们使用一个名为find_yard_stick的辅助函数。这个函数计算并返回两个初始质心之间的最小距离（yard_stick）。当质心在单次迭代中移动的距离都不超过yard_stick乘以epsilon时，我们的收敛条件就满足了。

在收敛后，我们返回到cuda_kmeans。在这里，我们通过计算每个质心与其数据点之间的平方欧几里得距离来确定最佳种子。具有最有效分组的种子——通过最小的**惯性**来表示——被认为是最佳的。然后，我们从这个种子中提取质心和标签，并将它们复制到输出数组中的一行。一旦所有块都完成，这些输出将被复制回主机（CPU）。

![](../Images/e023eb19ea4a1684ef6fee357567e11a.png)

K-means算法中的数据传输。图片由作者提供。

## Numba 简介

设计自定义内核最简单的方法是使用Numba。**Numba**是一个Python库，可以用于将Python代码编译成CUDA内核。

![](../Images/934081746e8272fba1084b302a68e739.png)

抽象层次。图片由作者提供。

在底层，Numba与CUDA进行接口。为了并行化代码，Numba将你指定的GPU代码编译成内核并传递给GPU，将程序逻辑分为两个主要部分：

1.  CPU级别代码

1.  GPU级别代码

使用Numba，你可以将代码中的顺序部分和可并行部分分开，分别交给CPU和GPU处理。为了将函数编译到GPU上，程序员在函数定义上方使用**@cuda.jit**装饰器，从而将该函数转换为一个**内核**，该内核从CPU（主机）调用，但在GPU（设备）上并行执行。

# Python Numba

链接到[Colab](https://colab.research.google.com/drive/1Il_OyESH92iVapFas0oTarF74IQcM3sq?usp=sharing)。

Numba作为Python代码与CUDA平台之间的桥梁。由于Python代码与上述算法伪代码几乎相同，因此我只提供几个关键相关语法的示例。

```py
cuda_kmeans[(NUM_ROWS,), (NUM_SEEDS,)](input_rows, output_labels, output_centroids, random_states)
```

在实例化了必要的全局变量和数据结构后，我们可以从主机调用内核 cuda_kmeans。Numba 需要两个元组来表示块和线程的维度。由于我们将使用一维块和线程，因此每个元组中的第二个索引为空。我们还传入了我们的数据结构和一个随机状态数组用于随机种子的实例化。

```py
@cuda.jit()
def cuda_kmeans(input, output_labels, output_centroids, random_states):
    row = cuda.blockIdx.x
    seed = cuda.threadIdx.x
    shared_input_row = cuda.shared.array(shape=(LINE_SIZE), dtype=np.float32)
    shared_inertia = cuda.shared.array(shape=(NUM_SEEDS), dtype=np.float32)
    shared_centroids = cuda.shared.array(shape=(NUM_SEEDS, NUM_CLUSTERS), dtype=np.float32)
    shared_labels = cuda.shared.array(shape=(NUM_SEEDS, LINE_SIZE), dtype=np.int32)
    if seed == 0:
        get_initial_centroids(shared_input_row, shared_centroids, random_states)
    cuda.syncthreads()

    ...

    kmeans(shared_input_row, shared_labels, shared_centroids)
```

我们使用 Numba 的 @cuda.jit() 装饰器来标记进行 GPU 编译。使用 cuda.blockIdx.x 和 cuda.threadIdx.x 符号，我们可以获得内核的当前索引。共享数组通过 cuda.shared.array 使用两个参数进行实例化，形状和类型，这两者必须在编译时已知。在获取质心并填充行数据后，我们调用 kmeans 函数，填充共享数组，并调用 cuda.syncthreads()。

```py
@cuda.jit(device=True)
def kmeans(data_row, output_labels, output_centroids): 
    seed = cuda.threadIdx.x
    labels_row = output_labels[seed]
    centroids_row = output_centroids[seed]

    ...

    old_centroids = cuda.local.array(shape=(NUM_CLUSTERS), dtype=np.float32)

    for iteration in range(NUM_ITERATIONS):
            if iteration > 0:
                if converged(centroids_row, old_centroids, yard_stick * EPSILON_PERCENT, iteration):
                    break
      # Assign labels and update centroids
```

K-means 是一个 **设备函数**，因为它是从内核函数中调用的。因此，我们必须在装饰器中指定 device=True：**@cuda.jit(device=True)**。k-means 函数随后获取当前行的标签和质心，并运行直到收敛。

只需额外增加十几行代码和一点点努力，你的 Python 代码就可以成为一个准备好进行并行处理的优化内核。

我们的并行 k-means 确实大幅减少了计算时间——然而，将像 Python 这样的高级语言进行包装和编译并不一定是最优的。出于好奇，我想看看用 C 语言编写代码是否能加速我们的项目，于是我深入了 CUDA C 的领域。

## C 语言简介

在 Python 中，内存和类型分配是自动的，而在 C 语言中，内存可以分配在栈上或堆上，这两者都需要显式的类型声明，并分配固定量的内存。栈内存由编译器自动分配和释放，而堆内存由程序员在运行时使用 malloc() 等函数手动分配，程序员负责其释放。

指针是持有变量内存地址的工具。指针所指向的数据类型在声明时定义。指针使用星号 (*) 指定。要获取变量的地址，称为引用，你需要使用与符号 (&)。要从指针中访问值，你再次使用星号来解除引用。

双指针，或指向指针的指针，存储指针的地址。这在将数组传递给函数时修改其他指针的地址非常有用。当将数组传递给函数时，它们作为指向其第一个元素的指针传递，没有大小信息，依赖于指针算术来索引数组。要从函数返回数组，你需要使用 & 传递指针的地址，并用双指针 ** 接收它，这样你就可以修改原始指针的地址，从而传递数组。

```py
int var = 100; // declare type
int *ptr = &var; // use of a pointer and reference
int **double_ptr = &ptr; // example of double pointer
printf(“Dereference double_ptr and ptr: %d %d \n:”, **double_ptr, *ptr)
int *ptr = 100; // initialize pointer to int type
```

# CUDA C

链接到 [Colab](https://colab.research.google.com/drive/1Il_OyESH92iVapFas0oTarF74IQcM3sq?usp=sharing)。

CUDA 是一个计算平台，利用 NVIDIA GPU 来并行处理复杂的计算问题。在你刚掌握 C 的基础上（开玩笑的），CUDA C 代码的结构与我们步进的伪代码结构完全相同。

在 CPU 端，我们设置一些常量来告诉算法预期的结果，导入库，初始化变量，并定义一些宏。

```py
#define NUM_ROWS 00000        // y dimension of our data set, number of blocks
#define LINE_SIZE 100         // x dimension of our data set
#define NUM_ITERATIONS 100    // max number of iterations
#define NUM_CLUSTERS 3        // We are running k = 3
#define MAX_INPUT_VALUE 100   // Upper bound on data
#define NUM_SEEDS 32          // Number of seeds/threads is 1/3 of LINE_SIZE
#define EPSILON_PERCENT 0.02  // Condition for convergence

void initInputData(float** input) {
    srand(1); 
    // allocate memory for the data

    ... // initialize data using malloc and rand
    // allocate memory on GPU
    cudaMalloc(input, NUM_ROWS * LINE_SIZE * sizeof(float)); 
    // copy memory from CPU sample_data to GPU memory
    cudaMemcpy(*input, sample_data, NUM_ROWS * LINE_SIZE * sizeof(float), cudaMemcpyHostToDevice);
    free(sample_data);
}

int main() {
    float* inputData; // initialize input data, dimensions will by NUM_ROWS x LINE SIZE
    initInputData(&inputData); // dereference and pass to function
    // initialize output labels and centroids
    cudaExtent labelsExtent = make_cudaExtent(sizeof(int), LINE_SIZE, NUM_ROWS);
    cudaPitchedPtr outputLabels; // create the pointer needed for the next call
    cudaMalloc3D(&outputLabels, labelsExtent); // allocate memory on GPU

    cudaExtent centroidsExtent = make_cudaExtent(sizeof(float), NUM_CLUSTERS, NUM_ROWS);
    cudaPitchedPtr outputCentroids; // create the pointer needed for the next call
    cudaMalloc3D(&outputCentroids, centroidsExtent); // allocate memory on GPU
    cuda_kmeans <<<NUM_ROWS, NUM_SEEDS>>> (inputData, outputLabels, outputCentroids);
    cudaDeviceSynchronize();

    ... // copy output from device to back to host
}
```

让我们分解这些差异。

主函数首先通过创建一个指针并将其作为地址传递给 initInputData 来初始化数据。该函数接收 inputData 作为指向指针的指针（float** input），这使得函数能够修改原始指针所持有的地址。然后，输入被指向通过 cudaMalloc 初始化的 GPU 内存地址，并使用 cudaMemcpy 填充，从临时主机数组 sample_data 中复制数据，该数组已经填充了随机数。

然后，代码在设备上分配内存来保存来自 k-means 函数的结果。该函数使用 make_cudaExtent 创建一个 cudaExtent 对象，目的是封装多维数组的维度。

类型 cudaPitchedPointer 用于定义一个能够寻址这个倾斜内存空间的指针。这种指针专门用于处理由 cudaMalloc3D 分配的内存，后者需要 cudaPitchedPtr 和 cudaExtent 对象来分配 GPU 上的线性内存。

```py
cuda_kmeans <<<NUM_ROWS, NUM_SEEDS>>> (inputData, outputLabels, outputCentroids);
```

进入 GPU 代码时，我们定义网格，使每个块对应于一行数据，每个线程对应于一个种子。

种子由每个块中的一个线程初始化，确保种子完全不同。

```py
__global__ void cuda_kmeans(float* input, cudaPitchedPtr outputLabels, cudaPitchedPtr outputCentroids) {
    int row = blockIdx.x;
    int seed = threadIdx.x;

    // shared memory is shared between threads in blocks
    __shared__ float input_shm[LINE_SIZE];
    __shared__ float error_shm[NUM_SEEDS];
    __shared__ float seed_centroids[NUM_SEEDS][NUM_CLUSTERS];
    __shared__ int seed_labels[NUM_SEEDS][LINE_SIZE];

    ... // get a single row of data
    ... // populate input_shm
    ... // populating the struct core_params
    // the actual k-means function
    kmeans(core_params);

    // find seed with smallest error
    calcError(core_params);
    __syncthreads();
    if (seed == 0) {
        int* labels_line = LABELS_LINE(outputLabels, row);
        float* centroids_line = CENTROIDS_LINE(outputCentroids, row);
        labels_line[threadIdx.x] = seed_labels[seed][threadIdx.x];
        centroids_line[threadIdx.x] = seed_centroids[seed][threadIdx.x];
    }
}
```

CUDA C 代码使用共享内存存储数据、质心、标签和错误。然而，与 Python 不同的是，代码将共享内存的指针存储在一个结构体中，这只是传递变量的一种方法。最后，cuda_kmeans 调用实际的 k-means 算法并传递 core_params。

```py
__device__ void kmeans(core_params_t& core_params) {
    DECLARE_CORE_PARAMS(core_params);
    getInitialCentroids(core_params);
    sort_centroids(centroids, num_clusters);
    float yard_stick = findYardStick(core_params);
    float* oldCentroids = (float*)malloc(NUM_CLUSTERS * sizeof(float));
    struct work_params_t work_params;
    work_params.min = find_min(line, LINE_SIZE);
    work_params.max = find_max(line, LINE_SIZE);
    work_params.aux_buf1 = (int*)malloc(NUM_CLUSTERS * sizeof(int));
    work_params.aux_buf2 = (int*)malloc(NUM_CLUSTERS * sizeof(int));
    work_params.aux_buf3 = (float*)malloc(NUM_CLUSTERS * sizeof(float));
    for (int iterations = 0; true; iterations++) {
        bool stop = (iterations > 100) || (iterations > 0 && (converged(core_params, oldCentroids, yard_stick * EPSILON_PERCENT)));
        if (stop)
            break;
        memcpy(oldCentroids, core_params.centroids, NUM_CLUSTERS * sizeof(float));
        getLabels(core_params);
        getCentroids(core_params, work_params);
    }
    free(work_params.aux_buf1);
    free(work_params.aux_buf2);
    free(work_params.aux_buf3);
    free(oldCentroids);
}
```

在设备函数中，首先我们使用 DECLARE_CORE_PARAMS 宏从 core_params 结构体中提取值到变量中。

然后，我们运行与 Python 中相同的 k-means 算法，唯一的区别是我们传递结构体而不是变量，并且需要管理内存、指针和类型。

# 基准测试

为了将我们的算法与非并行化的 k-means 进行比较，我们导入了 scikit-learn 的 k-means 模块。

在我们的基准测试中，我们运行 100,000 行和 100 列的三簇数据。由于 scikit-learn 没有针对不同行的并行化 k-means，我们在 for 循环中顺序运行这些行。

在 colab 的基准测试中，我们使用免费的 T4 GPU Colab 实例。

![](../Images/43b25f0e293f72b6503a1ede9ed7c5c2.png)

图片由作者提供。

结果很好——Python Numba代码比非并行化的CPU代码快两个数量级，而CUDA C代码快三个数量级。内核函数易于扩展，算法可以修改以支持更高维度的聚类。

请注意，C和Python算法中的随机初始质心生成并没有完全优化以使用所有核心。当优化后，Python算法的运行时间可能会非常接近C代码的运行时间。

![](../Images/a29d96b77f789ca427e1d575f9d8871d.png)

基于2023年11月23日的免费Colab T4 GPU的运行时间。图像由作者提供。

在不同数据集上运行k-means函数一百次并记录结果时间后，我们注意到第一次迭代显著较慢，因为编译C和Python代码在Colab中需要时间。

# 结论

现在你应该准备好编写自己的自定义GPU内核了！一个剩下的问题可能是——你应该使用CUDA C还是Numba来处理并行的数据处理工作负载？这要看情况。两者都比现成的scikit-learn快得多。虽然在我的案例中，CUDA C的批处理k-means实现比使用Numba编写的等效实现快了大约3.5倍，但Python提供了一些重要的优势，比如可读性以及对专门C编程技能的依赖较少，特别是对于主要使用Python的团队。此外，你的具体实现的运行时间将取决于你是否优化了代码，例如，避免在GPU上触发序列化操作。总之，如果你对C和并行编程都不熟悉，我建议先使用Numba来原型化你的算法，然后如果需要额外的加速，再将其转化为CUDA C。

# 参考文献

1.  [Scikit-learn: Python中的机器学习](https://jmlr.csail.mit.edu/papers/v12/pedregosa11a.html)，Pedregosa *等人*，JMLR 12，第2825–2830页，2011年。

1.  NVIDIA, Vingelmann, P. & Fitzek, F.H.P., 2020\. CUDA，版本：10.2\. 89，获取地址：[https://developer.nvidia.com/cuda-toolkit.](https://developer.nvidia.com/cuda-toolkit.)

1.  Lam, Siu Kwan, Antoine Pitrou, 和 Stanley Seibert. [“Numba: 基于llvm的python JIT编译器。”](https://numba.pydata.org) *第二届LLVM编译器基础设施在HPC研讨会论文集*。2015年。

1.  Harris, C.R., Millman, K.J., van der Walt, S.J. 等. “*使用NumPy的数组编程*。” Nature 585，357–362（2020年）。DOI: [10.1038/s41586–020–2649–2](https://doi.org/10.1038/s41586-020-2649-2)。 ([出版商链接](https://www.nature.com/articles/s41586-020-2649-2))
