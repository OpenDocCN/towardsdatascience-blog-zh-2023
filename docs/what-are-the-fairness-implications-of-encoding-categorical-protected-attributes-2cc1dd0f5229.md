# 对编码分类受保护属性的公平性影响是什么？

> 原文：[https://towardsdatascience.com/what-are-the-fairness-implications-of-encoding-categorical-protected-attributes-2cc1dd0f5229?source=collection_archive---------5-----------------------#2023-05-06](https://towardsdatascience.com/what-are-the-fairness-implications-of-encoding-categorical-protected-attributes-2cc1dd0f5229?source=collection_archive---------5-----------------------#2023-05-06)

## 探讨编码受保护属性对机器学习公平性的影响

[](https://medium.com/@carmougan?source=post_page-----2cc1dd0f5229--------------------------------)[![Carlos Mougan](../Images/7a56269362fc48c7a6179474b106857a.png)](https://medium.com/@carmougan?source=post_page-----2cc1dd0f5229--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2cc1dd0f5229--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2cc1dd0f5229--------------------------------) [Carlos Mougan](https://medium.com/@carmougan?source=post_page-----2cc1dd0f5229--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd5344df58d03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-fairness-implications-of-encoding-categorical-protected-attributes-2cc1dd0f5229&user=Carlos+Mougan&userId=d5344df58d03&source=post_page-d5344df58d03----2cc1dd0f5229---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2cc1dd0f5229--------------------------------) ·7分钟阅读·2023年5月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2cc1dd0f5229&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-fairness-implications-of-encoding-categorical-protected-attributes-2cc1dd0f5229&user=Carlos+Mougan&userId=d5344df58d03&source=-----2cc1dd0f5229---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2cc1dd0f5229&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-fairness-implications-of-encoding-categorical-protected-attributes-2cc1dd0f5229&source=-----2cc1dd0f5229---------------------bookmark_footer-----------)![](../Images/b3e4278cc0dbc42f6be7eb77d99a9e0c.png)

曙光中的公正女神来自[Unsplash](https://www.istockphoto.com/es/foto/lady-justice-al-amanecer-gm1318486133-405603913)

我们将探讨分类属性编码的世界及其对机器学习模型在准确性和公平性方面的影响。分类属性，如出生国家或族裔，在确定数据中敏感信息的存在方面起着关键作用。然而，许多机器学习算法难以直接处理分类属性，因此需要使用编码方法将其转换为模型可以利用的数值特征。此外，我们还研究了交叉公平性工程的影响。

本博客包含了我们**AI、伦理和社会（AIES'23）**会议论文**编码保护分类属性的公平性影响**的总结，[[link](https://arxiv.org/abs/2201.11358)]。

***总结：*** *编码保护属性提高了模型性能，但也增加了公平性违背。这可以通过目标编码正则化来调和。工程化交叉保护属性（性别-种族）提高了性能，但大幅增加了公平性违背。*

# 引言

敏感属性在公平性中至关重要，其处理贯穿整个机器学习流程。许多机器学习算法要求将分类属性适当地编码为数值数据，然后再输入到算法中。

***编码分类保护属性的影响是什么？***

**诱导偏差的类型**

我们研究了这些编码方法可能引发的两种偏差：不可减少的偏差和可减少的偏差：

+   **不可减少的偏差：** 这是指由于将群体分类到标签中而产生的（直接）群体歧视：关于比较群体的更多数据无法减少这种类型的偏差。在 COMPAS 数据集中，犯罪分子的族裔在确定再犯评分时至关重要；对像*非洲裔美国人*或*白人*这样的大族裔群体的数值编码可能导致歧视，这是一种源于不可减少偏差的不公平效应。

+   **可减少的偏差：** 由于对具有小统计代表性的群体进行编码时的方差而产生，有时甚至只有非常少的实例。当对族裔类别*阿拉伯裔*进行编码时，常常会引发较大的采样方差，最终导致几乎是随机和不现实的编码，从而发现和引入可减少的偏差。

## **编码方法**

**独热编码：** 这是分类特征中最成熟的编码方法，也是公平性文献中的默认方法。

![](../Images/bab53dcac4ac9357a9736db6654c298c.png)

One Hot Encoding 的示例。图片来源：作者

+   **目标编码：** 分类特征被替换为各自类别的均值目标值。这种技术处理高基数分类数据，并对类别进行排序。目标编码的主要缺点出现在样本较少的类别被替换为接近期望目标的值时。这会引入模型偏差，因为它过度信任目标编码特征，并使其容易过拟合和减少偏差。

此外，这种类型的编码允许正则化。在本文中，我们研究了两种类型的正则化（在博客中我们只研究了高斯正则化）

![](../Images/a0d53d7dcb08e053827801e160c5d4c5.png)

目标编码示例说明。作者提供的图像

# 实验

对于预测性能，我们使用AUC和公平性指标。参考组（r）与我们想要比较的组（i）之间。Z是受保护的属性，\hat{Y}是模型预测。

人口统计学平等

![](../Images/641e5d5a310c41356bac49d93c2bbb2c.png)

不受特权组与受特权组获得的有利结果之间的差异

平等机会公平性

![](../Images/ac4c65829d26fc3c5bd1c57fc0cf8f3a.png)

确保公平机会而非原始结果。该值是

受保护组与

参考组

平均绝对赔率

![](../Images/4e06f1f4f8272733447eae1d2ca29488.png)

不受特权组与受特权组的真实正例率和假正例率之间绝对差异的总和

**实验假设**

对受保护属性的编码

1.  提高性能

1.  增加了公平性违反

1.  性能和公平性可以通过目标编码正则化来调和。

![](../Images/b618cc20c6b8c700b2e9dac280d95de9.png)

图：比较OHE和目标编码正则化（高斯噪声）在COMPAS数据集上的逻辑回归。红点表示不同的正则化参数：红色越深，正则化越高。蓝点表示独热编码。作者提供的图像。

![](../Images/a7f34b2884ac148042ee7ae252702788.png)

高斯噪声正则化参数$\lambda$对COMPAS数据集测试集上的性能和公平性指标的影响，使用L1惩罚的逻辑回归。在左侧图像中，所有受保护组的AUC都超过了正则化超参数。在右侧，平等机会公平性、人口统计学平等和平均绝对赔率在正则化超参数中的变化。作者提供的图像

在实验中，我们展示了公平机器学习文献中最常用的分类编码方法——独热编码，在平等机会公平性方面比目标编码更具歧视性。然而，目标编码表现出了良好的结果。使用高斯正则化的目标编码在存在两种偏见时显示了改善，但在过度参数化的情况下风险会显著丧失模型性能。

## 交叉公正性

在机器学习中追求公平时，必须认识到属性的复杂相互作用及其对歧视的影响。本节将**深入探讨**分类属性编码对交叉公正性的影响，重点关注从COMPAS数据集中获得的见解。我们提出了有关通过属性工程可能导致公正性下降的假设、分类编码可能增加歧视的倾向以及正则化技术在缓解交叉偏见中的有效性。

为了探讨编码对交叉公正性的影响，我们分析了COMPAS数据集中连接的“民族”和“婚姻状况”属性。通过选择“白人已婚”组作为参考，我们比较了所有组的最大公平性违规。为了便于理解，我们利用了上一节的广义线性模型，并强调平等机会公平性指标，这与其他公平性指标的行为一致。

![](../Images/8d1f54272e82a0c62bbef0e8edcdd335.png)

编码分类保护属性的平等机会公平性及其在Compas数据集上的正则化效果。水平线是保护属性未包含在训练数据中的基线。正则化目标编码不会损害公平性指标，但可以提高该数据集的预测性能。图片由作者提供。

上图直观地展示了属性连接如何创建交叉属性并加剧公平性违规，为我们的第一个假设提供了实证支持。值得注意的是，即使在未编码保护属性的情况下（由水平线表示），最大公平性违规也显著增加，从“民族”0.015或“婚姻状况”0.08增加到交叉属性的0.16。这一发现证实了金伯莉·克伦肖1958年的开创性工作，揭示了不同形式的压迫如何交织在一起并加剧对边缘化群体的歧视。

此外，我们的第二个假设得到了验证，即观察到这两种编码技术导致的平等机会违反率高于不编码受保护属性的情况。这突显了编码在放大歧视方面的作用。然而，仍有一线希望：通过目标编码的正则化，可以提升公平性。这个结果与我们的理论理解一致，因为属性连接可能会通过增加不可减少偏差和可减少偏差来恶化公平性。

# 结论

我们的研究强调了类别属性编码在平衡机器学习模型的准确性和公平性方面的重要作用。我们已识别出编码类别属性时可能出现的两种偏差：不可减少偏差和可减少偏差。

通过理论分析和实证实验，我们发现独热编码（one-hot encoding）表现出比目标编码（target encoding）更多的歧视。然而，经过正则化的目标编码表现出有希望的结果，它在保持可接受的模型性能的同时，展现了提高公平性的潜力。

我们强调了考虑编码类别受保护属性的影响的重要性，因为对编码方法进行轻微修改可以在不显著牺牲预测准确性的情况下实现公平性改善。

# **潜台词**

近年来，我们看到许多算法方法从数据收集、预处理、处理过程中以及后处理步骤等多个角度来提高数据驱动系统中的公平性。在这项工作中，我们集中研究了如何通过编码类别属性（一个常见的预处理步骤）来调和模型质量和公平性。

许多公平机器学习工作的一个共同基础假设是，公平性与准确性之间的权衡可能需要复杂的方法或困难的政策选择 [[Rodolfa et al.](https://www.nature.com/articles/s42256-021-00396-x)]

由于正则化的目标编码易于执行且不需要对机器学习模型进行重大更改，因此它可以作为公平机器学习中处理方法的合适补充，值得在未来进一步探索。

我们鼓励行业从业者考虑编码类别受保护属性的影响。通过对编码方法进行轻微调整，可以在不显著影响预测性能的情况下实现公平性改善。然而，必须理解的是，使用公平人工智能方法并不保证复杂社会技术系统的公平性。

**限制**：本工作旨在展示在任何时刻对分类保护属性进行编码的一些影响。应理解为，在任何情况下对分类保护属性的编码不会增加公平性指标；我们提倡在公平性轴上考虑编码正则化的效果，而不仅仅是在预测性能轴上。Fair-AI 方法并不一定保证基于 AI 的复杂社会技术系统的公平性。

## **致谢**

*本工作得到了欧洲联盟 Horizon 2020 研究与创新计划资助，资助项目为 Marie Sklodowska-Curie Actions（资助协议编号 860630），项目名称为‘’NoBIAS — Artificial Intelligence without Bias’’*

## **免责声明**

*本工作仅反映作者的观点，欧洲研究执行局（REA）对其包含的信息的任何使用不承担责任。*

## 引用

```py
@inproceedings{fairEncoder,
author = {Mougan, Carlos and Alvarez, Jose M. and Ruggieri, Salvatore and Staab, Steffen},
title = {Fairness Implications of Encoding Protected Categorical Attributes},
year = {2023},
url = {https://arxiv.org/abs/2201.11358},
booktitle = {Proceedings of the 2023 AAAI/ACM Conference on AI, Ethics, and Society},
location = {Montreal, Canada},
series = {AIES '23}
}
```
