# **An imPULSE to Action: A Practical Solution for Positive-Unlabeled Classification**

> 原文：[https://towardsdatascience.com/an-impulse-to-action-a-practical-solution-for-positive-unlabelled-classification-cd5895128e45?source=collection_archive---------12-----------------------#2023-04-06](https://towardsdatascience.com/an-impulse-to-action-a-practical-solution-for-positive-unlabelled-classification-cd5895128e45?source=collection_archive---------12-----------------------#2023-04-06)

## 我们介绍了一种称为**ImPULSE Classifier**的方法，与其他方法相比，在平衡和不平衡的PU数据上表现更佳

[](https://wldmrgml.medium.com/?source=post_page-----cd5895128e45--------------------------------)[![Volodymyr Holomb](../Images/ff4a34f4dc4ee397d4d30512aa8f177c.png)](https://wldmrgml.medium.com/?source=post_page-----cd5895128e45--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd5895128e45--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cd5895128e45--------------------------------) [Volodymyr Holomb](https://wldmrgml.medium.com/?source=post_page-----cd5895128e45--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F95923fba037b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-impulse-to-action-a-practical-solution-for-positive-unlabelled-classification-cd5895128e45&user=Volodymyr+Holomb&userId=95923fba037b&source=post_page-95923fba037b----cd5895128e45---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd5895128e45--------------------------------) ·5分钟阅读·2023年4月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd5895128e45&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-impulse-to-action-a-practical-solution-for-positive-unlabelled-classification-cd5895128e45&user=Volodymyr+Holomb&userId=95923fba037b&source=-----cd5895128e45---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd5895128e45&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-impulse-to-action-a-practical-solution-for-positive-unlabelled-classification-cd5895128e45&source=-----cd5895128e45---------------------bookmark_footer-----------)![](../Images/344834119473633b29719a471ec76bee.png)

根据作者的描述，由DALL-E-2制作

在数据科学中，正负未标记的数据集，即正负未标记（PU）数据集，呈现出一个常见但具有挑战性的问题。PU 数据集的特点是存在未标记的数据，其中正实例未被明确标记为正。PU 数据集经常出现在商业场景中，其中只有一小部分数据被标记。其余未标记的样本可能属于正类或负类。这些场景包括欺诈检测、产品推荐、交叉销售或客户保留。目标是准确地将未标记数据分类为正类或负类，利用现有的标记数据。

![](../Images/a5ef4cb61b34b96594b8ded20e60fdad.png)

作者提供的图像

# 朴素方法

处理此任务时，首先想到的二分类方法是朴素方法。它涉及**将未标记的数据视为负类样本**。然而，这种方法在处理不平衡数据集时表现有限，即数据集中存在大量未知正样本。此外，它依赖于一个假设，即未标记数据中正样本的比例足够小，不会显著影响模型的性能。尽管实现起来很简单，但朴素方法可能并不适用于所有场景，并且在实践中常常导致性能不佳。

# Elkan 和 Noto 的方法

更复杂的正负未标记分类方法是 Elkan 和 Noto 的 (E&N) 方法。在这种方法中，我们通常训练一个分类器来预测样本被标记的概率，并进一步使用模型来估计正样本被标记的概率。然后，将未标记样本被标记的概率除以正样本被标记的概率，以获得样本为正的实际概率。你可以在[这里](/semi-supervised-classification-of-unlabeled-data-pu-learning-81f96e96f7cb)找到关于它如何工作的详细解释。

尽管 E&N 方法在实践中有效，但**它需要大量标记数据**来准确估计样本为正的概率，而在现实场景中，标记数据稀缺，这可能是不切实际的。

# 定制自我训练方法

然而，我们的团队开发了一种定制的 ImPULSE（代表**不平衡正负未标记学习与自我丰富**）解决方案，解决了这一问题，并在平衡和不平衡数据集上，相较于 E&N 方法表现更佳，即使在高比例的未知正标签情况下也是如此。

ImPULSE 分类器是对任何（潜在的）监督分类方法的一种修改，能够以半监督的方式工作。我们使用 LightGBM（作为基础估计器）以及一种修改后的工作流程，将所有预测概率作为训练集中每个样本的权重。这不仅使我们能够为模型训练的下一次迭代添加新的伪标签，还能使模型关注最有前途（有效）的负样本，从而使模型在更大、更具多样性的数据集上进行更有信心的再训练。此外，我们在每次迭代时提供调整后的类别权重，以处理不平衡数据。

![](../Images/7d7e29f5379a9a275233aac8fe40915b.png)

图片由作者提供

我们的方法通常涉及在可用数据上训练分类器，并使用它对未标记数据进行预测（以[自我训练方式](https://www.altexsoft.com/blog/semi-supervised-learning/)）。我们首先将数据拆分为训练集和留出评估集。然后，我们训练模型，同时监控其在评估集上的表现，以防止过拟合。

接下来，我们使用训练好的模型预测未标记数据的标签，并选择具有高置信度预测的样本作为正例。**我们使用这些正例来更新标记数据，并迭代地重新训练模型**。为了防止过拟合，我们实施了基于平均精度指标的自定义早停策略。

为了加速收敛，我们在每次迭代时提高学习率，使模型能更快地从更新的标记数据集中学习，并对未标记数据做出更好的预测。这个迭代过程会持续直到满足停止标准。总体而言，我们的方法专注于用有信息量的正例更新标记数据，并有效地防止过拟合，以提升模型性能。

# 实验运行

此外，我们希望解释如何评估三种提到的方法在分类正负未标记数据方面的性能。为此，我们开发了一个实验流程，利用了 LightGBM 分类器、一个独立的朴素估计器、E&N Noto 方法的 Python 实现以及我们自定义的自我训练算法。

为了创建一个受控的环境，我们使用 Scikit-learn 的 *make_classification* 函数生成了一个合成数据集，然后将其拆分为训练集和测试集。我们定义了希望执行的迭代次数，对应于要隐藏的正标签比例，然后通过在每次迭代时删除相应的标签来迭代地修改训练集。

在每次迭代中，我们将所有模型拟合到（修改后的）训练数据上，并在测试集上获取预测结果。我们使用预测标签和测试标签计算了F1分数，这是一种结合了精确率和召回率的指标。通过这样做，我们能够更严格地评估每种方法的性能，并在不同条件下进行比较。

下面是所有实验运行的结果：

您可以在[Jovian上找到相应的演示笔记本](https://jovian.com/wldmrgml/impulse-demo-git)，以及在[GitHub仓库中的完整代码](https://github.com/woldemarg/self_training_pu.git)。

# 结论

因此，我们开发了一种定制解决方案，用于正无标签分类，相比于E＆N方法在平衡和不平衡数据集上显示出了性能提升。我们的方法涉及使用可用数据训练分类器，并以自训练方式对未标记数据进行预测。这种解决方案被用作交叉销售和客户流失等应用分析工具中的核心分类算法。尽管我们的方法可能还不像[E＆N方法的已知Python实现](https://pulearn.github.io/pulearn/doc/pulearn/)那样强大和通用（但），但在实际业务场景中，我们的方法可能被视为一种实用选择，用于PU数据的分类。

# 参考阅读

1.  [Bekker Jessa, and Davis Jesse. “从正无标签数据中学习：一项调查。”，2018](https://doi.org/10.48550/arXiv.1811.04820)

1.  [Kiyomaru Hirokazu，一个收录在“从正无标签数据中学习：一项调查。”中介绍的算法的笔记本集合，2020](https://github.com/hkiyomaru/pu-learning)

1.  [Dobilas Saul. “自训练分类器：如何使任何算法表现得像半监督学习算法。”，2021](/self-training-classifier-how-to-make-any-algorithm-behave-like-a-semi-supervised-one-2958e7b54ab7)

1.  [Dorigatti, Emilio, et al. “具有自监督的强大高效不平衡正无标签学习。”，2022](https://doi.org/10.48550/arXiv.2209.02459)

1.  [JointEntropy. “了不起的ML正无标签学习。”，2022](https://github.com/JointEntropy/awesome-ml-pu-learning)

1.  [Agmon Alon. “半监督分类无标签数据（PU学习）。”，2022](/semi-supervised-classification-of-unlabeled-data-pu-learning-81f96e96f7cb)

1.  [Holomb, Volodymyr. “在商业分析中评估正无标签（PU）分类器的实用方法。”，2023](/a-practical-approach-to-evaluating-positive-unlabeled-pu-classifiers-in-real-world-business-66e074bb192f)
