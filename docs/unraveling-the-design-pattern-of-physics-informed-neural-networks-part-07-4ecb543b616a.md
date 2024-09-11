# 揭示物理信息神经网络的设计模式：第07部分

> 原文：[https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a?source=collection_archive---------20-----------------------#2023-07-25](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a?source=collection_archive---------20-----------------------#2023-07-25)

## 高效训练参数化PINN的主动学习

[](https://shuaiguo.medium.com/?source=post_page-----4ecb543b616a--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----4ecb543b616a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ecb543b616a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4ecb543b616a--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----4ecb543b616a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----4ecb543b616a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ecb543b616a--------------------------------) ·8分钟阅读·2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4ecb543b616a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----4ecb543b616a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4ecb543b616a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a&source=-----4ecb543b616a---------------------bookmark_footer-----------)![](../Images/65bd5d8fceb65306815ee058fe60f42d.png)

图片来源：[Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到本系列的第七篇博客文章，我们将继续激动人心的探索物理信息神经网络（PINN）的***设计模式***之旅🙌

在这篇博客中，我们将更深入地探讨一篇将**主动学习**引入PINN的论文。像往常一样，我们将通过设计模式的视角来审视这篇论文：我们将从目标问题开始，然后介绍提出的方法。之后，我们将讨论评估程序及提出方法的优缺点。最后，我们将通过探索未来的机会来结束这篇博客。

> 随着本系列的不断扩展，PINN设计模式的集合变得更加丰富！以下是一些即将到来的内容的预览：
> 
> [PINN设计模式01：优化残差点分布](/unraveling-the-design-pattern-of-physics-informed-neural-networks-series-01-8190df459527)
> 
> [PINN设计模式02：动态解区间扩展](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791)
> 
> [PINN设计模式03：使用梯度提升训练PINN](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9)
> 
> [PINN设计模式04：增强梯度的PINN学习](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde)
> 
> [PINN设计模式05：自动化超参数调整](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23)
> 
> [PINN设计模式](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2)…
