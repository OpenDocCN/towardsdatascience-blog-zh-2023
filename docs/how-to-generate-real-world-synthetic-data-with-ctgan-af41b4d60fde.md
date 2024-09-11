# 如何使用CTGAN生成真实世界的合成数据

> 原文：[https://towardsdatascience.com/how-to-generate-real-world-synthetic-data-with-ctgan-af41b4d60fde?source=collection_archive---------2-----------------------#2023-04-13](https://towardsdatascience.com/how-to-generate-real-world-synthetic-data-with-ctgan-af41b4d60fde?source=collection_archive---------2-----------------------#2023-04-13)

## 合成数据生成

## 探索ydata-synthetic中介绍的Streamlit应用

[](https://medium.com/@miriam.santos?source=post_page-----af41b4d60fde--------------------------------)[![Miriam Santos](../Images/decbc6528a641e7b02934a03e136284a.png)](https://medium.com/@miriam.santos?source=post_page-----af41b4d60fde--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af41b4d60fde--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----af41b4d60fde--------------------------------) [Miriam Santos](https://medium.com/@miriam.santos?source=post_page-----af41b4d60fde--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F243289394aaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-real-world-synthetic-data-with-ctgan-af41b4d60fde&user=Miriam+Santos&userId=243289394aaa&source=post_page-243289394aaa----af41b4d60fde---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af41b4d60fde--------------------------------) ·10分钟阅读·2023年4月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf41b4d60fde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-real-world-synthetic-data-with-ctgan-af41b4d60fde&user=Miriam+Santos&userId=243289394aaa&source=-----af41b4d60fde---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf41b4d60fde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-real-world-synthetic-data-with-ctgan-af41b4d60fde&source=-----af41b4d60fde---------------------bookmark_footer-----------)

G*生成合成数据正日益成为掌握的基本任务，因为我们正朝着数据中心的AI开发范式迈进。*

[合成数据](https://medium.com/ydata-ai/synthetic-data-1cd0ba907609)确实具有巨大的潜力，但它也带来了自身的挑战，特别是完全捕捉真实世界领域的复杂性和微妙之处，尤其是其异质性。

![](../Images/a596a47e4d2bddb9612433846d953718.png)

数据异质性在合成数据生成模型中处理起来非常困难，特别是对于包含额外（复杂！）数据特征和难度因素的现实世界领域。照片由[Tolga Ulkan](https://unsplash.com/@tolga__?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供。

现实世界的领域与[众多复杂性方面](/data-quality-issues-that-kill-your-machine-learning-models-961591340b40)相关（例如，缺失数据、不平衡数据、噪声数据），然而**最常见的情况之一是涵盖异质（或“混合”）数据，** 即包含数值型和类别型特征的数据。

由于每种特征类型可能具有其固有的特征，**异质数据给合成数据生成过程带来了额外的挑战**。

[CTGAN](https://arxiv.org/abs/1907.00503)（条件表格生成对抗网络）旨在部分“捕捉”这种现实世界数据的异质性，与[WGAN 和 WGAN-GP](https://medium.com/ydata-ai/generating-synthetic-tabular-data-with-gans-part-1-866705a77302)等其他架构相比，已证明在各种数据集上更为稳健和通用。

*在本文中，我们将剖析使该架构在表格数据中如此不同和高效的特性，并说明何时以及为什么应该利用它。*

# 现实世界的表格异质数据

**现实世界的领域通常由我们所称的“表格数据”来描述，** 即可以结构化和组织成类似表格格式的数据。

作为一种标准，特征（有时称为“变量”或“属性”）在*列*中表示，而观察值（或“记录”）对应于*行*。

**此外，现实世界的数据通常包含数值型和类别型特征。**

**数值型**特征（也称为“连续型”）是编码定量值的特征，而**类别型**（也称为“离散型”）表示定性测量。

这是一个[成人收入普查](https://www.kaggle.com/datasets/uciml/adult-census-income?resource=download)数据集的示例（在[Kaggle](https://www.kaggle.com)上以[CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/)许可证提供），我们将在后续中探讨：`age` 和 `fnlwgt` 是**数值型**特征，而其余的都是**类别型**特征。

![](../Images/d47e13ee9a35e8eda7a821bd7492bce4.png)

一个简单的表格异质数据集示例，包含数值型和类别型特征。图片由作者提供。

**由于每种特征类型的性质，处理异质数据在开发机器学习模型时并不简单。**

根据我们需要训练的算法的内部工作原理，输入数据需要以不同方式表示或预处理，以便模型能够正确学习它们的特征。

**在生成合成数据时，这点显得尤为重要。** 我们不仅关心数据的预处理，以便模型可以高效地使用数据，**我们还关心模型是否能高效地学习真实数据的特征**，以便能够输出保留这些特性的合成数据。

# 为什么选择 CTGAN 处理异构表格数据？

自原始 [GAN](https://arxiv.org/abs/1406.2661) 公式化以来，研究者们提出了对原始架构的修改、新的损失函数或优化策略，以解决特定的 GAN 限制。

例如，某些架构如 [WGAN](https://arxiv.org/abs/1701.07875) 和 [WGAN-GP](https://arxiv.org/abs/1704.00028) 在训练稳定性和收敛时间方面对 GAN 做出了显著改进。而 [PacGAN](https://arxiv.org/abs/1712.04086) 则旨在缓解模式崩溃，这是传统 GAN 架构的另一个常见缺陷。

**然而，在数据异质性（即处理数值特征和类别特征及其内在特性）方面，这些架构仍然显得不足。**

尽管它们在数值特征上表现良好，但在捕捉类别特征的分布时遇到了困难，而类别特征在大多数现实世界数据集中是普遍存在的。

确实，这些架构没有被概念化为处理包含混合特征类型的数据——既有数值型 *也有* 类别型。

**相反，CTGAN 是专门设计来应对表格数据挑战的，处理混合数据。**

基于其他架构（如 WGAN-GP 和 PacGAN）取得的成功，**CTGAN 进一步考虑了将合成数据生成作为一个完整的流程——从数据准备到 GAN 架构本身。** 换句话说，CTGAN 关注数值特征和类别特征的具体特征，并将这些特征融入生成模型中。*如何做到的？*

## 数值特征：非高斯和多模态分布

**CTGAN 引入了模式特定归一化**

与图像数据中像素值通常遵循高斯分布不同，**表格数据中的连续特征往往是非高斯的。**

此外，它们**往往遵循多模态分布**，即概率分布具有多个模式，即它们呈现出不同的局部极大值（或“峰值”）：

![](../Images/c3fa694d297e1a4fb73e11d468362f5f.png)

高斯型与偏斜数据分布的例子（图 a 和 b）。多模态分布分解为具有不同模式的分布的例子（图 c 和 d）。图片由作者提供。

为了捕捉这些行为，CTGAN 使用 **模式特定归一化**。通过使用 VGM（变分高斯混合）模型，连续特征中的每个值由一个表示其采样模式的独热向量和一个表示根据该模式归一化的值的标量组成：

![](../Images/b83ba6bee60f36edfd5e8f095c0345e4.png)

模式特定归一化的示例。ci,j 表示特征 j（例如，j = “年龄”）中的一个值 i，p3 被选择。因此，ci,j 由一个向量 [ai,j, 0, 0, 1] 表示。n3 和 p3 分别代表 p3 的模式和标准差。图片来自 [1]。

## 分类特征：稀疏的独热编码向量和高类别失衡

**CTGAN 引入了条件生成器**

CTGAN 旨在解决由分类特征引入的主要两个挑战：

+   **一个是现实数据中独热编码向量的稀疏性。** 虽然生成器输出所有可能分类值的概率分布，但原始的“真实”分类值直接编码在独热向量中。通过比较真实和合成数据之间的分布稀疏性，鉴别器可以很容易区分这些数据。

+   **另一个问题是某些分类特征的失衡。** 如果某些类别的特征被低估，生成器无法充分学习这些特征。如果我们关心的是预测建模或分类任务，数据过采样可能是缓解这个问题的解决方案。然而，由于合成数据生成的目标是模拟原始数据的属性，这不是一个选项。

**CTGAN 引入了条件生成器** 以应对由失衡类别带来的挑战，这通常会导致 GAN 臭名昭著的模式崩溃。然而，条件架构没有免费的午餐：**输入需要被准备好** 以便生成器可以解释这些条件，**生成的行需要保留输入条件**。

为此，CTGAN 考虑了一个 **条件向量**，在逐样本训练中使用时，CTGAN 的使用会有很大不同：

![](../Images/731878c5e5a23c8bf49f5553f37bed42.png)

CTGAN 模型概述。通过采样训练，示例根据分类特征的可能值进行条件化，并根据其对数频率进行采样。图片来自 [1]。

# 使用 CTGAN 生成合成表格数据

**获取合成数据的最简单方法之一是探索作为开源软件散布在 GitHub 上的模型。** 你可以尝试的工具很多：*查看* [*awesome-data-centric-ai*](https://github.com/Data-Centric-AI-Community/awesome-data-centric-ai#-synthetic-data) *库，获取开源工具的精选列表！*

**在学习和尝试新库时，我倾向于选择简单直观的体验：如果有UI，那就更好。**

对于合成数据生成，`ydata-synthetic`最近推出了一个Streamlit应用程序，让我们能够从数据读取到分析新生成的合成数据进行完整的流程。*太完美了！*

![](../Images/eaee4d32a4fb7f4a3136faa9606300d6.png)

ydata-synthetic Streamlit应用程序：欢迎屏幕。作者提供的图像。

让UI运行的第一步是安装`ydata-synthetic`。不要忘记添加“streamlit”额外组件：

```py
pip install "ydata-syntehtic[streamlit]==1.0.1"
```

然后，您可以打开一个Python文件并运行：

```py
from ydata_synthetic import streamlit_app

streamlit_app.run()
```

运行上述命令后，控制台将输出一个URL，您可以通过该URL访问应用程序！

## 训练合成器模型

训练合成器很简单：您可以访问“**训练合成器**”标签并上传一个文件（再次说明，我使用的是成人普查收入数据集）：

![](../Images/9390e1b69b3494f4bcd8c71103e81b52.png)

ydata-synthetic Streamlit应用程序：上传文件。作者的录屏。

文件加载后，我们需要指定哪些特征是`numeric`和`categorical`：

![](../Images/01f558ca1c18e02b21013ca25bb61b07.png)

ydata-synthetic Streamlit应用程序：指定数值和分类特征。作者的录屏。

然后，我们可以选择我们的合成器参数，即我们打算使用的`model`及其参数，如`batch size`、`learning rate`和其他设置（例如`noise dimension`、`layer dimension`以及正则化常数`beta`）。

最后，我们选择训练参数，即训练`epochs`，然后点击一个按钮开始训练：

![](../Images/1a7ab9fae777680654ca7b33c7513446.png)

ydata-synthetic Streamlit应用程序：定义合成器和训练参数。作者的录屏。

请注意，我在示例中自然使用了CTGAN，但目前也支持其他模型，如GAN、WGAN、WGANGP、CRAMER和DRAGAN。

## 生成和分析合成数据样本

要生成新的合成样本，我们可以访问“**生成合成数据**”标签，选择要生成的样本数量，并指定保存文件的文件名。

我们的模型默认保存并加载为`trained_synth.pkl`，但我们可以通过提供其路径来加载之前训练的模型。

![](../Images/a54f9970c135707eefb498543aae0cae.png)

ydata-synthetic Streamlit应用程序：生成新的合成样本。作者的录屏。

此外，我决定生成数据分析报告，以检查合成数据的整体特征，因此我勾选了“**生成合成数据分析**”，然后通过点击“**生成样本**”开始合成过程：

![](../Images/44256306c01ea416694065cb71be96b4.png)

ydata-synthetic Streamlit应用程序：新的合成样本和数据分析。作者的录屏。

报告是使用熟悉的`[ydata-profiling](https://github.com/ydataai/ydata-profiling)`包生成的，合成样本现在保存在`synthetic_adult.csv`文件中。

通过探索我们新生成样本的概况报告，我们可以轻松确定**CTGAN成功地学习了原始数据的特征**，即使在像成人数据集这样复杂的异构场景中也是如此：

+   基本特征**统计数据保持一致**，适用于数值和分类特征（例如，均值/标准差、类别数量/众数）；

+   **类别特征的表示被模仿**，即原始类别的频率在合成数据中得到保持。

+   特征之间的**潜在关系**——相关性和交互——**也得到了保留**，包括原始数据质量警报（即，合成数据显示了与原始数据相同的质量警报）。

自然地，根据模型所给的具体参数，我们可以改进合成数据生成结果，使新数据尽可能接近原始数据。

*就这样！几步完成，轻松无忧！*

# 限制与开放挑战

尽管CTGAN已被证明是一个强大的表格数据架构，但它仍然存在一些限制和缺点（有些是所有深度学习模型共有的，正如预期的那样）：

+   对于具有不同特征的数据集，**优化超参数**是具有挑战性的，可能需要大量的反复试验；

+   **处理高基数**特征仍然是个问题，因为模型学习和生成如此大量唯一类别变得困难；

+   处理**偏斜分布**或包含大量**常量值**（例如，大量0）的分布也是该架构难以捕捉的；

+   对于**小数据集**，合成的准确性可能较低，因为CTGAN与其他深度学习模型一样，对数据的要求较高；

+   训练和收敛可能需要大量的**计算资源和时间**，尤其是对于非常大的数据集；

总体而言，CTGAN在为**结构化、具有异构特征且训练规模适中的表格数据集生成合成数据**方面最为有效，但可能需要细致的观察，以发现特定的数据特征并评估模型是否在最佳条件下生成准确反映原始数据特性的合成数据。

# 最终思考

在本文中，我们讨论了CTGAN的工作原理，重点关注了该架构如何捕捉在现实世界领域广泛存在的某些复杂特征。此外，我们还探讨了`ydata-synthetic` Streamlit应用程序，这使我们可以开始使用合成数据，并在无代码、友好的环境中更多地了解CTGAN和其他生成模型。*很酷，对吧？*

即将加入 UI 的功能包括对时间序列模型的支持，即 [TimeGAN](/synthetic-time-series-data-a-gan-approach-869a984f2239)、CTGAN 的更高级设置，以及使用 `ydata-profiling` 的 [并排比较报告](https://pub.towardsai.net/how-to-compare-2-dataset-with-pandas-profiling-2ae3a9d7695e)。*期待未来的文章！*

一如既往，反馈、问题和建议总是受到欢迎：您可以留下评论、给仓库加星或贡献，甚至在 [数据中心 AI 社区](https://tiny.ydata.ai/dcai-medium) 讨论其他数据相关话题。*到时候见？*

# 关于我

博士，机器学习研究员，教育者，数据倡导者，以及全面的“万事通”。在 Medium 上，我撰写关于 **数据中心 AI 和数据质量** 的文章，教育数据科学与机器学习社区如何将不完美的数据转变为智能数据。

[数据中心 AI 社区](https://tiny.ydata.ai/dcai-medium) | [GitHub](https://github.com/Data-Centric-AI-Community) | [Google Scholar](https://scholar.google.com/citations?user=isaI6u8AAAAJ&hl=en) | [LinkedIn](https://www.linkedin.com/in/miriamseoanesantos/)

# 参考文献

1.  Xu, L., Skoularidou, M., Cuesta-Infante, A., & Veeramachaneni, K. [使用条件 GAN 建模表格数据](https://arxiv.org/abs/1907.00503)（2019年）。*神经信息处理系统进展，32*。

1.  Arjovsky, M., Chintala, S., & Bottou, L. [Wasserstein 生成对抗网络](https://arxiv.org/abs/1701.07875)（2017年）。在 *国际机器学习大会*（第214–223页）。

1.  Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. C. [改进的 Wasserstein GAN 训练](https://arxiv.org/abs/1704.00028)（2017年）。*神经信息处理系统进展*，*30*。

1.  Lin, Z., Khetan, A., Fanti, G., & Oh, S. PacGAN: [生成对抗网络中两个样本的力量](https://arxiv.org/abs/1712.04086)（2018年）。*神经信息处理系统进展*，*31*。

1.  Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A. & Bengio, Y. (2014). [生成对抗网络](https://arxiv.org/abs/1406.2661)。在 *神经信息处理系统进展*（第2672–2680页）。

1.  [成人普查收入](https://www.kaggle.com/datasets/uciml/adult-census-income) 数据集（获取自 [Kaggle](https://www.kaggle.com)，根据 [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/) 许可证）。Kohavi R.，[提高朴素贝叶斯分类器准确性的规模化：决策树混合方法](http://robotics.stanford.edu/~ronnyk/nbtree.pdf)（1996年），*第二届国际知识发现与数据挖掘大会论文集*。
