# 如何使用迭代滤波评估未报告的流行病感染

> 原文：[https://towardsdatascience.com/how-to-evaluate-unreported-epidemic-infections-with-iterated-filtering-b7f470c6b6c6?source=collection_archive---------20-----------------------#2023-02-10](https://towardsdatascience.com/how-to-evaluate-unreported-epidemic-infections-with-iterated-filtering-b7f470c6b6c6?source=collection_archive---------20-----------------------#2023-02-10)

## 使用TFP进行基于似然的POMP推断实现

[](https://medium.com/@vanillaxiangshuyang?source=post_page-----b7f470c6b6c6--------------------------------)[![Shuyang Xiang](../Images/36a5fd18fd9b7b88cb41094f09b83882.png)](https://medium.com/@vanillaxiangshuyang?source=post_page-----b7f470c6b6c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7f470c6b6c6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b7f470c6b6c6--------------------------------) [Shuyang Xiang](https://medium.com/@vanillaxiangshuyang?source=post_page-----b7f470c6b6c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b74bc8c860d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-unreported-epidemic-infections-with-iterated-filtering-b7f470c6b6c6&user=Shuyang+Xiang&userId=9b74bc8c860d&source=post_page-9b74bc8c860d----b7f470c6b6c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7f470c6b6c6--------------------------------) ·6分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb7f470c6b6c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-unreported-epidemic-infections-with-iterated-filtering-b7f470c6b6c6&user=Shuyang+Xiang&userId=9b74bc8c860d&source=-----b7f470c6b6c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb7f470c6b6c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-unreported-epidemic-infections-with-iterated-filtering-b7f470c6b6c6&source=-----b7f470c6b6c6---------------------bookmark_footer-----------)![](../Images/18ca0a50748e692458a56c74e6777614.png)

作者提供的图像：一个描述流行病传播的典型SIR模型

了解疾病流行的动态对于公共卫生决策者制定进一步的预防措施至关重要。然而，流行病模型的推断可能会很困难，因为在大多数情况下，疾病传播只能部分观察到：不是所有的群体都能被完全观察到。

这种现象的一个例子是COVID-19的传播，由于多种可能的原因，仅报告了所有阳性病例的一部分：一些感染者没有严重症状，认为自己没有病毒；一些测试结果为假阴性；一些人不愿接受检测，等等。因此，很明显，每日报告的病例数小于实际感染数。

在这篇博客文章中，我将简要介绍用于推断这类部分观测马尔可夫过程（POMP）的迭代滤波算法，并在特定的部分观测流行病案例上使用TensorFlow Probability（TFP）的相应API。我希望突出我用TFP实现的算法，该实现几乎没有文档说明API的使用。

# POMP的推断

也称为隐状态空间模型或随机动态系统，部分观测马尔可夫过程（POMP）通常包含两个模型组件：一个未观测的马尔可夫过程{X(t; θ) : t ≥ 0}，可以是离散的或连续的时间，以及一个观察模型，该模型描述了在离散点收集的数据Y(t)与未观测状态X(t)之间的关系。

![](../Images/27981a5ebbc149129836375dbf6ccde7.png)

POMP，[图像](https://arxiv.org/abs/1712.03058)由Theresa Stocks提供

一般来说，POMP的推断开始于制定一个马尔可夫过程，并通过某种观察模型将观察到的数据与该过程连接起来，然后通过最大化模型的似然来学习模型参数的后验分布。

但这绝不是一件容易的事。

对POMP推断的研究量表明了这个问题的重大意义和困难。我们根据三个标准对这些方法进行了分类：即插即用的属性；完全信息还是基于特征；频率学派还是贝叶斯学派。你可以在[这里](https://kingaa.github.io/short-course/mif/mif.html#introduction)找到可用算法的列表。

# 迭代滤波

那么，为什么今天我要谈论迭代滤波呢？这是因为，在所有可用方法中，迭代滤波方法是当前唯一可用的、完全信息的、即插即用的、频率学派的方法，用于POMP模型，并且它成功地解决了基于似然的推断问题，特别是在对现有贝叶斯方法论计算上不可行的流行病学情况中。

我们可以简单地将迭代滤波理解为名字所示：通过迭代进行滤波。在这里，“滤波”一词可以理解为一种“估计器”，它从噪声数据中提取有关感兴趣数量的信息，参考Simon Haykin的[自适应滤波理论](http://users.ics.forth.gr/tsakalid/UVEG09/Book/Haykin-AFT%283rd.Ed.%29_Introduction.pdf)。

我们将简要介绍Ionides等人（2015）的IF2算法，是的，2006年有一个IF1，但今天不讨论。在这个IF2算法中，我们输入：(i) 初始状态的先验，(ii) 马尔科夫过程的传输模型，(iii) 描述状态与观察关系的观察模型，(iv) 观察数据，我们希望通过迭代得到模型参数的后验。

1.  每次迭代包括一个粒子滤波器，对每个粒子进行随机游走，使用参数向量进行操作。

2. 在时间序列的末尾，参数向量集合被回收作为下一次迭代的起始参数。

3. 随机游走方差在每次迭代中减少。

我不会详细讲解实现，但强烈建议读者查阅IF2算法伪代码。

![](../Images/dc899aa9258b39011478e0270b4cfa6a.png)

IF2伪代码：图片来自[wikipedia](https://en.wikipedia.org/wiki/Iterated_filtering)

# 部分记录感染的疫情示例

现在让我们考虑以下部分观察的动态示例：由[SIR模型](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology#The_SIR_model)描述的疫情动态，该模型将总人口分为三个部分：易感者、感染者和康复者。各部分之间的进展通过具有两个重要参数的常微分方程进行建模：感染率和康复率。假设只有一部分感染被记录，我们将这部分感染的比例称为报告率。

要了解疾病的进展，我们必须了解上述三个参数：**感染率**、**康复率**和**报告率**，给定**每日报告病例**，这些病例，抱歉，我必须再次强调，总是小于实际感染数。

让我们开始了解SIR模型的参数，基于作者模拟的[合成数据集](https://drive.google.com/drive/folders/1Tqbfi6K3xvF6Z7eekHb6bb8vXW_UP_2-)的每日报告感染情况。下图显示了100天内的每日报告病例图。

![](../Images/bbd9a567598a910026ac769c45f202a4.png)

作者提供的图片：100天内的报告感染

# 使用TFP进行推断

好消息是，IF2算法已经由TFP实现了[**tfp.experimental.IteratedFilter**](https://www.tensorflow.org/probability/api_docs/python/tfp/experimental/sequential/IteratedFilter)，我们可以直接使用。坏消息是，目前没有相关文档。在接下来的章节中，我将解释如何在上述数据集中使用该方法。让我们先看看API：

```py
tfp.experimental.sequential.IteratedFilter(
    parameter_prior,
    parameterized_initial_state_prior_fn,
    parameterized_transition_fn,
    parameterized_observation_fn,
    parameterized_initial_state_proposal_fn=None,
    parameterized_proposal_fn=None,
    parameter_constraining_bijector=None,
    name=None
)
```

为了初始化该方法，需要定义四个参数：parameter_prior、parameterized_initial_state_prior_fn、parameterized_transition_fn和parameterized_observation_fn。

在我们的示例中，我们将`parameter_prior`定义为我们感兴趣的三个比率的先验分布，使用**tfd.Distribution**作为均匀分布。

对于`parameterized_initial_state_prior_fn`，我们将其定义为一个将参数映射到SIR模型中各个部分的函数。

进一步地，我们将`parameterized_transition_fn`定义为一个描述模型所有部分如何在一个时间步后进展的函数，并且我们要指出，这个函数不过是原始SIR模型的离散化版本。

对于`parameterized_observation_fn`，我们将其定义为一个将SIR部分与观察到的报告感染联系起来的函数，也就是说，在每个时间步，报告的感染应该是新报告病例（根据模型，是两个时间步之间易感病例的差异）和报告率的乘积：即**reported_case_t=(suspectible_case_{t-1}- suspectible_case_t)*reported_rate**。

有关详细代码，请参见[notebook](https://colab.research.google.com/drive/1CYwlOM8CRFIOb3z57MPw352MamWlfw1x#scrollTo=Q_Nz6U-fgpYX)。一旦所有内容声明完毕，我们可以直接调用**iterated_filter.estimate_parameters**通过IF2算法来学习参数。下面的图表展示了报告率后验的可视化结果。我们可以看到，在这个示例中，只有约76%的感染病例被报告了。

![](../Images/e502b6ee998c9069c2a46f71b765e977.png)

作者提供的图片：报告率后验的箱线图

# 结论

在这篇博客文章中，我简要介绍了迭代滤波，并强调了它在POMP分析中的重要性，特别是在流行病传播研究中。我使用了TFP的IteratedFilter API，并以部分报告的流行病感染为例。欢迎提出任何问题。
