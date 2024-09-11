# 合成时间序列的评估

> 原文：[https://towardsdatascience.com/evaluation-of-synthetic-time-series-1b4fc4e2be39?source=collection_archive---------2-----------------------#2023-12-19](https://towardsdatascience.com/evaluation-of-synthetic-time-series-1b4fc4e2be39?source=collection_archive---------2-----------------------#2023-12-19)

## 探索用于合成时间序列评估的各种指标，并附有实际代码示例

[](https://medium.com/@an231?source=post_page-----1b4fc4e2be39--------------------------------)[![Alexander Nikitin](../Images/decc36cddc4c7a23952569e293e7d209.png)](https://medium.com/@an231?source=post_page-----1b4fc4e2be39--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b4fc4e2be39--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1b4fc4e2be39--------------------------------) [Alexander Nikitin](https://medium.com/@an231?source=post_page-----1b4fc4e2be39--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5afaa29ee25a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluation-of-synthetic-time-series-1b4fc4e2be39&user=Alexander+Nikitin&userId=5afaa29ee25a&source=post_page-5afaa29ee25a----1b4fc4e2be39---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----1b4fc4e2be39--------------------------------) ·10分钟阅读·2023年12月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1b4fc4e2be39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluation-of-synthetic-time-series-1b4fc4e2be39&user=Alexander+Nikitin&userId=5afaa29ee25a&source=-----1b4fc4e2be39---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1b4fc4e2be39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluation-of-synthetic-time-series-1b4fc4e2be39&source=-----1b4fc4e2be39---------------------bookmark_footer-----------)

*这篇博客文章可以作为一个* [*在 GitHub 上的 jupyter notebook*](https://github.com/AlexanderVNikitin/tsgm/blob/main/tutorials/evaluation.ipynb) *获得，并且是* [*TSGM，一个时间序列生成建模库*](https://github.com/AlexanderVNikitin/tsgm)*的一部分。*

今天，我们将讨论合成时间序列数据集的评估——这些数据集是人为创建的，以表示真实数据。假设有一个合成数据集D*，旨在代表真实数据集D。至关重要的是定量评估这些合成数据的好坏：D*是否很好地代表了D？这些数据是否安全？这些数据对下游问题是否有价值？在本教程中，我们将深入探讨用于定量和定性评估合成时间序列数据质量的方法。

![](../Images/e88a7f55a86866407f0c484546b07141.png)

原始和合成正弦数据的示例。

首先，让我们考虑[1]中描述的两种合成数据的可能用法：

**场景 1.** 一个组织希望雇用外部代理分析敏感数据或研究特定问题的统计方法。由于隐私或商业考虑，分享真实数据可能会很复杂。合成数据可以提供一个方便的解决方案。

**场景 2.** 一个组织希望在一个相对较小的数据集上训练模型。然而，这个数据集不足以满足所需的建模质量。此类有限的数据集可以通过合成数据来增强。这些合成数据必须与真实数据相似，旨在提升模型的性能，或在其他情况下，协助进行模型可靠性测试。

总体而言，我们在本教程中指出并描述了以下度量：

1.  真实数据相似性 (场景 1 和 2)，

+   距离度量

+   判别度量，

+   最大均值差异分数

2\. 预测一致性 (场景 1)，

3\. 下游有效性 (场景 2)，

4\. 隐私 (场景 1)，

5\. 多样性 (场景 1 和场景 2)，

6\. 公平性 (场景 1 和场景 2)，

7\. 视觉比较 (场景 1 和 2)。

在TSGM中，所有度量都整齐地组织在`tsgm.metrics`中。深入了解详细信息，请参阅[我们的综合文档](https://tsgm.readthedocs.io/en/latest/modules/root.html#metrics)。

现在，让我们通过安装tsgm来启动编码示例：

```py
pip install tsgm
```

**生成合成数据。** 接下来，我们导入tsgm，并加载一个示例数据集。一个张量`Xr`现在将包含100个正弦时间序列或常数时间序列（基于目标类`yr`）。我们将使用`(Xr, yr)`作为**真实**（= 历史 = 原始）数据集。`Xs`包含由变分自编码器生成的合成数据。*（注意：为了演示我们只使用一个时期；在实际应用中增加时期数并检查训练收敛性。）*

```py
import numpy as np
import functools
import sklearn
import tensorflow as tf
from tensorflow import keras

import tsgm

n, n_ts, n_features = 100, 100, 20
vae_latent_dim = 8

# Load data that will be used as real
Xr, yr = tsgm.utils.gen_sine_vs_const_dataset(n, n_ts, n_features, max_value=2, const=1)
Xr = Xr.astype(np.float32)
yr = keras.utils.to_categorical(yr).astype(np.float32)
ys = yr  # use real labels as synthetic labels

# Using real data generate synthetic time series dataset
scaler = tsgm.utils.TSFeatureWiseScaler()        
scaled_data = scaler.fit_transform(Xr)
architecture = tsgm.models.zoo["cvae_conv5"](n_ts, n_features, vae_latent_dim)
encoder, decoder = architecture.encoder, architecture.decoder
vae = tsgm.models.cvae.cBetaVAE(encoder, decoder, latent_dim=vae_latent_dim, temporal=False)
vae.compile(optimizer=keras.optimizers.Adam())

# Train VAE using historical data
vae.fit(scaled_data, yr, epochs=1, batch_size=64)
Xs, ys = vae.generate(ys)

d_real = tsgm.dataset.Dataset(Xr, yr)
d_syn = tsgm.dataset.Dataset(Xs, ys)
```

# 与真实数据的相似性

## 距离度量

一开始，测量真实数据与合成数据之间的相似性是方便的。一种方法是计算合成数据与真实数据的汇总统计量向量之间的距离。

![](../Images/8ebdb2e72519e7a014038e2fc4b3e2be.png)

距离越小，合成数据与实际数据的现实性越接近。现在，让我们定义一组统计数据，这些统计数据将作为我们距离度量的基础。方法`tsgm.metrics.statistics.axis_*_s`计算提供的轴上的统计数据`*`。

```py
statistics = [
    functools.partial(tsgm.metrics.statistics.axis_max_s, axis=None),
    functools.partial(tsgm.metrics.statistics.axis_min_s, axis=None),
    functools.partial(tsgm.metrics.statistics.axis_max_s, axis=1),
    functools.partial(tsgm.metrics.statistics.axis_min_s, axis=1)]
```

接下来，我们建立距离度量。为了简化，我们将选择欧几里得范数。

```py
discrepancy_func = lambda x, y: np.linalg.norm(x - y)
```

综合起来，我们将利用`tsgm.metrics.DistanceMetric`对象。

```py
dist_metric = tsgm.metrics.DistanceMetric(
    statistics=statistics, discrepancy=discrepancy_func
)
print(dist_metric(d_real, d_syn))
```

## MMD度量

另一种方法涉及比较合成数据和真实数据的分布。在这种情况下，使用最大均值差异（MMD）[3]被证明是方便的。MMD作为一个非参数的两样本测试来确定样本是否来自相同的分布。通过经验观察，我们发现MMD度量是评估真实数据相似性的特别方便的方法。

```py
mmd_metric = tsgm.metrics.MMDMetric()
print(mmd_metric(Xr, Xs))
```

## 区分度量

在这种方法中，模型被训练以区分真实数据和合成数据。在TSGM中，`tsgm.metrics.DiscriminativeMetric`被证明是一个有价值的工具。该度量帮助评估模型在区分真实数据集和合成数据集方面的有效性，为数据相似性提供了额外的视角。

```py
# use LSTM classification model from TSGM zoo.
model = tsgm.models.zoo["clf_cl_n"](
    seq_len=Xr.shape[1], feat_dim=Xr.shape[2], output_dim=1).model
model.compile(
    tf.keras.optimizers.Adam(),
    tf.keras.losses.CategoricalCrossentropy(from_logits=False)
)

# use TSGM metric to measure the score
discr_metric = tsgm.metrics.DiscriminativeMetric()
print(
    discr_metric(
        d_hist=Xr, d_syn=Xs, model=model,
        test_size=0.2, random_seed=42, n_epochs=10
    )
)
```

## 一致性度量

接下来，我们讨论一致性度量。这个想法与上面写的**情境 1**相一致。在这里，我们关注于评估一组下游模型的一致性。更具体地说，我们考虑一组模型 ℳ 和一个评估者 E: ℳ × 𝒟 → ℝ。

为了评估ℳ在D和D*上的一致性，我们测量p(m₁ ∼ m₂| m₁, m₂ ∈ ℳ, D, D*)，其中m₁ ∼ m₂表示m₁与m₂一致：“如果m₁在D上优于m₂，则在D*上也优于m₂，反之亦然。” 估计这个概率涉及固定一个有限的集合ℳ，并使用真实数据评估模型，同时使用合成数据分别评估模型。

在TSGM中，我们的第一步是定义一组评估者。为此，我们将利用一系列LSTM模型，范围从一个到三个LSTM块。

```py
class EvaluatorConvLSTM():
    '''
    NB an oversimplified classifier, for educational purposes only.
    '''

    def __init__(self, model):
        self._model = model

    def evaluate(self, D: tsgm.dataset.Dataset, D_test: tsgm.dataset.Dataset) -> float:
        X_train, y_train = D.Xy
        X_test, y_test = D_test.Xy

        self._model.fit(X_train, y_train)

        y_pred = np.argmax(self._model.predict(X_test), 1)
        print(self._model.predict(X_test).shape)
        y_test = np.argmax(y_test, 1)
        return sklearn.metrics.accuracy_score(y_pred, y_test)

# Define a set of models
seq_len, feat_dim, n_classes = *Xr.shape[1:], 2
models = [tsgm.models.zoo["clf_cl_n"](seq_len, feat_dim, n_classes, n_conv_lstm_blocks=i) for i in range(1, 4)]
for m in models:
    m.model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
evaluators = [EvaluatorConvLSTM(m.model) for m in models]

# Utilize the set of evaluators with ConsistencyMetric from tsgm
consistency_metric = tsgm.metrics.ConsistencyMetric(evaluators=evaluators)
print(consistency_metric(d_real, d_syn, d_real))
```

# 下游性能

现在，让我们探索生成的数据如何有助于改善特定下游问题中的预测性能。我们将考虑两种不同的方法来评估下游性能：

1\. **用合成数据增强真实数据。**

当数据有限时，这种方法非常有价值。通过用生成的数据补充真实数据，我们旨在增强训练集以提高模型性能。请参见我们关于数据增强的[博客文章](https://medium.com/@an231/time-series-augmentations-16237134b29b) [2]。

2\. **仅利用生成的数据进行下游模型训练。**

在真实数据稀缺和隐私的情况下，这种方法会派上用场。在这种情况下，下游模型仅在生成的数据上进行训练，并随后在真实数据上进行评估。

```py
downstream_model = tsgm.models.zoo["clf_cl_n"](seq_len, feat_dim, n_classes, n_conv_lstm_blocks=1).model
downstream_model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

evaluator = EvaluatorConvLSTM(downstream_model)

downstream_perf_metric = tsgm.metrics.DownstreamPerformanceMetric(evaluator)
print(downstream_perf_metric(d_real, d_syn, d_real))
```

结果表示通过合成数据增强与仅使用训练数据训练的模型相比，准确性的提升。

# 隐私：成员推断攻击指标

![](../Images/cd059109e90b1369ad28b0dea525824e.png)

成员推断攻击可视化。

测量合成数据隐私的一种方法是测量对成员推断攻击的敏感性。成员推断攻击过程在上图中进行了可视化。思路如下。设想一个攻击者拥有合成数据和一个特定的数据样本（该样本可能存在于原始数据集中，也可能不存在）。目标是**推断**这个样本是否存在于真实数据中。

`tsgm.metrics.PrivacyMembershipInferenceMetric` 通过合成数据衡量对成员推断攻击的敏感性。步骤评估过程如下：

1\. **数据分割**。将历史数据分割为训练集和保留集（记作 Dₜ 和 Dₕ），

2\. **生成模型训练。** 在 Dₜ 上训练生成模型，并生成一个合成数据集 D*，

3\. **一类分类（OCC）模型训练。** 在合成数据 D* 上训练一类分类（OCC）模型，并在 Dₜ 和 Dₕ 上评估该模型，

4\. **目标分数计算。** 使用 OCC 模型的精度的补数作为目标分数。

该评估过程提供了对利用合成数据进行成员推断攻击潜在脆弱性的洞察。

现在，让我们引入一个攻击模型。为了演示，我们将定义一个一类 SVM 分类器。

```py
class FlattenTSOneClassSVM:
    def __init__(self, clf):
        self._clf = clf

    def fit(self, X):
        X_fl = X.reshape(X.shape[0], -1)
        self._clf.fit(X_fl)

    def predict(self, X):
        X_fl = X.reshape(X.shape[0], -1)
        return self._clf.predict(X_fl)

attacker = FlattenTSOneClassSVM(sklearn.svm.OneClassSVM())
privacy_metric = tsgm.metrics.PrivacyMembershipInferenceMetric(
    attacker=attacker
)
```

现在，让我们定义一个测试集并测量隐私指标：

```py
X_test, y_test = tsgm.utils.gen_sine_vs_const_dataset(10, 100, 20, max_value=2, const=1)
d_test = tsgm.dataset.Dataset(X_test, keras.utils.to_categorical(y_test))

# 1 indicates high privacy and 0 -- low privacy.
privacy_metric(d_real, d_syn, d_test)
```

# 多样性

通过该指标，我们的目标是量化合成数据的多样性。请考虑下面的图像，其中红色点表示真实数据，蓝色点表示合成数据。哪个选项产生了更好的合成数据集？右侧的选项看起来更有利，但为什么？答案在于其多样性，使其可能更具通用性和实用性。然而，仅有多样性是不够的；必须同时考虑其他指标，如距离或下游性能。在我们的探索中，我们将使用熵来说明这一概念。

![](../Images/d01738606e2f68c6cb465c09e203d557.png)

2D中的合成数据。A: 非多样化的合成数据；B: 多样化的合成数据。

```py
spec_entropy = tsgm.metrics.EntropyMetric()
print(spec_entropy(Xr))
print(spec_entropy(Xs))
```

# 公平性

公平性与合成时间序列生成在两个重要方面交叉。首先，评估合成数据是否引入了新的偏差至关重要。其次，合成数据提供了减少原始数据固有偏差的机会。定义标准化的公平性检查程序颇具挑战性，因为这通常取决于下游问题的具体情况。一些衡量公平性的示例指标包括人口平等、预测率平等范式以及机会平等。

以机会平等为例。机会平等作为一种公平度量标准，用于衡量对于一个优先标签（赋予个人优势或利益的标签）和给定属性，分类器是否对该属性的所有值都同样准确地预测该优先标签[6]。这一度量标准对于确保在不同属性值之间的公正性和公平对待至关重要。[6]中提供了一个很好的例子：“假设Glubbdubdrib大学招收了利利普特人和布罗卜丁纳根人进入一个严格的数学课程。利利普特人的中学提供了强大的数学课程，绝大多数学生都符合大学课程的资格。布罗卜丁纳根人的中学根本没有提供数学课程，因此，符合资格的学生远少于利利普特人。如果符合资格的学生，无论是利利普特人还是布罗卜丁纳根人，都同样可能被录取，那么机会平等就满足了‘被录取’这一优先标签的要求。”

# 定性分析

为了定性地评估数据，方便：

a. 从合成和真实数据中绘制样本并可视化单个样本，

b. 构建生成样本的嵌入并使用例如TSNE进行可视化。让我们用TSGM来示例（b）：

```py
tsgm.utils.visualize_tsne_unlabeled(Xr, Xs, perplexity=10, markersize=20, alpha=0.5)
```

![](../Images/65a9696637a67f6d3a280ba9ed8028dc.png)

原始和合成时间序列数据的TSNE可视化。

# 引用

本博客文章是TSGM项目的一部分，我们正在创建一个通过增强和合成数据生成来改进时间序列管道的工具。如果您觉得这篇文章有帮助，请查看我们的代码库，并考虑引用关于TSGM的论文：

```py
@article{
  nikitin2023tsgm,
  title={TSGM: A Flexible Framework for Generative Modeling of Synthetic Time Series},
  author={Nikitin, Alexander and Iannucci, Letizia and Kaski, Samuel},
  journal={arXiv preprint arXiv:2305.11567},
  year={2023}
}
```

# 结论

总之，我们探讨了合成时间序列数据的各种评估技术，提供了不同场景的全面概述。为了有效地导航这些方法，考虑描述的场景是有益的。*最终*，选择正确的度量标准取决于下游问题、应用领域和数据使用的法律法规。提供的多种度量标准旨在帮助构建针对特定问题的全面评估管道。

# 参考文献：

[1] Nikitin, A., Iannucci, L. and Kaski, S., 2023\. TSGM: A Flexible Framework for Generative Modeling of Synthetic Time Series. arXiv预印本 arXiv:2305.11567\. [Arxiv Link](https://arxiv.org/pdf/2305.11567.pdf)。

[2] 时间序列增强，TowardsDataScience文章， [https://medium.com/towards-data-science/time-series-augmentations-16237134b29b](https://medium.com/towards-data-science/time-series-augmentations-16237134b29b)。

[3] Gretton, A., Borgwardt, K.M., Rasch, M.J., Schölkopf, B. 和 Smola, A., 2012\. 核两样本检验。机器学习研究期刊, 13(1), 页码 723–773\. [JMLR 链接](https://jmlr.csail.mit.edu/papers/v13/gretton12a.html)。

[4] Wen, Q., Sun, L., Yang, F., Song, X., Gao, J., Wang, X. 和 Xu, H., 2020\. 深度学习中的时间序列数据增强：一项综述。arXiv 预印本 arXiv:2002.12478\. [Arxiv 链接](https://arxiv.org/pdf/2002.12478.pdf)。

[5] Wattenberg, M., Viégas, F. 和 Hardt, M., 2016\. 通过更智能的机器学习对抗歧视。Google Research, 17\. [Google Research 链接](http://research.google.com/bigpicture/attacking-discrimination-in-ml/)。

[6] 机器学习词汇表：公平性。Google Developers Blog。

[Google Developers Blog 链接](https://developers.google.com/machine-learning/glossary/fairness)。

*除非另有说明，所有图片均由作者提供。有关合成时间序列生成的额外材料，请参见* [*TSGM 在 GitHub 上*](https://github.com/alexandervnikitin/tsgm)*，并且* [*订阅 Medium 帖子*](https://an231.medium.com/subscribe)*。*
