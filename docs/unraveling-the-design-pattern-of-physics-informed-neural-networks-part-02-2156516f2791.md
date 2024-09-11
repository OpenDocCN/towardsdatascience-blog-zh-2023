# 物理信息神经网络设计模式揭示：第二部分

> 原文：[https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791?source=collection_archive---------7-----------------------#2023-05-19](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791?source=collection_archive---------7-----------------------#2023-05-19)

## 通过集成学习和动态解决方案区间扩展提升PINN训练稳定性

[](https://shuaiguo.medium.com/?source=post_page-----2156516f2791--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----2156516f2791--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2156516f2791--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2156516f2791--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----2156516f2791--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----2156516f2791---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2156516f2791--------------------------------) ·7分钟阅读·2023年5月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2156516f2791&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----2156516f2791---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2156516f2791&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791&source=-----2156516f2791---------------------bookmark_footer-----------)![](../Images/96f9d77f92cae57a4a95d217344c3fdd.png)

图片来源于 [Clay Banks](https://unsplash.com/de/@claybanks?utm_source=medium&utm_medium=referral) 的 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎阅读本系列第二篇关于物理信息神经网络（PINN）设计模式的博客🙌 在这里，我们将深入探讨**集成学习**和**动态解决方案区间扩展**技术，以增强PINN训练的稳定性和准确性。

展望未来，我们将从提出的策略试图解决的具体问题开始，然后详细说明所提出的策略、如何实施以及其可能奏效的原因。之后，我们将审视所基准化的物理问题，以及所提方法的优缺点。最后，我们讨论该方法的替代方案和未来机会。

> 随着这一系列的不断扩展，PINN 设计模式的集合变得更加丰富*🙌* 这里有一个即将到来的内容预告：
> 
> [PINN 设计模式 01：优化残差点分布](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-series-01-8190df459527)
> 
> [PINN 设计模式 03：使用梯度提升训练 PINN](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9)
> 
> [PINN 设计模式 04：梯度增强的 PINN 学习](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde)
> 
> [PINN 设计模式 05：自动化](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23)…
