# 治疗与反应的故事

> 原文：[https://towardsdatascience.com/a-story-of-treatment-and-response-63fd1fc80e27?source=collection_archive---------9-----------------------#2023-03-03](https://towardsdatascience.com/a-story-of-treatment-and-response-63fd1fc80e27?source=collection_archive---------9-----------------------#2023-03-03)

## 《预测营销活动受众治疗效果简介》

[](https://medium.com/@js.schmidl?source=post_page-----63fd1fc80e27--------------------------------)[![Jürgen Schmidl](../Images/d445090530fbee0bb4c2e6d8b1cfd69f.png)](https://medium.com/@js.schmidl?source=post_page-----63fd1fc80e27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63fd1fc80e27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----63fd1fc80e27--------------------------------) [Jürgen Schmidl](https://medium.com/@js.schmidl?source=post_page-----63fd1fc80e27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe7cd77e36518&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-story-of-treatment-and-response-63fd1fc80e27&user=J%C3%BCrgen+Schmidl&userId=e7cd77e36518&source=post_page-e7cd77e36518----63fd1fc80e27---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----63fd1fc80e27--------------------------------) · 9 min read · 2023年3月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F63fd1fc80e27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-story-of-treatment-and-response-63fd1fc80e27&user=J%C3%BCrgen+Schmidl&userId=e7cd77e36518&source=-----63fd1fc80e27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F63fd1fc80e27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-story-of-treatment-and-response-63fd1fc80e27&source=-----63fd1fc80e27---------------------bookmark_footer-----------)![](../Images/a79a431dd08a5b732da34926c72671fb.png)

图片由 geralt @ pixabay.com 提供

想象一下，你负责挑选应该收到下一个印刷广告材料的客户。

当然，只有对那些影响最大的人发放广告材料才是合理的。但这比说起来容易做。本文将探讨机器学习如何用来预测刺激（如促销）的治疗效果。

这种方法在营销活动中进行了说明，但也可以应用于其他问题。此外，本文介绍的方法有一个概念验证。[您可以在这里找到它。](https://colab.research.google.com/drive/1SOKW5Njv5iN21R_syNVK0S9bpFeLehs8?usp=sharing)

*请注意，尽管我提到了来源，但本文基于我在该领域的经验。如果您发现任何改进的地方，请给我留言。*

# 什么是反应及其测量方法？

## 反应的定义

任何接受处理的人可以以两种不同的方式响应。这个人可以忽视处理或对其做出反应。这种反应代表了处理效果。

首先，我们需要确定如何测量反应。在促销的背景下，购买本身在文献中通常被用作反应的二元术语。

如果反应的强度相当均匀，这可能就足够了。然而，对于大多数用例，反应的强度也应该被考虑。在我们的例子中，这将是订单的价值，这取决于反应的强度而变化。

因此，我建议将反应定义为通过促销活动带来的收入。

## 反应的测量

基于这个定义，我们现在可以继续测量处理效果。

这也是第一个挑战所在。购买可能是处理的因果效应，但也可能以“自然”的方式发生。例如，假设客户在收到广告后的两个月下订单。您会认为这个订单是广告的因果结果吗？根据我的经验，解决这个问题有两种方法：

## **有处理参考的反应**

一种方法是将反应直接归因于促销处理。这可以通过跟踪基础处理的优惠券代码或广告专用商品编号来实现。如果这样的因果分配是可能的，可以称之为有处理参考的反应。

如果广告材料包含一个没有或很低最低订单金额的优惠券，根据我的经验，几乎所有客户都会使用它。然而，对于促销商品编号而言，进行因果分配则更加困难，因为许多客户也会直接在网络上搜索这些商品。这可能会根据具体业务有所不同。

![](../Images/d5b50240b044e06c823691c2aa7b5e91.png)

有处理参考的反应示意图（作者提供的图片）

## **没有处理参考的反应**

另一种方法是将销售归因于特定时间段的促销活动，例如广告活动后的30天。这可以描述为没有处理参考的反应。

确定最佳时间段需要仔细考虑——评估期应对所有广告材料保持一致，并且不应与后续处理重叠。它还应涵盖大多数可能的反应，同时保持尽可能简短。

![](../Images/9bd778f98f3509c929038c8b38bfde95.png)

无处理参考的响应示意图（图片由作者提供）

这两种方法都不是完美的。一方面，有些客户有意识或无意识地隐瞒了处理参考（例如，不兑换优惠券或不使用提供的项目编号）。另一方面，并不是每次购买都是由处理引发的，即使它在指定的时间段内。

在我看来，每当大比例的反应可以归因于某种广告媒介时，应使用带处理参考的收入。这使得模型创建更容易，并且需要更少的数据来获得良好的结果。

# 选择你的方法

一旦你决定了标签（因变量）的定义并创建了一些自变量，你就可以开始建模。根据标签的不同情况，预测客户反应可能需要不同的模型。

如引言中所述，我更喜欢将订单价值作为标签。然而，也可以仅预测购买概率。因此，这也应考虑在内。

![](../Images/75df75a9125c7422905ff7f991292673.png)

ITE = 个体处理效果 / CATE = 条件平均处理效果（表由作者提供）

上表展示了多种建模处理效果的方法。如你所见，不同的标签定义需要不同的建模方法。

在这篇文章中，我将重点介绍可以建立因果关系的处理相关反应预测方法（见上表：“带处理参考”）。提升建模是一个独立的话题，需要单独的文章。我会在发布后将其链接到这里。在此之前，我很高兴推荐给你Shelby Temple：

[## 快速提升建模介绍](/a-quick-uplift-modeling-introduction-6e14de32bfe0?source=post_page-----63fd1fc80e27--------------------------------)

### 了解提升建模如何改进经典数据科学应用。

[towardsdatascience.com](/a-quick-uplift-modeling-introduction-6e14de32bfe0?source=post_page-----63fd1fc80e27--------------------------------)

因此，本文仅处理直接归因于广告活动的销售预测。这种方法对于大多数用例是足够的，特别是因为提升建模需要没有接受任何广告材料的对照组。

# **数据集**

## 多期数据集

为了创建一个有效的模型，利用多个过去促销的数据非常有帮助。如果这些活动覆盖整年，模型可以理解不同类型的广告材料和季节性变化如何影响结果。

历史数据可能并不总是需要用于那些由于促销而未发生变化的客户属性，如年龄和性别。然而，对于其他属性，例如客户多久前进行了购买，重要的是确保处理没有影响独立变量。（例如，包括因广告而进行的购买）

不幸的是，如果一个独立变量由于客户的反应发生了变化，就足以极大地影响整个模型。在这种情况下，模型在生产环境中的结果显著差于测试中的表现。为了避免这种情况，建议在创建任何变量之前，将促销前的数据与促销后的响应分开。

## 零膨胀数据集

建模客户响应的下一个挑战是数据集中存在大量零值。这是因为许多客户不会回应促销材料，因此不会进行购买。（相比之下，印刷广告材料的回应率达到10%被认为是非常好的。）

为了更好地说明这一点，我使用了[Kevin Hillstrom在Scikit-Uplift包中包含的数据集](https://www.uplift-modeling.com/en/latest/api/datasets/fetch_hillstrom.html)作为本文的例子，并在[随附的笔记本中](https://colab.research.google.com/drive/1SOKW5Njv5iN21R_syNVK0S9bpFeLehs8?usp=sharing)展示。

数据集包含多个独立变量以及对电子邮件营销活动的响应。电子邮件活动的响应分布可以通过直方图显示，如下所示：

![](../Images/5a07322e67cb1333b6381faf88148368.png)

Hillstroem数据集中的响应直方图。（图像由作者提供）

这类数据集通常被称为零膨胀数据集。处理这类数据集有两种不同的方法：**过采样**或**建模**。

对于过采样，我推荐[SMOTER](https://researchcommons.waikato.ac.nz/bitstream/handle/10289/8518/smoteR.pdf;jsessionid=D35C64E5C6994E13BF9C1236166B1B4F?sequence=23)（用于回归的合成少数过采样技术）或[SMOGN](https://www.researchgate.net/publication/319906917_SMOGN_a_Pre-processing_Approach_for_Imbalanced_Regression)（带高斯噪声的合成少数过采样技术）。这些算法可以帮助建模零膨胀数据集。

# 建模

然而，我最喜欢的方法不是过采样，而是根据数据调整模型。

一些回归器可以默认处理零膨胀数据集，如基于树的回归模型，例如随机森林回归器。然而，经验表明，这并不是建立此类模型的最佳方式。

一方面，这归因于较低的可解释性，另一方面则是由于较差的性能，尤其是当数据集中包含大量独立变量时。

因此，我通常使用两步模型。在这里，数据集被分解为分类问题和回归问题。

![](../Images/af345a9f60523742977b2f2754281efa.png)

使用两步模型预测零膨胀数据集的示例（图像由作者提供）

这种方法的优势在于回归不再需要学习包含零值的数据集，因为零/非零分类由其自身的估计器处理。

**分类：**

分类组件预测数据点为零的概率。分类器在完整的数据集上进行训练。（过采样仍然有用）

**回归：**

回归仅在大于零的数据点上进行训练。这样，回归不必处理零膨胀，并且可以选择大量回归器。

使用类似方法的模型通常被称为响应-支出元学习器（保险行业中的风险/严重性元学习器）。它们通常表现更好，并在选择模型时提供更多灵活性。

这两种模型的结果可以通过从购买概率和购买时的销售值形成期望购买值来相对容易地结合起来。

![](../Images/35ab00152f007e1d291d035949a67395.png)

形成期望值的函数示例（图像由作者提供）

作为期望值主题的复习，我推荐阅读这篇文章。

[](/what-is-expected-value-4815bdbd84de?source=post_page-----63fd1fc80e27--------------------------------) [## 期望值是什么？

### 通过使用游戏的简单示例来直观地解释期望值

towardsdatascience.com](/what-is-expected-value-4815bdbd84de?source=post_page-----63fd1fc80e27--------------------------------)

基于这里介绍的程序，我还创建了一个简短的笔记本作为概念验证。在这里，我比较了随机森林回归器与元学习器的表现，后者适配了线性回归和逻辑回归。

你可以在这里找到它：[相关笔记本](https://colab.research.google.com/drive/1SOKW5Njv5iN21R_syNVK0S9bpFeLehs8?usp=sharing)

# 评估

元学习器可以作为回归进行评估，或者可以单独评估其组件（回归和分类）。

实际上，如果模型的表现没有传达给数据科学家，通常最好避免使用如RSME或MAE这样的值，而是回答问题：‘模型能多好地区分好客户与坏客户？’

为了演示这一点，我按预测质量的降序对客户进行排序（从预测最高的客户到预测最低的客户），并绘制在活动中实现的实际或理论销售额。（注意：用于评估的活动必须不包括在训练数据集中）。

![](../Images/039b9e6dc4da1c795c38a7e77c289a6f.png)

优秀的接收者选择示例（合成数据，作者提供的图像）

使用好的选择模型，预测非常好的客户也应该产生非常高的收入，这些收入应迅速下降并趋近于零。

# 结论

在本文中，我分享了使用机器学习进行客户选择的经验。然而，需要注意的是，当客户响应无法充分衡量时，需要采取不同的方法。

为了解决这个问题，我的下一篇文章将扩展这里介绍的程序，以预测提升（个体处理效应/条件平均处理效应）。

否则，我很高兴你能坚持到现在，并感谢你的关注。如果你有任何问题或建议，请随时留言。

## 数据集：

数据集来自Kevin Hillstrom的博客 [“MineThatData”](https://blog.minethatdata.com/2008/03/minethatdata-e-mail-analytics-and-data.html)，并在许多Python包和科学出版物中使用。在我的文章中，我提到数据集在 [Python包“Scikit-Uplift”](https://www.uplift-modeling.com/en/latest/api/datasets/fetch_hillstrom.html) 中的实现，该包在 [MIT许可证](https://github.com/maks-sh/scikit-uplift/blob/master/LICENSE) 下发布。

## **资源：**

McCrary, M. 使用多阶段模型增强客户定位：预测零售业的客户销售和利润。 *J Target Meas Anal Mark* **17**, 273–295 (2009). [https://doi.org/10.1057/jt.2009.22](https://doi.org/10.1057/jt.2009.22)

Torgo, L., Ribeiro, R.P., Pfahringer, B., Branco, P. (2013). SMOTE回归。载于：Correia, L., Reis, L.P., Cascalho, J. (编辑) 《人工智能进展》。EPIA 2013\. 计算机科学讲义集()，第8154卷\. 施普林格，柏林，海德堡。 [https://doi.org/10.1007/978-3-642-40669-0_33](https://doi.org/10.1007/978-3-642-40669-0_33)

Torgo, L., Ribeiro, R.P., Branco P. (2017) SMOGN: 不平衡回归的预处理方法。载于：2017年机器学习研究会议论文集
