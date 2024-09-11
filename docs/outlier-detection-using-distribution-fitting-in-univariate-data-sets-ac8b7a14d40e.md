# 在单变量数据集中使用分布拟合进行离群点检测

> 原文：[https://towardsdatascience.com/outlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e?source=collection_archive---------2-----------------------#2023-02-18](https://towardsdatascience.com/outlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e?source=collection_archive---------2-----------------------#2023-02-18)

## 学习如何使用概率密度函数检测离群点，以获得快速、轻量的模型和可解释的结果。

[](https://erdogant.medium.com/?source=post_page-----ac8b7a14d40e--------------------------------)[![Erdogan Taskesen](../Images/8e62cdae0502687710d8ae4bbcd8966e.png)](https://erdogant.medium.com/?source=post_page-----ac8b7a14d40e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ac8b7a14d40e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ac8b7a14d40e--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----ac8b7a14d40e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----ac8b7a14d40e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac8b7a14d40e--------------------------------) ·16分钟阅读·2023年2月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fac8b7a14d40e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----ac8b7a14d40e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fac8b7a14d40e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e&source=-----ac8b7a14d40e---------------------bookmark_footer-----------)![](../Images/b8f7a7adb294d55c78fd3b0f70d41809.png)

图片由 [Randy Fath](https://unsplash.com/es/@randyfath?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/G1yhU1Ej-9A?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

异常或新颖性检测适用于需要明确、提前警告异常情况的广泛场景，如传感器数据、安全操作和欺诈检测等。由于问题的性质，异常值并不会频繁出现，并且由于缺乏标签，创建监督模型可能变得困难。异常值也被称为异常或新颖性，但在基础假设和建模过程中存在一些根本的区别。***在这里，我将讨论异常和新颖性之间的基本区别以及异常检测的概念。通过一个实践示例，我将演示如何使用概率密度拟合为单变量数据集创建无监督模型以检测异常和新颖性。*** *distfit库在所有示例中均有使用。*

*如果你觉得这篇文章有帮助，可以使用我的* [*推荐链接*](https://medium.com/@erdogant/membership) *继续无限制地学习，并注册Medium会员。此外，* [*关注我*](http://erdogant.medium.com) *以便随时获取我的最新内容！*

# 异常还是新颖性？
