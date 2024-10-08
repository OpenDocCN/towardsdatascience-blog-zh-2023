# 合成数据能提升机器学习性能吗？

> 原文：[`towardsdatascience.com/can-synthetic-data-boost-machine-learning-performance-6b4041e75dda`](https://towardsdatascience.com/can-synthetic-data-boost-machine-learning-performance-6b4041e75dda)

## 研究合成数据在不平衡数据集上提高模型性能的能力

[](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----6b4041e75dda--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b4041e75dda--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 5 日

--

![](img/71bd354c415afacf5396ba2c0956b4fc.png)

图片由作者提供：由 Midjourney 生成

# 背景——不平衡数据集

在商业机器学习应用中，数据不平衡分类问题经常发生。你可能会在客户流失预测、欺诈检测、医疗诊断或垃圾邮件检测中遇到它们。在所有这些情况下，我们要检测的对象都属于少数类，而这些少数类在数据中可能严重不足。为提高模型在不平衡数据集上的表现，提出了几种方法：

+   **欠采样**：通过随机欠采样多数类来实现更平衡的训练数据集。

+   **过采样**：通过随机过采样少数类来获得平衡的训练数据集。

+   **加权损失**：根据少数类为损失函数分配权重。

+   **合成数据**：使用生成式 AI 创建高保真度的少数类合成数据样本。

在这篇文章中，我展示了如何通过在合成数据上训练模型来超越其他方法，从而提高分类器的性能。

# 数据集

数据来自[Kaggle](https://www.kaggle.com/)，包括 284,807 笔信用卡交易，其中 492 笔（0.172%）被标记为欺诈交易。数据可用于商业和非商业用途，采用[开放数据公共许可证](https://opendatacommons.org/licenses/dbcl/1-0/)。

对感兴趣的读者，Kaggle 提供了有关数据的更多详细信息和基本[描述性统计](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)。

从这个 Kaggle 数据集中，我创建了两个子集：一个训练集和一个持出集。训练集包含总数据的 80%，以及在探索该方法时生成的合成样本。持出集则包含原始数据的 20%，不包括任何合成样本。

![](img/bd244d3062156d0eec2447cd9c9fc53b.png)

作者提供的图像：数据拆分过程

# 模型

我使用了[Ludwig](https://ludwig.ai/latest/)，这是一个开源的声明式框架，用于构建深度学习模型，因为它易于实现。通过在 yaml 文件中声明模型并通过 Ludwig 的 Python API 运行训练任务，可以轻松构建和训练模型。我之前写过一篇[文章](https://medium.com/towards-data-science/ludwig-a-friendlier-deep-learning-framework-946ee3d3b24)，详细介绍了 Ludwig，供感兴趣的读者参考。

对于每种方法，我使用相同的基线模型，仅根据需要调整特定参数。例如，Ludwig 原生支持权重和采样调整——这些可以简单地在 yaml 文件中进行调整。我提供了每种方法的模型配置 yaml 文件的链接，供您探索。

+   基线模型 — [链接](https://github.com/john-adeojo/Credit-Card-Fraud-Model-Registry/blob/main/model%20yaml%20files/synthetic_data/baseline_model.yaml)

+   加权损失模型 — [链接](https://github.com/john-adeojo/Credit-Card-Fraud-Model-Registry/blob/main/model%20yaml%20files/synthetic_data/model_weighted_loss.yaml)

+   欠采样模型 — [链接](https://github.com/john-adeojo/Credit-Card-Fraud-Model-Registry/blob/main/model%20yaml%20files/synthetic_data/model_undersample.yaml)

+   过采样模型 — [链接](https://github.com/john-adeojo/Credit-Card-Fraud-Model-Registry/blob/main/model%20yaml%20files/synthetic_data/model_oversample.yaml)

+   合成数据 — 使用与基线相同的模型，因为类别是平衡的。

# 生成合成数据

我使用了合成数据库（SDV），这是一个用于生成合成数据样本的开源库。使用 SDV，我生成了额外的 284k 合成欺诈样本，从而在训练数据集中实现了两个类别的均等表示。

合成样本是通过适用于表格数据的变分自编码器（TVAE）生成的。有关 TVAE 背后的理论，您可以在这篇[论文](https://arxiv.org/pdf/1907.00503.pdf)中找到更多细节。

[SDV](https://docs.sdv.dev/sdv/)提供了诊断统计数据，显示拟合质量的指示。您可以通过比较真实数据与生成数据中的变量分布，手动探索拟合质量，示例如下。

![](img/54121d9ed8930d1145cb1c2b16a0531e.png)

作者提供的图像：真实与合成的变量 v1 分布对比

![](img/046f7c574ca531d64ea1b8e845ce76e9.png)

作者提供的图像：真实与合成的变量 v10 分布对比

![](img/e0aa806a65638bbdd960eba187b4db50.png)

作者提供的图像：真实与合成的变量分布对比

# 使用精确度召回图表评估性能

我们通过绘制模型与持出数据集的精确度与召回率曲线来评估每个模型的性能。

## 精确度-召回率曲线

精确度-召回率曲线，即将精确度（在 y 轴上）与召回率（在 x 轴上）进行绘制的图，与 [ROC 曲线](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc#:~:text=An%20ROC%20curve%20(receiver%20operating,False%20Positive%20Rate) )类似。它作为一种强健的诊断工具，用于评估模型在显著类别不平衡场景中的性能，例如我们的信用卡欺诈检测用例，便是一个典型例子。

图表的右上角代表“理想”点 —— 假阳性率为零，真正阳性率为一。一个熟练的模型应该能够达到或接近这一点，这意味着曲线下面积（AUC-PR）较大的模型可能更优越。

## 无技能预测器

“无技能”预测器是一个简单的模型，其预测是随机的。对于不平衡数据集，无技能线是一个高度等于正类比例的水平线。这是因为如果模型随机预测正类，精确度将等于数据集中正实例的比例。

## 模型性能 — 基线

基线模型是没有样本调整、损失函数调整或增强训练数据的深度神经网络。每种方法与基线性能进行比较，基线性能作为性能基准。

![](img/389d22765e6bb2466c6609912444ae32.png)

作者提供的图像：基线模型的精确度-召回率曲线

## 模型性能 — 加权损失方法

加权损失根据欺诈交易与非欺诈交易的比例调整损失函数。

![](img/ad6118e9e5c3b6e863d4a4710ee78b60.png)

作者提供的图像：加权损失方法的精确度-召回率曲线

## 模型性能 — 过采样方法

过采样随机地过度采样欺诈交易，直到训练数据集中各类别之间的表示均等。

![](img/bc225c1fbad884c19136df4a0516504d.png)

作者提供的图像：过采样方法的精确度-召回率曲线

## 模型性能 — 欠采样方法

欠采样随机地欠采样非欺诈交易，直到训练数据集中各类别之间的表示均等。

![](img/bf43d3a05070ce9ad42512b9c49c2969.png)

作者提供的图像：欠采样方法的精确度-召回率曲线

## 模型性能 — 人工合成数据方法

利用 TVAEs 生成 284k 人工合成的欺诈样本，以在训练数据集中获得各类别的均等表示。

![](img/0b46183b2f1de8ae3ae1ba925d718749.png)

作者提供的图像：人工合成数据方法的精确度-召回率曲线

# 自助法持出数据集

为了获得对保留集性能的稳健视角，我从原始数据中创建了五十个自举保留集。对每种方法关联的模型在所有集上运行，提供了性能分布。然后，我们可以使用 Kolmogorov-Smirnov 检验来确定每种方法是否与基线存在统计显著差异。

**加权**：加权方法在召回率和 AUC 方面相对于基线表现略逊。除此之外，各性能指标的方差相对于其他方法显得较高。

![](img/2cbf98db34757c7b0777fb03de7f26d7.png)

作者提供的图像：模型性能指标在 50 个自举保留样本上的表现。基线与加权损失，KS 统计 — AUC 0.420 p 值 < 0.000，精度 0.260 p 值 0.068，召回率 0.520 p 值 < 0.000

**过采样**：过采样方法提高了模型的召回率，但导致精度的急剧恶化。

![](img/da5bc03287b1607c6f2a95ae98c8b772.png)

作者提供的图像：模型性能指标在 50 个自举保留样本上的表现。基线与过采样，KS 统计 — AUC 0.160 p 值 0.549，精度 1.0 p 值 < 0.000，召回率 0.9 p 值 < 0.000

**欠采样**：该方法在所有指标上表现都不如基线。

![](img/9d0481f0207a1e82b2260a9f6cd37405.png)

作者提供的图像：模型性能指标在 50 个自举保留样本上的表现。基线与过采样，KS 统计 — AUC 0.880 p 值 < 0.000，精度 0.6 p 值 < 0.000，召回率 1.0 p 值 < 0.000

**合成**：合成方法提升了模型的召回率，尽管以牺牲精度为代价。尽管精度的影响仍然显著，但与过采样方法相比，合成方法提供了更具韧性的替代方案，能够在不显著影响精度的情况下提升模型召回率。合成方法的稳健性在 AUC-PR 的提升中得到了进一步证明。

![](img/aa624f4f7cd1058e4bd764e3c2e7b950.png)

作者提供的图像：模型性能指标在 50 个自举保留样本上的表现。基线与合成，KS 统计 — AUC 0.620，精度 0.560，召回率 0.360 所有 p 值 ≤ 0.003

# 结论

我们注意到，相对于基线，合成数据方法可以提升模型的召回率，但以牺牲精度为代价。过采样也能实现类似的结果，但模型精度相比之下急剧下降。

在我们特定的信用卡欺诈检测背景下，假阳性不像假阴性那样昂贵。因此，如果提高召回率能够显著提高，我们可以在模型精度上做出一定妥协。通过合成实例丰富我们的训练数据似乎是提高召回率同时减轻精度不良影响的有效策略。这种增强可能会显著影响盈利能力，特别是在将模型扩展到处理数百万笔交易时。最终，将假阳性和假阴性的确切成本进行归因，将使我们更清楚地理解最具商业可行性的方法，这一话题超出了本文的范围。

检查不同样本规模的合成数据的表现将非常有趣，也许可以与加权损失结合起来。类似地，尝试不同的过采样比例可能会产生与我们观察到的合成方法类似的效果。

这个项目的笔记本可以在我的 [GitHub repo](https://github.com/john-adeojo/synthetic_data_credit_cards/blob/main/notebook/Synthetic%20Data%20Experiment.ipynb) 中找到

*在* [*LinkedIn*](https://www.linkedin.com/in/john-adeojo/) *上关注我*

*订阅 Medium 以获取更多来自我的见解：*

[](https://johnadeojo.medium.com/membership?source=post_page-----6b4041e75dda--------------------------------) [## 使用我的推荐链接加入 Medium — John Adeojo

### 我分享数据科学项目、经验和专业知识，以帮助你在旅程中。你可以通过…

johnadeojo.medium.com](https://johnadeojo.medium.com/membership?source=post_page-----6b4041e75dda--------------------------------)

*如果你有兴趣将 AI 或数据科学整合到你的业务操作中，我们邀请你预约与我们进行免费的初步咨询：*

[](https://www.data-centric-solutions.com/book-online?source=post_page-----6b4041e75dda--------------------------------) [## 在线预约 | 数据驱动解决方案

### 通过免费咨询发现我们在帮助企业实现雄心勃勃目标方面的专业知识。我们的数据科学家和…

www.data-centric-solutions.com](https://www.data-centric-solutions.com/book-online?source=post_page-----6b4041e75dda--------------------------------)
