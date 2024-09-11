# ICA 和现实中的鸡尾酒会问题

> 原文：[https://towardsdatascience.com/ica-and-the-real-life-cocktail-party-problem-6375ba35894b?source=collection_archive---------4-----------------------#2023-10-24](https://towardsdatascience.com/ica-and-the-real-life-cocktail-party-problem-6375ba35894b?source=collection_archive---------4-----------------------#2023-10-24)

## 为什么独立成分分析在其经典实验中失败了，以及我们可以从这种失败中学到什么。

[](https://medium.com/@ballkenneth?source=post_page-----6375ba35894b--------------------------------)[![Kenneth Ball](../Images/9ce4fb9e79dfa9aef0ecaccd8538693b.png)](https://medium.com/@ballkenneth?source=post_page-----6375ba35894b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6375ba35894b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6375ba35894b--------------------------------) [Kenneth Ball](https://medium.com/@ballkenneth?source=post_page-----6375ba35894b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c5f8506be44&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fica-and-the-real-life-cocktail-party-problem-6375ba35894b&user=Kenneth+Ball&userId=9c5f8506be44&source=post_page-9c5f8506be44----6375ba35894b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6375ba35894b--------------------------------) · 13分钟阅读 · 2023年10月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6375ba35894b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fica-and-the-real-life-cocktail-party-problem-6375ba35894b&user=Kenneth+Ball&userId=9c5f8506be44&source=-----6375ba35894b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6375ba35894b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fica-and-the-real-life-cocktail-party-problem-6375ba35894b&source=-----6375ba35894b---------------------bookmark_footer-----------)![](../Images/84a4b6ec8b37738a767b94d45139a697.png)

鸡尾酒会隐喻（作者图片）

自1990年代显著发展以来，独立成分分析（ICA）已成为一种常用的数据分解和预处理技术。ICA 是一种盲源分离（BSS）方法：一些独立的源被盲目混合，结果混合信号由若干观察者接收。ICA 方法通过寻找一个最小化解混成分之间互信息或最大化数据在这些成分上投影的“非高斯性”的基变换，来*解混*观察到的信号并寻找独立源。

已经有很多教程介绍了 ICA 及其应用：本文*并不是*另一个 ICA 的介绍。相反，这是一篇关于几乎总是伴随 ICA 解释的动机问题的评论。

几乎所有对 ICA 的介绍都利用鸡尾酒会问题作为 ICA 旨在解决的 BSS 问题的说明。² 鸡尾酒会问题*确实*是一个富有启发性和激励性的思维实验。只有一个小问题：ICA 在现实生活中的鸡尾酒会上会失败，而且其失败的原因*确实*应该影响 ICA 的应用方式。

# ICA 和鸡尾酒会

一个拥挤的房间。聚会的人群——手里拿着鸡尾酒——在彼此交谈。听众如何将混合的聊天声分离成不同的声音，或许还可以聚焦到一个单独的说话者？这就是鸡尾酒会问题的设置，这是用来介绍 ICA 的经典示例。想象一下，几个麦克风被放置在房间的不同位置。有人说，ICA 向我们揭示了如何将录制的信号分解为*独立*的组件，代表聚会上的不同说话者。

BSS 问题通过一个混合问题进行表述，其中一些独立源 *y* 被混合到观测信号 *x* 中。

对于*N* 个时间样本。*A* 是一个混合矩阵，我们用 *j* 和 *i* 索引源和观测。对于本文中的几个方程，我使用了爱因斯坦求和符号。

Scikit-learn 的分解模块包括了一个非常实用的 FastICA 实现，我们可以用它来展示在低维示例中它是如何工作的。我们将设置几个独立的来源，这些来源只是不同频率下相位偏移的正弦波，随机混合它们，然后应用 FastICA 尝试将其分解。我们看到的是——在缩放、符号和排列的范围内——FastICA 能很好地恢复原始信号（因此，在已知麦克风位置的实际问题中，我们可以恢复扬声器的方向/位置）。

```py
import numpy as np
from sklearn.decomposition import FastICA
import matplotlib.pyplot as plt

rng = np.random.default_rng(8675309)
t = np.linspace(0, 10, 10000)
x = np.array([
    np.sin(2 * np.pi * t), 
    np.sin(2 * np.pi / 2 * t + 1), 
    np.sin(2 * np.pi / 2 * 2 * t + 2)])
mixing = rng.uniform(-1, 1, [3, 3])
# check that our randomly generated mixing is invertible
assert np.linalg.matrix_rank(mixing) == 3
demixing_true = np.linalg.inv(mixing)

y = np.matmul(mixing, x)

fica = FastICA(3)
z = fica.fit_transform(np.transpose(y))
z = np.transpose(z)
z /= np.reshape(np.max(z, 1), (3, 1))
fig, ax = plt.subplots(3, 1, figsize = (8, 5))
for ii in range(3):
    ax[0].plot(t, x[ii])
    ax[1].plot(t, y[ii])
    ax[2].plot(t, z[ii])
ax[0].set_title("Independent Sources")
ax[1].set_title("Randomly, Linearly, Instaneously Mixed Signals")
ax[2].set_title("ICA Components (Match up to Sign and Linear Scaling)")
plt.tight_layout()
```

![](../Images/3604b7275c7a770169f471fe10b3b168.png)

FastICA 对瞬时混合信号

让我们列出一些鸡尾酒会模型的假设：

+   在鸡尾酒会的房间里，我们假设观察者（例如麦克风）的数量多于源（例如扬声器），这是问题不会欠定的必要条件。

+   源是独立的，并且不呈正态分布。

+   *A* 是一个常量矩阵：混合是瞬时的且不变的。

BSS 问题是“盲”的，所以我们注意到源 *y* 和混合 *A* 是未知的，我们寻求 *A* 的广义逆矩阵，称为分解矩阵 *W*。ICA 算法是推导 *W* 的策略。

![](../Images/2045b55f5764a7ab77e4999df2d3c135.png)

准备混合（图片由作者提供）

# 现实中的鸡尾酒会

如果我们在派对上实际设置一个麦克风阵列并尝试对录制的音频进行 ICA 会发生什么？恰好的是，开箱即用的 ICA 几乎肯定会在分离扬声器方面失败得很惨！

让我们重新审视我们模型的一个假设：特别是瞬时混合。由于声音的有限传播速度，从扬声器位置发出的音频会以不同的时间延迟到达房间中的每个麦克风。

在派对上，声音的传播速度约为 343 米/秒，因此距离扬声器 10 米的麦克风会录制大约 0.03 秒的延迟。虽然对在派对上的人来说，这似乎几乎是瞬时的，但对于 10 kHz 级别的录音来说，这意味着数百个数字样本的延迟。

尝试将这个时间延迟的盲混合信号输入到原始 ICA 中，结果不会很美观。但等等，难道没有 ICA 用于去混合音频的例子吗？³ 是的，但这些玩具问题是数字化和*瞬时混合*的，因此与 ICA 模型假设一致。现实世界的录音不仅存在时间延迟，还会受到更复杂的时间变换（下面会详细讨论）。

我们可以重新审视上面的玩具示例，并在源和录音麦克风之间引入随机延迟，以观察当模型假设被违反时，FastICA 如何开始崩溃。

```py
rng = np.random.default_rng(8675309)
t = np.linspace(0, 11, 11000)
x = np.array([
    np.sin(2 * np.pi * t), 
    np.sin(2 * np.pi / 2 * t + 1), 
    np.sin(2 * np.pi / 2 * 2 * t + 2)])
mixing = rng.uniform(-1, 1, [3, 3])
# check that our randomly generated mixing is invertible
assert np.linalg.matrix_rank(mixing) == 3
demixing_true = np.linalg.inv(mixing)

delays = rng.integers(100, 500, (3, 3))
y = np.zeros(x.shape)
for source_i in range(3):
    for signal_j in range(3):
        x_ = x[source_i, delays[source_i, signal_j]:]
        y[signal_j, :len(x_)] += mixing[source_i, signal_j] * x_
t = t[:10000]
x = x[:, :10000]
y = y[:, :10000]

fica = FastICA(3)
z = fica.fit_transform(np.transpose(y))
z = np.transpose(z)
z /= np.reshape(np.max(z, 1), (3, 1))
fig, ax = plt.subplots(3, 1, figsize = (8, 5))
for ii in range(3):
    ax[0].plot(t, x[ii])
    ax[1].plot(t, y[ii])
    ax[2].plot(t, z[ii])
ax[0].set_title("Independent Sources")
ax[1].set_title("Randomly, Linearly, Time-Delayed Mixed Signals")
ax[2].set_title("ICA Components (Match up to Sign and Linear Scaling)")
plt.tight_layout()
```

![](../Images/b85732da9a683ea892fbbb06daebe038.png)

针对延迟混合信号的 FastICA：注意到去混合的成分与源信号的形状发生了偏离。

# 时间延迟问题

值得更详细地检查一下为什么 ICA 不能处理这些时间延迟。毕竟，我们已经在处理一个未知的混合，难道我们不能处理一点时间扰动吗？更进一步，原始 ICA 在结构化数据上是*排列不变的*！你可以对时间序列或图像数据集进行抽样或像素顺序的洗牌，然后得到相同的 ICA 结果。那么，为什么 ICA 不会对这些时间延迟具有鲁棒性呢？

在现实世界的鸡尾酒会中，问题在于每对扬声器和麦克风之间有不同的时间延迟。将每个来自扬声器的数字样本视为来自随机变量的抽样。当没有延迟时，每个麦克风在同一时间听到相同的抽样/样本。然而，在现实世界中，每个麦克风记录的是相同扬声器的*不同*延迟样本，就像混合矩阵 A 是未知的，时间延迟也是未知的。当然，实际问题甚至比单一延迟值更复杂：混响、回声和衰减会在信号到达麦克风之前进一步扩散源信号。

让我们更新我们的模型公式，以表示这种复杂的时间延迟。假设房间的声学特性没有实际变化，麦克风和扬声器保持在相同的位置，我们可以写出：

其中 *k* 表示离散时间延迟索引，而混合矩阵 *A* 现在是一个矩阵函数，随着 *k = 0…T* 而变化。换句话说，*i-* 号麦克风的实际观察值是回溯 *T* 个样本的源信号的线性混合。此外，我们可以注意到，每个源/麦克风对的单一时间延迟问题（没有更复杂的声学效应）是上述模型公式的一个子情况，其中矩阵 *A* 在每个 *(i, j)* 索引对的一个 *k* 值下取非零形式。

数学上感兴趣的人，或者那些沉浸于信号处理神秘技艺中的人，会注意到现实世界的鸡尾酒会问题模型⁴开始很像 *卷积*。事实上，这是一种功能卷积的离散模拟，通过傅里叶变换我们可以得到一个可能更易处理的频率空间版本的问题。

这里有很多内容需要解读。鸡尾酒会的卷积表示简洁地揭示了为什么 ICA 在对 BSS 问题的简单处理上注定要失败。现实世界的多传感器音频录音几乎肯定是一个 *去卷积* 问题，而不是线性解混问题。尽管仍然可以找到问题的近似解决方案（我们将在下面讨论一些策略），但不应假设 ICA 能在时间域中提供有空间意义的解混，除非 *做大量* 更多的工作。

我们可以再一次回顾我们的示例，通过设计一个随机绝对延迟和长度的非线性卷积来模拟一个基本的卷积。在这种情况下，我们可以真正开始看到 FastICA 组件解决方案与原始源信号显著不同。

```py
rng = np.random.default_rng(8675309)
t = np.linspace(0, 11, 11000)
x = np.array([
    np.sin(2 * np.pi * t), 
    np.sin(2 * np.pi / 2 * t + 1), 
    np.sin(2 * np.pi / 2 * 2 * t + 2)])
mixing = rng.uniform(-1, 1, [3, 3])
# check that our randomly generated mixing is invertible
assert np.linalg.matrix_rank(mixing) == 3
demixing_true = np.linalg.inv(mixing)

delays = rng.integers(100, 500, (3, 3))
impulse_lengths = rng.integers(200, 400, (3, 3))
y = np.zeros(x.shape)
for source_i in range(3):
    for signal_j in range(3):
        impulse_length = impulse_lengths[source_i, signal_j]
        impulse_shape = np.sqrt(np.arange(impulse_length).astype(float))
        impulse_shape /= np.sum(impulse_shape)
        delay = delays[source_i, signal_j]
        for impulse_k in range(impulse_length):
            x_ = x[source_i, (delay + impulse_k):]
            y[signal_j, :len(x_)] += (
                mixing[source_i, signal_j] 
                * x_ * impulse_shape[impulse_k]
            )
t = t[:10000]
x = x[:, :10000]
y = y[:, :10000]

fica = FastICA(3)
z = fica.fit_transform(np.transpose(y))
z = np.transpose(z)
z /= np.reshape(np.max(z, 1), (3, 1))
fig, ax = plt.subplots(3, 1, figsize = (8, 5))
for ii in range(3):
    ax[0].plot(t, x[ii])
    ax[1].plot(t, y[ii])
    ax[2].plot(t, z[ii])
ax[0].set_title("Independent Sources")
ax[1].set_title("Randomly Convolved Signals")
ax[2].set_title("ICA Components (Match up to Sign and Linear Scaling)")
plt.tight_layout()
```

![](../Images/659b7a0e5de3230ebec68a074bee47cf.png)

对于卷积信号的 FastICA：注意到解混后的组件与源信号形状显著不同。

然而，频率空间版本的模型开始更像是一个 ICA 模型问题，至少作为一个线性混合问题。它并不完美：傅里叶变换后的混合矩阵函数在频率空间中并不是平稳的。然而，这可能是我们想要深入研究的问题所在，并且确实是更通用去卷积策略的起点。

# “现实世界”与 ICA

不管你做什么，不要在鸡尾酒会上使用 ICA 进行音频源分离。然而，ICA 是否在现实世界中有用呢？

让我们考虑 ICA 最常见的应用之一：脑电图 (EEG) 特征化和分解。EEG 信号是从头皮上的电极（有时也来自大脑中的电极）记录的电位时间序列。应用 ICA 到预处理的 EEG 数据中以识别大脑和身体中的独立电位信号源已经成为一个小型行业。

在 EEG 记录的情况下，ICA 模型的瞬时混合假设肯定得到了满足：电信号相对于人头的长度尺度和采样频率（通常为几十到几百赫兹）几乎是瞬时传播的。这对 ICA 是一个好兆头，事实上，独立成分通常会分离出一些空间上有意义的特征。眼球运动和肌肉活动（皮肤导电性将信号传播到头皮）通常是明显不同的成分。其他成分会在头皮上产生看似有意义的电极激活模式，这些激活被认为是由大脑中的神经元集合作为辐射偶极源产生的。根据头皮上电极位置的准确坐标映射，可以进一步推断这些源的三维位置和方向。

我们已经确定这里满足瞬时混合假设，但其他模型假设怎么样呢？如果电极在头皮上没有移动，并且受试者保持静止，那么常量混合也可能是一个合理的假设。我们测量的通道是否比源更多？ICA 不会生成比记录信号通道更多的独立成分，但如果实际源比可以辨别的源多得多，将空间意义赋予成分可能会存在问题。

最后，源是否独立？这可能会非常棘手！辐射偶极源当然不是单个神经元，而是许多神经元的集体尖峰活动。我们在 EEG 的采样时间尺度上有多大程度上相信这些一致的神经元簇是相互*独立*的？十年前，Makeig 和 Onton 对这一主题进行了广泛讨论和研究。⁶ 其要点是，源被认为是局部一致的皮层神经元块：邻近连接相对于远离连接的强度既会诱发“池塘涟漪”般的电位（集中在局部源处），也会减少空间上分隔的块之间的依赖关系。也就是说，关于通过 ICA 在复杂领域中检查 EEG 的卷积混合问题的兴趣曾间歇性出现。⁷ ⁸

# 解卷积与 ICA

ICA 是否仍然可以用来解决现实世界鸡尾酒会问题所示的解卷积问题？让我们回到 BSS 解卷积问题的频率空间表示。记住，这非常接近 ICA 能处理的情况……混合矩阵是线性变换，主要问题在于它不是频率的函数上的平稳的。如果我们对（盲）卷积做一些假设，我们可能能够将 ICA 适应到解卷积问题上。

假设频率空间混合在频率上连续且有些“缓慢”地变化。这里的“缓慢”指的是频率参数的微小变化会引起混合的较小变化。我们在术语上有些模糊，但总体思路是，给定足够的样本，我们可以将BSS问题划分到频率空间的子集上，并在每个子集内运行ICA，假设在频率子集内混合是静态的。例如，我们知道*全球范围*内混合随频率变化，但也许它变化得足够缓慢，以至于我们可以假设在频谱窗口中它是静态的。因此，在10到15 kHz之间，我们将使用一堆傅里叶变换样本来估计该频率窗口中的单一静态混合。

理论上，我们可以尝试在整个频率范围内对静态ICA解决方案进行插值。因此，如果我们有10–15 kHz的ICA解混方案和15–20 kHz的另一种解决方案，我们可以提出一些插值方案，将我们的两个解决方案中心放在12.5 kHz和17.5 kHz，然后推断这两个点之间的频率混合函数。

然而，有一些模糊之处需要解决。首先，解混矩阵不仅仅是向量，还有一些附加的群体结构我们可能需要关注。其次，ICA解的组件在排列和缩放方面是不变的……换句话说，再次把ICA视为基变换，任何基方向的重新排序或符号/大小的变化都是同样好的解决方案。因此，进行这种频率空间分布ICA的策略可以归结为如何解决相邻频率集之间ICA解决方案的匹配和一致性问题。

![](../Images/a1f1a286007537c31c967902b1a63810.png)

混合鸡尾酒（作者提供的图像）

# 无忧与细致的特征化

希望在所有这些中有一个更广泛适用的教训。即使在其模型假设是否得到满足存在一些模糊性的情况下，ICA也可以是一种非常强大的分解技术。事实上，作为研究人员，我几乎总是会选择FastICA进行降维，而不是——或者至少与——PCA进行比较。我特别喜欢用FastICA来处理更抽象的数据，而没有正式的BSS解释。

为什么ICA可以更普遍地使用？因为算法本身只是BSS解决方案的抽象近似。FastICA做的正是它所说的：它找到一种基变换，使数据组件在统计学上最大程度地非高斯——通过峰度（或多或少）来推断。如果这种变换恰好与物理上有意义的独立源重合，那就太好了！如果没有，它仍然可以作为一种有用的变换，类似于PCA的抽象用途。如果我们把PCA和FastICA看作是分别优化第二和第四阶统计量的基变换，它们甚至有非常松散的关系。

但必须小心不要对ICA结果进行过多的解读。我们可以说ICA成分是最大程度上独立的或非高斯的：当然没问题！但我们能否说ICA成分是物理上有意义的独立来源？只有在存在满足我们所提出的假设的基础BSS模型问题时才可以。抽象中的ICA成分可能确实指示了被非线性和卷积层掩盖的有用关系。我们只需小心不要在没有验证模型假设的情况下过度解读ICA。

## 参考文献和脚注

[1] ICA的两个历史上最著名的变种——FastICA和Infomax ICA——可追溯至：

A. [Hyv](https://direct.mit.edu/neco/article-abstract/9/7/1483/6120/A-Fast-Fixed-Point-Algorithm-for-Independent)ä[rinen](https://direct.mit.edu/neco/article-abstract/9/7/1483/6120/A-Fast-Fixed-Point-Algorithm-for-Independent) 和 E. Oja，[一种用于独立成分分析的快速固定点算法](https://direct.mit.edu/neco/article-abstract/9/7/1483/6120/A-Fast-Fixed-Point-Algorithm-for-Independent)（1997），《神经计算》

A. Bell 和 T. Sejnowski，[一种信息最大化方法用于盲分离和盲去卷积](https://ieeexplore.ieee.org/abstract/document/6796129)（1995），《神经计算》

[2] C. Maklin，[Python中的独立成分分析（ICA）](/independent-component-analysis-ica-in-python-a0ef0db0955e)（2019），《数据科学进展》

[3] J. Dieckmann，[ICA介绍：独立成分分析](/introduction-to-ica-independent-component-analysis-b2c3c4720cd9)（2023），《数据科学进展》

[4] 我们在这里稍微滥用了一些符号表示，比如忽略了音频录制的边界，例如当*t=0*时。别担心！毕竟，一切都是对数学符号的滥用。

[5] 在“现实世界”中，任何模型是否完全真实？答案是否定的，[Dr. Box](https://en.wikipedia.org/wiki/All_models_are_wrong)回答道。[Rob Thomas](https://en.wikipedia.org/wiki/Real_World_(Matchbox_Twenty_song))，厌倦了被打扰，也表示赞同。

[6] S. Makeig 和 J. Onton，[ERP特征和EEG动态：ICA视角](https://www.researchgate.net/publication/228639595_ERP_features_and_EEG_dynamics_An_ICA_perspective)（2012），《牛津事件相关电位成分手册》

[7] J. Anemüller, T. J. Sejnowski, 和 S. Makeig，[频域脑电图数据的复杂独立成分分析](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2925861/)（2003），《神经网络》

[8] A. Hyvärinen, P. Ramkumar, L. Parkkonen, 和 R. Hari，[短时傅里叶变换的独立成分分析用于自发EEG/MEG分析](https://www.cs.helsinki.fi/u/ahyvarin/papers/NIMG10reprint.pdf)（2009），《NeuroImage》
