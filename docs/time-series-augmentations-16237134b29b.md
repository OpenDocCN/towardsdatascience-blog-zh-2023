# 时间序列增强

> 原文：[https://towardsdatascience.com/time-series-augmentations-16237134b29b?source=collection_archive---------0-----------------------#2023-10-19](https://towardsdatascience.com/time-series-augmentations-16237134b29b?source=collection_archive---------0-----------------------#2023-10-19)

## 一种简单但有效的增加时间序列数据量的方法

[](https://medium.com/@an231?source=post_page-----16237134b29b--------------------------------)[![亚历山大·尼基廷](../Images/decc36cddc4c7a23952569e293e7d209.png)](https://medium.com/@an231?source=post_page-----16237134b29b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16237134b29b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16237134b29b--------------------------------) [亚历山大·尼基廷](https://medium.com/@an231?source=post_page-----16237134b29b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5afaa29ee25a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-augmentations-16237134b29b&user=Alexander+Nikitin&userId=5afaa29ee25a&source=post_page-5afaa29ee25a----16237134b29b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16237134b29b--------------------------------) ·8 min read·2023年10月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16237134b29b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-augmentations-16237134b29b&user=Alexander+Nikitin&userId=5afaa29ee25a&source=-----16237134b29b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16237134b29b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-augmentations-16237134b29b&source=-----16237134b29b---------------------bookmark_footer-----------)

*这篇博客文章可以在* [*GitHub上的jupyter notebook*](https://github.com/AlexanderVNikitin/tsgm/blob/main/tutorials/augmentations.ipynb)*找到。*

增强已成为计算机视觉管道中不可或缺的组成部分。然而，它们在其他领域如时间序列中的受欢迎程度却没有达到相同的高度。在本教程中，我将**深入探讨**时间序列增强的世界，阐明其重要性，并提供使用强大的生成时间序列建模库TSGM [5]的实际应用示例。

我们的起点是一个标记为（𝐗，𝐲）的数据集。在这里，𝐱ᵢ ∈ 𝐗 是多变量的（意味着每个时间点是一个多维特征向量）时间序列，而y是标签。预测标签y被称为下游任务。我们的目标是使用（𝐗，𝐲）生成额外的样本（𝐗*，𝐲*），这可以帮助我们更有效地解决下游任务（在预测性能或鲁棒性方面）。为了简化起见，我们在本教程中不会处理标签，但我们在这里描述的方法可以很容易地推广到有标签的情况，并且我们使用的软件实现可以通过向`.generate`方法添加额外参数来轻松扩展到监督情况（见下例）。

事不宜迟，让我们逐一考虑时间序列增强。

在TSGM中，所有的增强方法都整齐地组织在`tsgm.models.augmentations`中，你可以查看 [TSGM documentation](https://tsgm.readthedocs.io/en/latest/guides/introduction.html#augmentations) 提供的全面文档。

现在，让我们通过安装tsgm来启动编码示例：

```py
pip install tsgm
```

接下来，我们导入tsgm，并加载一个示例数据集。一个张量`X`现在包含100个长度为64的正弦时间序列，每个序列有2个特征。具有随机的偏移、频率和振幅（最大振幅为20）。

```py
# import the libraries
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import random
from tensorflow import keras
import tsgm
```

```py
# and now generate the dataset
X = tsgm.utils.gen_sine_dataset(100, 64, 2, max_value=20)
```

# 抖动 / Gaussian噪声

作为第一个增强方法，我们考虑抖动。

时间序列数据通过随机Gaussian噪声进行增强 ([Wikipedia](https://en.wikipedia.org/wiki/Gaussian_noise)))

![](../Images/44c18aa6fd22c7037051e5a7cefa7519.png)

在tsgm中，Gaussian噪声增强可以如下应用：

```py
aug_model = tsgm.models.augmentations.GaussianNoise()
samples = aug_model.generate(X=X, n_samples=10, variance=0.2)
```

Gaussian噪声增强的理念是，向时间序列中添加少量的抖动可能不会显著改变它，但会增加数据集中这种噪声样本的数量。这通常使下游模型对噪声样本更具鲁棒性或提高预测性能。

Gaussian噪声的超参数及其添加方式（例如，Gaussian噪声可能会在时间序列的末尾增加）是一个困难的问题，并且取决于特定的数据集和下游问题。通常值得进行实验，看看这些参数如何影响目标模型的性能。

在这里，我们提供了原始正弦数据集样本和增强样本的可视化。

![](../Images/bc5f06309b37e786f90ecfeff2727c18.png)

原始时间序列和通过抖动生成的合成数据。

# 打乱特征

另一种时间序列增强的方法是简单地打乱特征。这种方法仅适用于特定的多变量时间序列，其中这些序列对所有或特定特征的排列是不变的。例如，它可以应用于每个特征表示来自各种传感器的相同独立测量的时间序列。

为了解释这种方法，假设有五个相同的传感器，标记为 S_1, S_2, S_3, S_4 和 S_5。为了说明，假设传感器 1 到 4 在旋转方面是可以互换的。那么，尝试在 S_1，…，S_5 传感器的旋转角度上进行特征旋转的数据增强是有意义的。

![](../Images/33ef023a670f580bf559f0e92d317013.png)

在这个例子中，有五个传感器，来自这些传感器的测量生成了五维时间序列数据。传感器 1 到 4 可以被任意旋转以生成新的合成样本（例如，1->2，2->3，3->4，4->1）。因此，通过对原始数据应用这些变换，可以生成新的合成样本。

与之前的例子类似，增强可以如下进行：

```py
aug_model = tsgm.models.augmentations.Shuffle()
samples = aug_model.generate(X=X, n_samples=3)
```

这里，我们展示了一个具有 5 个特征的时间序列样本，以及一个增强样本，类似于上面的图像。

![](../Images/5c82f025b90a71ba39c5d85fa4431094.png)

原始时间序列和通过特征洗牌生成的合成数据。

# 切片和洗牌

切片和洗牌增强 [3] 将时间序列切成片段并洗牌这些片段。对于在时间上表现出某种形式的不变性的时间序列，可以执行这种增强。例如，假设从可穿戴设备上测量的时间序列持续了几天。在这种情况下，一个好的策略是按天切割时间序列，并通过洗牌这些天来获得额外的样本。切片和洗牌增强在以下图像中可视化：

![](../Images/fb2647d33c5989842439315ba9b28271.png)

切片和洗牌示意图。

```py
aug_model = tsgm.models.augmentations.SliceAndShuffle()
samples = aug_model.generate(X=X, n_samples=10, n_segments=3)
```

让我们查看增强样本和原始样本：

![](../Images/49b868a4660f4f006921f821a774773b.png)

原始时间序列和通过切片和洗牌生成的合成数据。

# 幅度扭曲

幅度扭曲 [3] 通过将原始时间序列与立方样条曲线相乘来改变时间序列数据集中每个样本的幅度。这个过程会缩放时间序列的幅度，这在许多情况下是有益的，例如我们用正弦波 `n_knots` 数量的结点在随机幅度下分布的合成例子，其中 *σ* 由函数 `.generate` 中的参数 `sigma` 设置。

```py
aug_model = tsgm.models.augmentations.MagnitudeWarping()
samples = aug_model.generate(X=X, n_samples=10, sigma=1)
```

这里是一个原始数据和通过 `MagnitudeWarping` 生成的增强样本的例子。

![](../Images/0e3f871956ba908f9ed5e0b9726b5e43.png)

原始时间序列和通过幅度扭曲生成的合成数据。

# 窗口扭曲

在这种技术 [4] 中，时间序列数据中的选定窗口要么加速，要么减速。然后，将整个结果时间序列缩放回原始大小，以保持时间步长在原始长度。请参见下面的这种增强的例子：

![](../Images/bb3e8162230f88cfb55b95b90b7bb6e7.png)

这种增强可能是有益的，例如，在设备建模中。在这种应用中，传感器测量可以根据设备的使用方式改变变化速度。

在tsgm中，与往常一样，可以通过以下方式进行生成：

```py
aug_model = tsgm.models.augmentations.WindowWarping()
samples = aug_model.generate(X=X, n_samples=10, scales=(0.5,), window_ratio=0.5)
```

下面可以找到一个生成的时间序列示例。

![](../Images/f125afd7137a500f20d74a2099619531.png)

原始时间序列和通过窗口规整生成的合成数据。

# 动态时间规整重心平均（DTWBA）

动态时间规整重心平均（DTWBA）[2]是一种基于动态时间规整（DTW）的扩增方法。DTW是一种测量时间序列相似性的方法。其思想是“同步”这些时间序列，如下图所示。

![](../Images/6a525a2c686cff6601fbd76b6414d4b0.png)

DTW用于测量两个时间序列信号sin(x)和sin(2x)的相似性。DTW测量通过白色线条显示。此外，还可视化了交叉相似性矩阵。

关于DTW计算的更多细节可以在[https://rtavenar.github.io/blog/dtw.html](https://rtavenar.github.io/blog/dtw.html)中找到。

DTWBA的过程如下：

1\. 算法选择一个时间序列来初始化DTWBA结果。这个时间序列可以明确给出，也可以从数据集中随机选择。

2\. 对于每一个`N`个时间序列，算法计算DTW距离和路径（路径是最小化距离的映射）。

3\. 在计算所有`N`个DTW距离后，算法通过对所有找到的路径进行平均来更新DTWBA结果。

4\. 算法重复步骤（2）和（3），直到DTWBA结果收敛。

参考实现可以在[tslearn](https://github.com/tslearn-team/tslearn/blob/main/tslearn/barycenters/dba.py#L60)中找到，描述可以在[2]中找到。

在tsgm中，可以通过以下方式生成样本：

```py
aug_model = tsgm.models.augmentations.DTWBarycentricAveraging()
initial_timeseries = random.sample(range(X.shape[0]), 10)
initial_timeseries = X[initial_timeseries]
samples = aug_model.generate(X=X, n_samples=10, initial_timeseries=initial_timeseries )
```

![](../Images/5f90e46a8f02f3b1b3b87f23a54fec58.png)

原始时间序列和通过DTWBA生成的合成数据。

# 使用生成机器学习模型进行扩增

另一种扩增方法是训练一个机器学习模型在历史数据上，并训练它生成新颖的合成样本。这是一种黑箱方法，因为很难解释新样本是如何生成的。在时间序列的情况下，可以应用几种方法；特别是，tsgm拥有VAE、GANs和高斯过程。以下是使用VAE生成合成时间序列的示例：

```py
n, n_ts, n_features = 1000, 24, 5
data = tsgm.utils.gen_sine_dataset(n, n_ts, n_features)
scaler = tsgm.utils.TSFeatureWiseScaler() 
scaled_data = scaler.fit_transform(data)
```

```py
architecture = tsgm.models.zoo[“vae_conv5”](n_ts, n_features, 10)
encoder, decoder = architecture.encoder, architecture.decodervae = tsgm.models.cvae.BetaVAE(encoder, decoder)
vae.compile(optimizer=keras.optimizers.Adam())
vae.fit(scaled_data, epochs=1, batch_size=64)
samples = vae.generate(10)
```

# 结论

我们探索了几种合成时间序列生成方法。许多方法将归纳偏差引入模型，并在实际应用中非常有用。

如何选择？首先，分析你的问题是否包含不变性。它对随机噪声不变吗？对特征洗牌不变吗？

接下来，选择一组广泛的方法，并验证这些方法中的任何一种是否提高了下游问题的性能（tsgm有[下游性能指标](https://tsgm.readthedocs.io/en/latest/autoapi/tsgm/metrics/index.html#tsgm.metrics.DownstreamPerformanceMetric)）。然后，选择那些提供最大性能提升的扩增方法。

*最后但同样重要的是，我感谢 Letizia Iannucci 和 Georgy Gritsenko 对于这篇文章写作的帮助和有益讨论。除非另有说明，所有图片均由作者提供。*

这篇博客文章是 TSGM 项目的一部分，我们正在创建一个工具，通过增强和合成数据生成来提升时间序列管道。如果你觉得它有帮助，可以查看 [我们的仓库](https://github.com/AlexanderVNikitin/tsgm) 并考虑引用 [关于 TSGM 的论文](https://arxiv.org/abs/2305.11567)：

```py
@article{
  nikitin2023tsgm,
  title={TSGM: A Flexible Framework for Generative Modeling of Synthetic Time Series},
  author={Nikitin, Alexander and Iannucci, Letizia and Kaski, Samuel},
  journal={arXiv preprint arXiv:2305.11567},
  year={2023}
}
```

# 参考文献

[1] H. Sakoe 和 S. Chiba, “用于口语词汇识别的动态规划算法优化”。IEEE 声学、语音与信号处理学报, 26(1), 43–49 (1978)。

[2] F. Petitjean, A. Ketterlin & P. Gancarski. 动态时间规整的全局平均方法及其在聚类中的应用。模式识别，Elsevier，2011，第 44 卷，第 3 期，第 678–693 页。

[3] Um TT, Pfister FM, Pichler D, Endo S, Lang M, Hirche S,

Fietzek U, Kulic´ D (2017) 使用卷积神经网络对可穿戴传感器数据进行数据增强以监测帕金森病。发表于第 19 届 ACM 国际多模态交互会议论文集，第 216–220 页。

[4] Rashid, K.M. 和 Louis, J., 2019\. Window-warping: 一种用于施工设备活动识别的 IMU 数据时间序列数据增强方法。发表于 ISARC. 国际自动化与施工机器人学会国际研讨会论文集（第 36 卷，第 651–657 页）。IAARC 出版社。

[5] Nikitin, A., Iannucci, L. 和 Kaski, S., 2023\. TSGM: 一种用于合成时间序列生成建模的灵活框架。*arXiv 预印本 arXiv:2305.11567*。 [Arxiv 链接](https://arxiv.org/abs/2305.11567)。
