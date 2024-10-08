# 4 个失败的物理信息神经网络的想法

> 原文：[`towardsdatascience.com/4-ideas-for-physics-informed-neural-networks-that-failed-ce054270e62a`](https://towardsdatascience.com/4-ideas-for-physics-informed-neural-networks-that-failed-ce054270e62a)

## 这是一个关于 PINNs 的扩展列表，这些扩展要么没有提高性能，要么完全破坏了它们——所以你不必自己尝试。

[](https://rabischof.medium.com/?source=post_page-----ce054270e62a--------------------------------)![拉斐尔·比绍夫](https://rabischof.medium.com/?source=post_page-----ce054270e62a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce054270e62a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce054270e62a--------------------------------) [拉斐尔·比绍夫](https://rabischof.medium.com/?source=post_page-----ce054270e62a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce054270e62a--------------------------------) ·9 分钟阅读·2023 年 2 月 11 日

--

![](img/3cf70f1cea6074bd3edb27d1586c5dfd.png)

图片由 [DeepMind](https://unsplash.com/es/@deepmind?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在 [物理信息神经网络（PINNs）](https://www.sciencedirect.com/science/article/pii/S0021999118307125) 的世界里，就像在任何其他新兴的机器学习领域一样，似乎每个人都急于分享他们发现的改进这些模型的惊人技术。我自己也不例外，已经发布了三篇关于改进 PINNs 性能的有用扩展的文章：

+   [通过自适应损失平衡来改进 PINNs](https://medium.com/@rafael.bischof07/improving-pinns-through-adaptive-loss-balancing-55662759e701)

+   [PINNs 的专家混合（MoE-PINNs）](https://medium.com/@rafael.bischof07/mixture-of-experts-for-pinns-moe-pinns-6520adf32438)

+   [10 个提升物理信息神经网络（PINNs）的实用提示和技巧](https://medium.com/@rafael.bischof07/10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001)

然而，常常被忽视的是那些未能奏效的无数想法。现实是，提升 PINNs 的旅程并不总是一帆风顺，许多有前途的想法最终都未能实现。

> 我没有失败。我只是找到了一万种行不通的方法。— **托马斯·A.** **爱迪生**

在本文中，我想探讨一些有前途但遗憾未能取得预期结果的 PINNs 增强想法。因此，秉持托马斯·爱迪生的精神，加入我一起深入探讨那些未成功的 PINNs 扩展。

一如既往，文章附带一个包含本文介绍概念的笔记本：

[## Burgers_PINN_fails.ipynb

### Colaboratory 笔记本

[drive.google.com](https://drive.google.com/file/d/1dEJeWjmHjmKBzpDnQQqw0IIEXlnr-Rj1/view?usp=sharing&source=post_page-----ce054270e62a--------------------------------)

# 引言

PINN 与经典神经网络在处理和表示数据的方式上有所不同。当查看 PINN 中隐含表示的 TSNE 图时，这一点立即变得明显：

![](img/1f7fccf8759b3badfc2883e2a6582adc.png)

在训练有三层的 PINN 模型上进行的隐含向量的 3D TSNE 表示。左侧：基尔霍夫 PDE 的参考解。中间：第一层（蓝色）、第二层（绿色）和第三层（橙色）的表示。右侧：数据点根据位置点的 x + y 值着色，其中 x 控制像素颜色中的红色量，y 控制蓝色量。图由作者提供。

在经典神经网络中，评估出相似结果的协同位置点会在隐含向量中表示得很接近。在 PINN 中情况并非如此。每一层的表示不仅位于流形的完全不同区域，即使是协同位置点的坐标也不会混合。

这些特殊性质使得处理 PINN 变得困难且违反直觉，即使对于经验丰富的数据科学家也是如此。尽管该领域的进展主要来自理论和数学界，但以经验为驱动的研究人员在获得立足点方面遇到了困难。

本文尝试通过展示未能成功的扩展和想法来增加对 PINN 内部工作原理的理解，这些扩展和想法要么未能提高准确性，要么完全破坏了 PINN 的训练流程。

# 1. 批量归一化

批量归一化是深度学习中一种流行的技术，用于归一化每一层的激活，这可以提高训练稳定性并减少过拟合。然而，由于其训练过程的特殊性，PINN 中不能使用批量归一化。

在 PINN 中，训练过程涉及最小化一个损失函数，该函数衡量模型预测与系统底层物理之间的差异。这些物理定律通常不包含有关如何处理基于点的均值和标准差进行动态偏移和缩放的输入的信息。通过基于随机小批量样本的统计数据来扰动数据，批量归一化“破坏”了指导 PINN 训练的物理规律，因此不应使用。

批量归一化的替代方法可以是层归一化，它根据同一层中的节点值对节点上的值进行归一化，或是可学习的归一化，其中一个密集层负责产生偏移和缩放，并且进行正则化，以使层中的值具有零均值和单位方差。

然而，在进行各种实验后，我发现，在 PINN 中，逐层归一化可能并非必要。实际上，加快模型收敛的一个有效且简单的方法是，在输入网络之后添加一层。这一层通过使用样本的物理域范围，将配点缩放到 [-1, 1] 的范围。

```py
import tensorflow as tf
from tensorflow.keras.layers import Input, Concatenate, Dense

def pinn_model(n_layers:int, n_nodes:int, activation:tf.keras.activations, x_range:tuple, y_range:tuple):
    x = Input((1,), name=name+'_input_x')
    y = Input((1,), name=name+'_input_y')

    # normalize data between -1 and 1
    x_norm = (x - x_range[0]) / (x_range[1] - x_range[0])
    y_norm = (y - y_range[0]) / (y_range[1] - y_range[1])
    xy = Concatenate()([x_norm, y_norm]) * 2 - 1

    u = Dense(n_nodes, activation=activation, kernel_initializer='glorot_normal')(xy)
    for i in range(1, n_layers):
        u = Dense(n_nodes, activation=activation, kernel_initializer='glorot_normal')(u) + u
    u = Dense(1, use_bias=False, kernel_initializer='glorot_normal')(u)

    return tf.keras.Model([x, y], u)
```

# 2\. ReLU**(n+1) 激活

确保网络中使用的激活函数可以足够多次可微是至关重要的。这是因为 PINN 的输出需要多次相对于输入进行导数计算（根据 PDE 的微分阶数），并且还需要一次相对于模型权重进行导数计算。

具体而言，激活函数应具有 n+1 个非零导数，其中 n 是所解 PDE 的微分阶数。这个要求排除了流行的激活函数 ReLU，因为它的一阶导数是常数（0 或 1），而其二阶导数在所有地方都是零（不包括未定义的零点）。

然而，将 ReLU 激活函数提升到功率 n，可以使其可微 n 次，这可能是克服这一限制的一个有希望的思路。更好的是，通过将 PINN 初始化为多个并行层，每个层使用不同值 k ≤ n+1 的 ReLU，我们可以确保不同微分阶数的模式由网络的不同部分捕捉。

![](img/951539246a5ad15d9861eb75b5ac2309.png)

示例架构展示了一个具有多个平行层的 PINN，使用了不同功率 k ≤ n + 1 的 ReLU 激活函数。图由作者提供。

然而，让我非常失望的是，这种架构未能收敛。我只能猜测这是由于功率 k 的层没有从作用于更高阶微分的损失项中获得更新。但在推断时，当预测 u 时，即使所有 PDE 的组件没有完全通知，每一层都会参与计算。这可能导致噪声的注入，从而限制了架构的建模能力。

# 3\. 卷积神经网络

在二维域上求解 PDE 时，人们可能会倾向于在网格中采样配点，然后将其视为图像中的像素。这将允许使用 CNN，由于其空间不变性的归纳偏差，CNN 是图像处理中的广泛使用架构。

然而，请记住，PINNs 是通过物理定律进行训练的。这些定律通常不包含有关如何从多个配点处聚合值的信息。因此，考虑多个配点的内核并没有太大意义。此外，CNN 的一个重要特性是它们可以在多个抽象层次上找到局部模式，前几层本质上作为边缘检测器。但在 PINNs 中，第一层内核的输入只是等间距配点的坐标。从中没有模式可以学习。

为了使 CNN 有效，你需要[修改 PINNs 的设置和目标](https://arxiv.org/abs/2004.13145) [2]。

# 4\. 向量量化

[向量量化](https://arxiv.org/abs/1711.00937) [3]是一种机器学习技术，它将高维连续数据映射到一组离散符号中。在 PINNs 中使用向量量化层的想法是引入一个离散组件，以帮助捕捉 PDE 中的间断性。

神经网络在建模尖锐的间断性时表现不佳，因为其逼近需要使用一个步进函数，而步进函数本身不可微。通过将向量量化融入架构中，可能会认为 PINNs 能够更好地处理这些尖锐的过渡。这是因为向量量化层本质上充当了一个“离散化”步骤，将连续输入空间划分为较小、更易管理的段，并在它们之间形成明显的边界。

![](img/2ad5a78570a2e614a4e082d450b38624.png)

向量量化 PINNs（VQPINNs）可能的外观示意图。作者绘制的图。

首先，坐标 x 和 y 通过一个密集层映射到更高维空间。然后计算该向量与训练字典中一组向量之间的余弦相似度，并选择最相似的向量作为 PINN 的附加输入。因此，PINN 接收的输入包括配点和源自离散操作的向量。该操作通过[直通估计器](https://arxiv.org/abs/1308.3432) [4]被近似为可微。

然而，回到本文主题，这一努力充满了挫折和失望，因为一个有前途的想法再次没有结出成果。当我在[巴克利-利弗雷特方程](https://arxiv.org/pdf/2112.14826.pdf) [5]上训练一个 PINN，该方程在整个领域内存在间断时，我的模型确实将其划分为两个段，并在之间形成了一个尖锐的边界。但边界从未在正确的位置，并且子区域内主方程的建模也不令人满意。

![](img/b7125210073ddb489644e79a956acbe1.png)

Buckley-Leverett PDE 在整个领域上有一个明显的不连续性。图由[Diab et al.](https://arxiv.org/pdf/2112.14826.pdf) [5]

我最好的猜测是，不连续性使物理领域出现了明显的分割，网络的不同部分专注于不同的子领域。这意味着，如果网络的部分仅应用于自己子领域之外的区域，它们可能会与边界或初始条件断开连接。这也是为什么 PINNs 的集合，如[MoE-PINNs](https://medium.com/@rafael.bischof07/mixture-of-experts-for-pinns-moe-pinns-6520adf32438) [6]，必须确保门控网络保持连续函数，即不要使 softmax 操作过于尖锐。否则，边界条件和初始条件不会在整个领域内强加，从而导致不正确的解决方案。

# 结论

成功实现和扩展 PINNs 是一项困难且常常违背直觉的工作。已建立的（批量归一化，CNNs）或来自其他机器学习领域的有前景的特性（向量量化）无法应用于 PINNs，这表明从业者需要深入了解内部机制，以提出有效的扩展和想法。

这些想法对于使 PINNs 更强大，加速收敛，以及最终使其成为像有限元方法这样的已建立数值方法的有前景的替代方案是非常需要的。

感谢你抽时间阅读这篇文章。我鼓励你亲自尝试这些想法，并且非常期待听到有人对这些想法进行小的调整，从而突然使它们有效的消息！为了铺平这样的发现之路，我创建了一个笔记本，你可以在其中玩转这些想法的实现：

[## Burgers_PINN_fails.ipynb

### Colaboratory 笔记本

drive.google.com](https://drive.google.com/file/d/1dEJeWjmHjmKBzpDnQQqw0IIEXlnr-Rj1/view?usp=sharing&source=post_page-----ce054270e62a--------------------------------)

如果你有更多的建议或推荐，我非常愿意在评论中听到它们！你可以在[mkrausai.com](http://mkrausai.com/)上找到关于我的同事 Michael Kraus 的更多信息，在[rabischof.ch](http://rabischof.ch/)上找到关于我自己的更多信息。

[1] M. Raissi, P. Perdikaris, 和 G. E. Karniadakis, 物理信息神经网络：解决涉及非线性偏微分方程的正向和逆向问题的深度学习框架，*计算物理学杂志* 378 (2019), 686–707。

[2] Gao, Han, Luning Sun, 和 Jian-Xun Wang. “PhyGeoNet: 物理信息几何自适应卷积神经网络用于解决不规则领域的参数化稳态 PDEs。” *计算物理学杂志* 428 (2021): 110079。

[3] Van Den Oord, Aaron, 和 Oriol Vinyals. “神经离散表示学习。” *神经信息处理系统进展* 30 (2017)。

[4] Bengio, Yoshua, Nicholas Léonard 和 Aaron Courville。“通过随机神经元估计或传播梯度以进行条件计算。” *arXiv 预印本 arXiv:1308.3432* (2013)。

[5] Diab, Waleed 和 Mohammed Al Kobaisi。“用于非凸流量函数的双曲 Buckley-Leverett 问题的 PINNs 解决方案。” *arXiv 预印本 arXiv:2112.14826* (2021)。

[6] R. Bischof 和 M. A. Kraus，“[物理信息神经网络的专家混合集成元学习](https://scholar.google.com/scholar?oi=bibs&cluster=12593425659851579449&btnI=1&hl=en)”，第 33 届建筑信息学论坛论文集，2022。
