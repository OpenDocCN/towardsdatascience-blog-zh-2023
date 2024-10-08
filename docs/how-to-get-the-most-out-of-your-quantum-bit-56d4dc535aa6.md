# 如何充分利用你的量子比特

> 原文：[`towardsdatascience.com/how-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6`](https://towardsdatascience.com/how-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6)

## 量子比特的内容远不止 0 和 1

[](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)![Frank Zickert | Quantum Machine Learning](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------) [Frank Zickert | Quantum Machine Learning](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 11 日

--

想开始学习量子机器学习吗？看看[**使用 Python 进行量子机器学习实操**](https://www.pyqml.com/volume1?provider=medium&origin=mostofqubit)**。**

量子计算机是 21 世纪最有前景的技术之一。然而，我们仍处于研究早期阶段，以搞清楚如何大规模地构建可靠的量子计算机。

但量子计算机不再是科幻小说。IBM、谷歌、微软、亚马逊以及许多其他公司已经开始了利用现有量子计算机潜力的商业化倡议。

然而，主要挑战在于当前设备的量子比特（qubit）数量极少。虽然我们不能简单地增加这个数量，但我们可以改变使用量子比特的方式。

本文提出了一种增加量子比特中存储信息量的新方法。

量子比特处于叠加状态。这是一种复杂（即复数）的线性关系，存在于其两个基态之间。

![](img/404e41a8830212ae0356bdc1219619d1.png)

作者提供的图像

量子比特不是离散的，而是连续的。它有两个维度，即实际幅度和相位。理论上具有无限精度，可以存储无限量的数据。

一旦你对其进行测量，它的状态要么是 0，要么是 1。量子比特失去了它的魔力。它会坍缩到仅有的两个值之一。

这是一个问题，因为当我们请求量子计算机提供一个解决方案时，它只能有两个可能的答案。0 或 1。是或否。这限制了提问复杂问题的可能性。

例如，当我们旨在解决一个优化问题时，我们期望系统配置的答案是导致最低成本的配置。因此，我们需要为系统配置的每个部分配备一个量子比特。但使用我们目前可以访问的 27 量子比特处理器，我们无法解决任何经典计算机无法解决的问题。

当我们研究需要 n²的旅行商问题时

量子比特和 n 是需要访问的地点数量，甚至当前的 433-Osprey 也只能解决 21 个地点的问题。

如果我们希望在可预见的未来利用量子计算，我们需要更充分地利用我们的量子比特。我们不能对现有的少量量子比特浪费。

不久前，我们的经典比特非常少。少到我们用只有两位数字来存储年份，这最终导致了 Y2K 问题。

当然，量子比特稀缺可能有不利影响，但我们现在并不处于像经典比特那样充裕的状态。因此，是时候恢复稀缺意识了。

我们需要从量子比特中提取比零或一更多的信息。事实上，一种量子计算技术恢复了不可见的状态：量子态层析。

假设我们在不同的可观察量下测量一个量子比特，每个维度一个，并计算每个维度的期望值。这使我们能够创建一个描述系统量子状态完整图景的密度矩阵。

状态层析的问题在于，随着量子比特数量的增加，可观察量的数量呈指数增长。由于我们必须为每个可观察量单独运行电路，这增加了复杂性。但是，避免这种情况是使用量子计算机的主要目的。

幸运的是，隧道的尽头有一线曙光。

我们无论如何都会运行几千次量子电路，因为量子比特具有概率性。通过这种方式，我们希望找到最可能的解决方案。然而，作为这些计算的副产品，我们得到许多未使用的答案。然而，仅查看最可能的结果而忽略其余部分是资源的浪费。

尤其是当我们考虑到重复执行电路会揭示更多关于基础量子状态的信息时。这使我们能够计算期望值。尽管这仍然只是整个量子状态的一部分，期望值比二进制测量（零或一）更为详细。

因此，如果我们查看期望值，我们会得到一个介于 0 和 1 之间的浮点数。准确度取决于实验中的重复次数。我们运行电路的次数越多，给出数字的准确度就越高。

例如，以下电路以 72.4%的概率产生 1。因此，期望值是 0.724（如果我们对测量结果进行加权）。这比将结果解释为“可能是 1”提供了更多信息。

```py
from math import asin, sqrt
from qiskit import QuantumCircuit, execute, Aer
from qiskit.visualization import plot_histogram

def prob_to_angle(prob):
    return 2*asin(sqrt(prob))

# Create a quantum circuit with one qubit
qc = QuantumCircuit(1)

# rotate the qubit state vector
qc.ry(prob_to_angle(0.7245), 0)

# Tell Qiskit how to simulate our circuit
backend = Aer.get_backend('statevector_simulator') 

# Do the simulation, returning the result
result = execute(qc,backend, shots=1000).result()

# get the probability distribution
counts = result.get_counts()

# Show the histogram
plot_histogram(counts)
```

![](img/aa07334cf7f35ea76c5c15dbd6a14bf7.png)

图片由作者提供

这个实验的警告是我们使用了状态矢量模拟器。这个模拟器计算精确的量子态。因此，完全没有随机性。不幸的是，我们只能对极少数量子比特进行这种操作，并且它不适用于我们在真实量子计算机上得到的结果。

以下示例展示了使用 QASM 模拟器进行的相同实验。

```py
# Create a quantum circuit with one qubit
qc = QuantumCircuit(1)

# rotate the qubit state vector
qc.ry(prob_to_angle(0.7245), 0)

# measure the qubit
qc.measure_all()

# Tell Qiskit how to simulate our circuit
backend = Aer.get_backend('qasm_simulator') 

# Do the simulation, returning the result
result = execute(qc,backend, shots=1000).result()

# get the probability distribution
counts = result.get_counts()

# Show the histogram
plot_histogram(counts)
```

![](img/85a11261e6696b4c3deaf5d1f84898ce.png)

作者提供的图片

`qasm_simulator`通过经验确定测量值。因此，结果并不是完全准确的。重复次数（由 shots 参数指定）越多，结果就越精确。

然而，提高精度的代价是需要更频繁地执行电路。因此，使用期望值检索信息并不是免费的午餐。

更糟的是，真实量子计算机的量子比特容易出错。这些错误进一步模糊了测量结果。以下示例在模拟中加入了这种噪声。

```py
from qiskit import transpile
from qiskit.providers.fake_provider import FakeQuito
from qiskit.providers.aer import AerSimulator

# create a fake backend
device_backend = FakeQuito()

# create a simulator from the backend
sim_quito = AerSimulator.from_backend(device_backend)

# transpile the circuit
mapped_circuit = transpile(qc, backend=sim_quito)

# run the transpiled circuit, no need to assemble it
result = sim_quito.run(mapped_circuit, shots=1000).result()

# get the probability distribution
counts = result.get_counts()

# Show the histogram
plot_histogram(counts)
```

![](img/7ba4e0eaece564391f48016b5ad34b57.png)

作者提供的图片

当噪声存在时，结果会进一步偏离完美结果。而且我们甚至还没有开始处理量子比特。我们与量子比特的互动越多，它就越容易受到噪声的影响。

这不可避免地导致精度损失。然而，在许多情况下，我们不需要对答案有很高的准确度。粗略的值通常就足够了。因此，我们不需要知道期望值精确为 0.724，但看到它在 0.7 左右就足够了。

但如果我们对浮点数不感兴趣，而是寻找一个离散的答案怎么办？

为什么我们不将不同的期望值解释为不同的离散数字呢？例如，我们可以将低于 0.25 的期望值解释为 0，将 0.25 到 0.5 之间的期望值解释为 1，将 0.5 到 0.75 之间的期望值解释为 2，将 0.75 以上的期望值解释为 3。这样，我们将在一个量子比特中编码四个离散值。

以下代码展示了这种离散化是如何工作的。

```py
def discretize(counts, blocks):
    weigthed = 0
    sum_count = 0
    print (counts)
    for key, value in counts.items():
        weigthed += int(key)*int(value*0.999*blocks)
        sum_count += value
    return int(weigthed/sum_count)

discretize(counts, 4) # Output here: 2
```

除了测量次数，离散化函数还将块数作为参数。这表示期望值被解释为不同值的数量。当然，我们可以选择任意数量的块。但我们在一个量子比特中编码的数字越多，它对不准确性和噪声的敏感度就越高。

## 结论

我认为，在面对量子比特极度稀缺的情况时，将超过两个值编码到一个量子比特中是值得的。

不幸的是，我们拥有的少量量子比特也容易受到噪声的影响。此外，当我们在一个量子比特中编码多个值时，我们对量子比特的准确性依赖更大。因此，这里提出的想法确实是一个折衷。

尽管如此，噪声是我们可以应对的。虽然这不容易，但我们可以掌握它。然而，我们只能使用可用的量子比特。如果这个数量太小以至于无法代表一个有意义的现实问题，我们就无法解决这些问题。

所以，如果我们想解决现实世界中的问题，我们必须确保有足够的量子比特，无论需要什么权衡。

[](https://pyqml.medium.com/membership?source=post_page-----56d4dc535aa6--------------------------------) [## 通过我的推荐链接加入 Medium - Frank Zickert | 量子机器学习

### 开始学习量子机器学习（并获得 Medium 上每篇文章的完整访问权限）获取所有内容的完整访问权限…

pyqml.medium.com](https://pyqml.medium.com/membership?source=post_page-----56d4dc535aa6--------------------------------)

不要错过下一集，订阅我的 [Substack 频道](https://pyqml.substack.com/)。

想要开始量子机器学习吗？看看 [**用 Python 实践量子机器学习**](https://www.pyqml.com/page?ref=medium_getmost&dest=%2F)**。**

![](img/c3892c668b9d47f57e47f1e6d80af7b6.png)

免费获取前三章 [这里](https://www.pyqml.com/page?ref=medium_getmost&dest=%2F)。
