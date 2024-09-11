# 如何测量机器学习嵌入的漂移

> 原文：[https://towardsdatascience.com/how-to-measure-drift-in-ml-embeddings-ee8adfe1e55e?source=collection_archive---------0-----------------------#2023-06-14](https://towardsdatascience.com/how-to-measure-drift-in-ml-embeddings-ee8adfe1e55e?source=collection_archive---------0-----------------------#2023-06-14)

## 我们评估了五种嵌入漂移检测方法

[](https://medium.com/@elena.samuylova?source=post_page-----ee8adfe1e55e--------------------------------)[![Elena Samuylova](../Images/bc3024500f8b90a97f13d82ecaa1c9e7.png)](https://medium.com/@elena.samuylova?source=post_page-----ee8adfe1e55e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee8adfe1e55e--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee8adfe1e55e--------------------------------) [Elena Samuylova](https://medium.com/@elena.samuylova?source=post_page-----ee8adfe1e55e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9621354b583a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-drift-in-ml-embeddings-ee8adfe1e55e&user=Elena+Samuylova&userId=9621354b583a&source=post_page-9621354b583a----ee8adfe1e55e---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----ee8adfe1e55e--------------------------------) ·10分钟阅读·2023年6月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee8adfe1e55e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-drift-in-ml-embeddings-ee8adfe1e55e&user=Elena+Samuylova&userId=9621354b583a&source=-----ee8adfe1e55e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee8adfe1e55e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-drift-in-ml-embeddings-ee8adfe1e55e&source=-----ee8adfe1e55e---------------------bookmark_footer-----------)![](../Images/1b687c02077cf4d863483344e04b819a.png)

图片来源：作者。

# 为什么要监控嵌入漂移？

当机器学习系统投入生产时，你通常不会立即获得真实标签。模型会做出预测或分类，但你不知道它的准确性如何。你必须等一会儿（或者很久！）才能获得标签，以衡量模型的真实质量。

当然，测量实际表现是最有效的。但如果由于反馈延迟而无法做到这一点，则可以查看一些有价值的替代指标。其中之一是检测模型预测的漂移（“模型输出是否与之前不同？”）和模型输入的漂移（“输入到模型的数据是否不同？”）

**检测预测和数据漂移可以作为早期预警。** 这有助于查看模型是否收到了可能难以处理的新输入。了解环境的变化也可以帮助识别改进模型的方法。

你可以检测到什么类型的问题？例如，如果你的模型进行文本分类，你可能需要注意是否出现了新主题、情感的变化、类别平衡的变化、新语言的文本，或者当你开始接收到大量垃圾邮件或损坏的输入时。

![](../Images/96557e301472fcbb62b476ded9108921.png)

作者提供的图像。

但是如何准确地检测这种变化呢？

如果你处理结构化数据，有许多方法可以用来检测漂移：从跟踪模型输出的描述性统计（最小-最大范围、预测类别的占比等），到更[复杂的分布漂移检测](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets)方法，从像Kholmogorov-Smirnov这样的统计测试到像Wasserstein距离这样的距离度量。

然而，如果你正在处理NLP或LLM驱动的应用程序，你将处理非结构化数据。通常以数值表示的形式存在——嵌入。你如何检测这些数值表示中的漂移呢？

![](../Images/bc480957080e9c871af1ce9773d06c4b.png)

作者提供的图像。

这是一个相当新的话题，目前还没有“最佳实践”方法。为了帮助理解不同的嵌入漂移检测方法，我们进行了一系列[实验](https://www.evidentlyai.com/blog/embedding-drift-detection)。在本文中，我们将概述我们评估的方法，并介绍[Evidently](https://github.com/evidentlyai/evidently)，一个用于检测嵌入漂移（以及其他功能）的开源Python库。

# 漂移检测基础

[漂移检测](https://www.evidentlyai.com/blog/machine-learning-monitoring-data-and-concept-drift)的核心思想是当“数据发生显著变化”时发出警报。重点是整体分布。

这与[异常值检测](https://www.evidentlyai.com/blog/ml-monitoring-drift-detection-vs-outlier-detection)不同，当你想要检测与其他数据点不同的个别数据点时。

**要测量漂移，你需要决定你的参考数据集是什么。** 通常，你可以将当前生产数据与验证数据或你认为具有代表性的过去某个时期的数据进行比较。例如，你可以将本周的数据与上周的数据进行比较，并随着时间推移调整参考数据。

这个选择是针对具体用例的：你需要制定一个对数据稳定性或波动性的预期，并选择一个能够充分捕捉你期望的“典型”输入数据和模型响应分布的参考数据。

**你还必须选择漂移检测方法并调整警报阈值。** 比较数据集的方法和你认为有意义的变化程度有很多种。有时你关注的是微小的偏差，有时则只是显著的变化。为了调整它，你可以[使用历史数据](https://www.evidentlyai.com/blog/tutorial-3-historical-data-drift)来建模你的漂移检测框架，或者选择一些合理的默认值，然后在实际操作中进行调整。

让我们来详细了解一下“如何”。假设你正在处理文本分类用例，并希望比较数据集（以嵌入形式表示）每周如何变化。你有两个嵌入数据集需要比较。你如何准确地衡量它们之间的“差异”？

我们将审查五种可能的方法。

# 欧几里得距离

![](../Images/d6a6ea85e1bbe82cd7c2529a1ae9ca5c.png)

作者提供的图片。

你可以对两个分布中的嵌入进行平均，从而得到每个数据集的代表性嵌入。然后，你可以测量它们之间的[欧几里得距离](https://en.wikipedia.org/wiki/Euclidean_distance)。通过这种方式，你可以比较两个向量在多维空间中的“距离”。

欧几里得距离是一种直接且熟悉的度量：它测量连接两个嵌入的线段的长度。你也可以使用其他距离度量，例如余弦、曼哈顿或切比雪夫距离。

作为漂移评分，你将得到一个从0（对于相同的嵌入）到无穷大的数字。值越高，两个分布之间的距离越远。

这种行为是直观的，但一个可能的缺点是欧几里得距离是一个绝对度量。这使得设置特定的漂移警报阈值更困难：“远”的定义将基于用例和使用的嵌入模型而有所不同。你需要为你监控的不同模型单独调整阈值。

# 余弦距离

![](../Images/9a9b249d11fe71a3d725c563924b897c.png)

作者提供的图片。

余弦距离是另一种流行的距离度量。它不是测量“长度”，而是计算向量之间角度的余弦值。

余弦相似度在机器学习中被广泛使用于搜索、信息检索或推荐系统等任务。要测量距离，你必须从1中减去余弦值。

*余弦距离 = 1 — 余弦相似度*

如果两个分布相同，余弦相似度将为1，距离将为0。距离可以取值从0到2。

在我们的[实验](https://www.evidentlyai.com/blog/embedding-drift-detection)中，我们发现阈值的调整可能不太直观，因为对于你已经想要检测的变化，它可能会取值低至0.001。请明智地选择阈值！另一个缺点是，如果应用像PCA这样的降维方法，它将不起作用，导致结果不可预测。

# 基于模型的漂移检测

![](../Images/16348f9132065c602e2c5064a32022d9.png)

图片由作者提供。

基于分类器的漂移检测方法遵循不同的思路。你可以训练一个分类模型，而不是测量平均嵌入之间的距离，该模型试图识别每个嵌入属于哪个分布。

如果模型可以自信地预测特定嵌入来自哪个分布，那么这两个数据集可能是足够不同的。

你可以测量在验证数据集上计算的分类模型的[ROC AUC分数](https://www.evidentlyai.com/classification-metrics/explain-roc-curve)作为漂移分数，并相应地设置阈值。分数高于0.5表明至少有一些预测能力，而分数为1则表示“绝对漂移”，即模型总是能识别数据属于哪个分布。

你可以在[论文](https://arxiv.org/pdf/1810.11953.pdf)《大声失败：检测数据集变化方法的实证研究》中阅读更多关于该方法的信息。

根据我们的实验，这种方法是一个很好的默认选择。它在我们测试的不同数据集和嵌入模型中都表现一致，无论是否使用PCA。它还具有任何数据科学家都熟悉的直观阈值。

# 漂移组件的比例

![](../Images/e705ca52602ced8c120b51d8c618f5b1.png)

图片由作者提供。

该方法的思想是将嵌入视为结构化的表格数据，并应用数值漂移检测方法——这与你用于检测数值特征漂移的方法相同。每个嵌入的各个组件被视为结构化数据集中的“列”。

当然，与数值特征不同，这些“列”没有可解释的意义。它们是输入向量的*一些*坐标。然而，你仍然可以测量这些坐标中有多少发生漂移。如果很多发生漂移，数据可能发生了有意义的变化。

要应用此方法，你首先必须计算每个组件的漂移。在我们的实验中，我们使用了[Wasserstein（地球移动者）距离](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets#wasserstein-distance-earth-mover-distance)并设置了0.1的阈值。这一指标的直觉是，当你将阈值设置为0.1时，你将注意到“0.1标准差”大小的变化。

然后，你可以测量漂移组件的总体比例。例如，如果你的向量长度是400，你可以将阈值设置为20%。如果超过80个组件发生漂移，你将收到漂移检测警报。

这种方法的好处在于你可以在0到1的范围内测量漂移。你还可以重用你可能用于检测表格数据漂移的熟悉技术。（还有[不同](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets#kolmogorov-smirnov-ks-test)的方法，比如K-L散度或各种统计测试）。

然而，对某些人来说，这可能是一个限制：你需要设置很多参数。你可以调整基础的漂移检测方法、其阈值以及响应的漂移组件的比例。

总的来说，我们认为这种方法有其优点：它与不同的嵌入模型一致，并且计算速度合理。

# 最大均值差异 (MMD)

![](../Images/45ff746998ce6b822ce334537c71a135.png)

图片由作者提供。

你可以使用 MMD 来测量向量均值之间的多维距离。目标是根据分布的均值嵌入 µp 和 µq 在 [再现核希尔伯特空间](https://en.wikipedia.org/wiki/Reproducing_kernel_Hilbert_space) F 中区分两个概率分布 p 和 q。

形式上：

![](../Images/13703b67d74abda53b1926fae7b46eb2.png)

另一种表示 MMD 的方法，其中 K 是再现核希尔伯特空间：

![](../Images/076037e1f773594e6850a0dc6fd42042.png)

你可以将 K 视为某种接近度度量。对象越相似，这个值越大。如果两个分布相同，MMD 应该是 0。如果两个分布不同，MMD 会增加。

![](../Images/38a3b4059b10afd1bbcb7e934595c9bf.png)

图片由作者提供。*使用 MMD 比较不同的分布。*

你可以在论文 “[A Kernel Method for the Two-Sample Problem](https://arxiv.org/abs/0805.2368)” 中阅读更多内容。

你可以使用 MMD 度量作为漂移分数。这个方法的缺点是许多人不熟悉这种方法，阈值也不可解释。计算通常比其他方法更长。如果你有理由并且对其背后的数学有坚实的理解，我们建议使用它。

# 选择哪种方法？

为了形成本方法的直觉，我们通过引入人工漂移到三个数据集进行了一系列实验，并随着漂移的增加估计漂移结果。我们还测试了计算速度。

你可以在 [单独的博客](https://www.evidentlyai.com/blog/embedding-drift-detection) 中找到所有的代码和实验细节。

**以下是我们的建议：**

+   基于模型的漂移检测以及使用 ROC AUC 分数作为漂移检测阈值是一个很好的默认选择。

+   跟踪漂移的嵌入组件比例从 0 到 1 是一个接近的选择。只要记住，如果你应用了降维，请调整阈值。

+   如果你想衡量时间中的“漂移大小”，像欧几里得距离这样的度量是一个不错的选择。然而，你需要决定如何设计警报，因为你将处理绝对距离值。

![](../Images/55710a84105e151bb3693f23442fbd97.png)

图片由作者提供。总结来自于 [研究博客](https://www.evidentlyai.com/blog/embedding-drift-detection)。

**需要记住的是，漂移检测是一种启发式方法。** 它是潜在问题的代理。你可能需要实验这种方法：不仅选择方法，还需要根据你的数据调整阈值。你还必须仔细考虑参考窗口的选择，并对你认为有意义的变化做出明智的假设。这将取决于你的错误容忍度、用例以及对模型泛化能力的期望。

**你也可以将漂移检测与调试分开。** 一旦收到可能的嵌入漂移警报，下一步就是调查具体发生了什么变化。在这种情况下，你必须不可避免地回顾原始数据。

**如果可能的话，你甚至可以首先从评估原始数据中的漂移开始。** 这样，你可以获得有价值的见解：例如，[识别顶级词汇](https://www.evidentlyai.com/blog/tutorial-detecting-drift-in-text-data)，帮助分类器模型决定文本属于哪个分布，或跟踪可解释的文本描述符——如长度或 OOV 词的比例。

![](../Images/e345ca8e2a9a26df805ac6b1616a1bb7.png)

*使用原始数据进行漂移检测的示例。*

# Evidently 开源

我们在 [Evidently](https://github.com/evidentlyai/evidently) 中实现了所有提到的漂移检测方法，这是一个开源 Python 库，用于评估、测试和监控机器学习模型。

你可以在任何 Python 环境中运行它。只需传递一个 DataFrame，选择包含嵌入的列，并选择漂移检测方法（或使用默认设置！）你还可以将这些检查作为管道的一部分实现，并使用 100 多种其他数据和模型质量检查。

![](../Images/0e91df16a7707701e796b30111d22186.png)

*来源：关于* [*嵌入漂移检测*](https://github.com/evidentlyai/evidently/blob/main/examples/how_to_questions/how_to_calculate_embeddings_drift.ipynb)* 的示例笔记本。*

你可以查看 [入门教程](https://docs.evidentlyai.com/get-started/tutorial/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=embedding-drift-detection) 以了解 Evidently 的功能，或直接跳转到关于嵌入漂移检测的 [代码示例](https://github.com/elenasamuylova/evidently/blob/main/examples/how_to_questions/how_to_calculate_embeddings_drift.ipynb)。

# 结论

+   漂移检测是生产环境中监控机器学习模型的宝贵工具。它有助于检测输入数据和模型预测的变化。你可以将其作为**模型质量的代理指标**，并提醒你潜在的变化。

+   漂移检测不仅限于处理表格数据。你还可以**监控机器学习嵌入的漂移**——例如，在运行自然语言处理（NLP）或基于大语言模型（LLM）的应用时。

+   在这篇文章中，我们介绍了**5种不同的嵌入监测方法**，包括欧几里得距离和余弦距离、最大均值差异、基于模型的漂移检测，以及使用数值漂移检测方法跟踪漂移嵌入的比例。所有这些方法都在开源的Evidently Python库中实现。

+   我们推荐**基于模型的漂移检测**作为一种合理的默认方法。这是因为它能够使用ROC AUC评分作为可解释的漂移检测指标，易于调整和操作。这种方法在不同的数据集和嵌入模型上表现一致，使其在不同项目中实践起来更加方便。

—

*这篇文章基于Evidently* [*博客*](https://www.evidentlyai.com/blog/embedding-drift-detection)*上的研究。感谢* [*Olga Filippova*](https://www.evidentlyai.com/author/olga-fillipova) *共同撰写了这篇文章。*
