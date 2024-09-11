# 高级维度缩减模型简明易懂

> 原文：[https://towardsdatascience.com/advanced-dimensionality-reduction-models-made-simple-639fca351528?source=collection_archive---------1-----------------------#2023-11-16](https://towardsdatascience.com/advanced-dimensionality-reduction-models-made-simple-639fca351528?source=collection_archive---------1-----------------------#2023-11-16)

## 了解如何高效地应用最先进的维度缩减方法并提升你的机器学习模型。

[](https://medium.com/@riccardo.andreoni?source=post_page-----639fca351528--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----639fca351528--------------------------------)[](https://towardsdatascience.com/?source=post_page-----639fca351528--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----639fca351528--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----639fca351528--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-dimensionality-reduction-models-made-simple-639fca351528&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----639fca351528---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----639fca351528--------------------------------) ·9分钟阅读·2023年11月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F639fca351528&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-dimensionality-reduction-models-made-simple-639fca351528&user=Riccardo+Andreoni&userId=76784541161c&source=-----639fca351528---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F639fca351528&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-dimensionality-reduction-models-made-simple-639fca351528&source=-----639fca351528---------------------bookmark_footer-----------)![](../Images/47503135e2f71ee99e299951edce437f.png)

图片来源：[unsplash.com](https://unsplash.com/photos/multicolored-wall-in-shallow-focus-photography-jbtfM0XBeRc)。

当你接近一个机器学习任务时，是否曾因**特征数量庞大**而感到震惊？

大多数数据科学家每天都会面临这种压倒性的挑战。尽管增加特征可以丰富数据，但它通常**减慢训练过程**并使**检测隐藏模式**变得更加困难，从而导致（不）著名的[**维度诅咒**](https://en.wikipedia.org/wiki/Curse_of_dimensionality)**。**

此外，在高维空间中会发生**惊人的现象**。为了用类比来描述这个概念，可以想象小说《平面国》中，生活在二维平面世界中的角色在遇到三维存在时感到震惊。同样，我们也难以理解，在高维空间中，**大多数点都是异常值**，而且**点与点之间的距离**通常**比预期的要大**。如果这些现象没有得到正确处理，可能会对我们的机器学习模型产生**灾难性的影响**。

![](../Images/1dba4905843191a4a899cc687755aa1d.png)

图片由作者提供。
