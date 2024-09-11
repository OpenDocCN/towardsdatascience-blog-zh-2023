# 揭示物理信息神经网络的设计模式：第五部分

> 原文：[https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23?source=collection_archive---------8-----------------------#2023-06-04](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23?source=collection_archive---------8-----------------------#2023-06-04)

## 利用自动化超参数优化来提高物理信息神经网络（PINN）的效果

[](https://shuaiguo.medium.com/?source=post_page-----67a35a984b23--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----67a35a984b23--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67a35a984b23--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----67a35a984b23--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----67a35a984b23--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----67a35a984b23---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67a35a984b23--------------------------------) · 9分钟阅读 · 2023年6月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67a35a984b23&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----67a35a984b23---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67a35a984b23&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23&source=-----67a35a984b23---------------------bookmark_footer-----------)![](../Images/33b724900a8fa118517e377c2e46f3fb.png)

图片来源：[Drew Patrick Miller](https://unsplash.com/@drewpatrickmiller?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到本系列的第五篇博客，在这里我们继续激动人心的探索***物理信息神经网络***（PINN）的设计模式🙌

你是否曾想过是否可以自动搜索出最适合物理信息神经网络的架构？事实证明，正如我们今天将要讨论的论文所指出的那样，确实存在这种方法。

和往常一样，我们将首先讨论当前的问题，然后介绍提出的解决方案、基准测试过程，以及所提技术的优缺点。最后，我们将探讨一些潜在的未来机会。

> 如果你对本系列中其他 PINN 设计模式感兴趣，可以在这里了解更多：
> 
> [PINN 设计模式 01：优化残差点分布](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-series-01-8190df459527)
> 
> [PINN 设计模式 02：动态解决方案区间扩展](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791)
> 
> [PINN 设计模式 03：使用梯度提升训练 PINN](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9)
> 
> [PINN 设计模式 04：梯度增强的 PINN 学习](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde)
> 
> [PINN 设计模式 06：因果关系](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2)…
