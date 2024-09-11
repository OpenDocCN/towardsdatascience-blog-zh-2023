# 《构建决策树分类器：从头构建决策树模型的全面指南》

> 原文：[https://towardsdatascience.com/constructing-a-decision-tree-classifier-a-comprehensive-guide-to-building-decision-tree-models-2e59959db22d?source=collection_archive---------13-----------------------#2023-03-29](https://towardsdatascience.com/constructing-a-decision-tree-classifier-a-comprehensive-guide-to-building-decision-tree-models-2e59959db22d?source=collection_archive---------13-----------------------#2023-03-29)

## 了解从头构建决策树分类器所涉及的基本过程

[](https://suhas-maddali007.medium.com/?source=post_page-----2e59959db22d--------------------------------)[![Suhas Maddali](../Images/933f27eab8ba9ee1f06ed2f24746d788.png)](https://suhas-maddali007.medium.com/?source=post_page-----2e59959db22d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e59959db22d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2e59959db22d--------------------------------) [苏哈斯·马达利](https://suhas-maddali007.medium.com/?source=post_page-----2e59959db22d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2a74f90399ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-a-decision-tree-classifier-a-comprehensive-guide-to-building-decision-tree-models-2e59959db22d&user=Suhas+Maddali&userId=2a74f90399ae&source=post_page-2a74f90399ae----2e59959db22d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e59959db22d--------------------------------) ·7分钟阅读·2023年3月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2e59959db22d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-a-decision-tree-classifier-a-comprehensive-guide-to-building-decision-tree-models-2e59959db22d&user=Suhas+Maddali&userId=2a74f90399ae&source=-----2e59959db22d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2e59959db22d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-a-decision-tree-classifier-a-comprehensive-guide-to-building-decision-tree-models-2e59959db22d&source=-----2e59959db22d---------------------bookmark_footer-----------)![](../Images/20a9591784f2a489089a025aa9089eb8.png)

照片由 [Jeroen den Otter](https://unsplash.com/@jeroendenotter?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**决策树**在机器学习中有多种用途，包括分类、回归、特征选择、异常检测和强化学习。它们通过简单的**if-else**语句进行操作，直到达到树的深度。理解某些关键概念对于全面理解决策树的内部工作原理至关重要。

探索决策树时需要掌握两个关键概念：**熵**和**信息增益**。熵量化了训练样本集中的杂质。一个只包含一个类别的训练集的熵为0，而一个包含所有类别样本均匀分布的集合的熵为1。信息增益则表示通过根据特定属性将训练样本划分为子集所获得的熵或杂质的减少。对这些概念的深入理解对于理解决策树的内部机制非常有价值。

我们将开发一个**决策树类**并定义进行预测所需的基本属性。如前所述，熵和信息增益是在决定哪个属性进行拆分之前计算的。在训练阶段，节点被拆分，这些值在推断阶段用于进行预测。我们将通过查看代码片段来检查这是如何完成的。

## 决策树分类器的代码实现

初步步骤包括创建一个决策树类，在随后的代码段中添加方法和属性。本文主要强调从头开始构建决策树分类器，以便清晰理解复杂模型的内部机制。以下是开发决策树分类器时需要考虑的一些事项。

## **定义决策树类**

在这个代码段中，我们定义了一个决策树类，具有一个**构造函数**，接受max_depth、min_samples_split和min_samples_leaf的值。**max_depth**属性表示算法可以停止节点拆分的最大深度。**min_samples_split**属性考虑节点拆分所需的最小样本数量。**min_samples_leaf**属性指定叶节点中的样本总数，超过此数量算法将限制进一步的分裂。这些超参数以及其他未提及的参数将在稍后定义各种功能的方法时使用。

## **熵**

这个概念涉及数据中存在的**不确定性**或**杂质**。它用于通过计算通过拆分所获得的整体信息增益来识别每个节点的最佳拆分点。

这段代码根据输出样本中每个类别的样本计数计算整体熵。需要注意的是，输出变量可能有两个以上类别（多类别），使得该模型同样适用于多类别分类。接下来，我们将引入一种计算**信息增益**的方法，该方法帮助模型根据此值拆分示例。以下代码片段概述了执行的步骤顺序。

## **信息增益**

下定义了一个**阈值**，它将数据分为左右两个节点。这个过程对所有特征索引进行，以识别最佳拟合。随后，记录拆分后得到的熵差异，并返回作为特定特征拆分的总信息增益。最后一步涉及创建一个**split_node**函数，该函数基于从拆分中得出的信息增益对所有特征执行拆分操作。

## **拆分节点**

我们通过定义关键超参数如`max_depth`和`min_samples_leaf`来启动这一过程。这些因素在`split_node`方法中发挥着至关重要的作用，因为它们决定是否应进一步拆分。例如，当树达到最大深度或满足最小样本数时，数据拆分将停止。

一旦满足最小样本数和最大树深度条件，下一步是**识别**从拆分中提供最高信息增益的特征。为此，我们遍历所有特征，计算每个特征拆分所产生的总熵和信息增益。最终，提供最大信息增益的特征作为将数据分为左右节点的参考。这一过程持续进行，直到树的深度达到并在拆分过程中考虑到最小样本数。

## **模型拟合**

接下来，我们利用之前定义的方法来拟合我们的模型。`split_node`函数在计算通过不同特征将数据划分为两个子集所产生的熵和信息增益方面**至关重要**。结果是，树达到了其最大深度，使得模型获得了优化的特征表示，从而简化了推断过程。

`split_node`函数接受一组属性，包括输入数据、输出和深度，这是一个**超参数**。该函数基于初始训练数据遍历决策树，识别拆分的最佳条件。随着树的遍历，深度、最小样本数和最小叶子数等因素在确定最终预测时发挥作用。

一旦使用适当的超参数构建了决策树，就可以用它来对未见或测试数据点进行预测。在接下来的章节中，我们将探讨模型如何利用由`split_node`函数生成的**结构良好的**决策树处理新数据的预测。

## 定义预测函数

我们将定义`predict`函数，该函数接受**输入**并对每个实例做出预测。基于先前定义的阈值进行拆分，模型会**遍历**树，直到为测试集获得结果。最后，预测以数组形式返回给用户。

这个`predict`方法作为决策树分类器的**决策功能**。它开始时初始化一个空列表`y_pred`，以存储给定输入值集的预测类别标签。然后，算法遍历每个输入示例，将当前节点设置为决策树的根节点。

当算法遍历树时，它会遇到包含每个特征关键信息的基于字典的节点。这些信息帮助算法决定是**向左**还是**向右**子节点移动，取决于特征值和指定的阈值。遍历过程继续，直到到达叶子节点。

达到叶子节点后，预测的类别标签会被追加到`y_pred`列表中。这个过程对每个输入示例重复，生成一个预测列表。最后，预测列表会被转换成**NumPy**数组，为每个测试数据点提供预测的类别标签。

## 可视化

在本小节中，我们将检验应用于估计**AirBnb**房价的数据集的决策树回归模型的输出。需要注意的是，可以为各种情况生成类似的图表，其中**树的深度**和其他**超参数**表示决策树的复杂性。

在本节中，我们强调**机器学习（ML）模型的可解释性**。随着各行各业对机器学习需求的激增，不能忽视模型可解释性的重要性。与其将这些模型视为黑箱，不如开发工具和技术来揭示它们的内部工作原理，并阐明其预测背后的理由。通过这样做，我们培养对机器学习算法的信任，并确保它们在各种应用中的负责任整合。

注意：数据集取自[纽约市Airbnb开放数据 | Kaggle](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)，根据[创作共用 — CC0 1.0 全球](https://creativecommons.org/publicdomain/zero/1.0/)许可协议。

![](../Images/c6f9fdfacf6db3c21bae23f5cac23422.png)

决策树回归模型（图片由作者提供）

决策树回归器和分类器因其**可解释性**而闻名，提供了对其预测背后**理由**的宝贵洞察。这种清晰度通过与领域知识对齐来增强对模型预测的信任和信心，并提高我们的理解。此外，它还提供了调试的机会，并处理伦理和法律问题。

在进行超参数调整和优化后，**AirBnb** 房价预测问题的最佳树深度被确定为 **2**。利用这一深度并可视化结果时，Woodside 邻里、经度和Midland Beach 邻里等特征被发现是预测 AirBnb 房价最重要的因素。

## 结论

完成本文后，你应该对决策树模型的机制有一个**坚实**的理解。从头到尾了解模型的实现尤为重要，特别是在使用**scikit-learn**模型及其超参数时。此外，你可以通过调整阈值或其他超参数来定制模型，从而提高性能。感谢你花时间阅读这篇文章。

*以下是你可以联系我或查看我工作的方式。*

***GitHub:***[*suhasmaddali (Suhas Maddali ) (github.com)*](https://github.com/suhasmaddali)

***YouTube:***[*https://www.youtube.com/channel/UCymdyoyJBC_i7QVfbrIs-4Q*](https://www.youtube.com/channel/UCymdyoyJBC_i7QVfbrIs-4Q)

***LinkedIn:***[*(1) Suhas Maddali, Northeastern University, Data Science | LinkedIn*](https://www.linkedin.com/in/suhas-maddali/)

***Medium:*** [*Suhas Maddali — Medium*](https://suhas-maddali007.medium.com/)

***Kaggle:***[*Suhas Maddali | Contributor | Kaggle*](https://www.kaggle.com/suhasmaddali007)
