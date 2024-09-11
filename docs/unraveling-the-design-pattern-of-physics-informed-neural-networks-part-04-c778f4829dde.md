# 解密物理信息神经网络的设计模式：第04部分

> 原文：[https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde?source=collection_archive---------15-----------------------#2023-05-29](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde?source=collection_archive---------15-----------------------#2023-05-29)

## 利用梯度增强学习提高PINN训练效率

[](https://shuaiguo.medium.com/?source=post_page-----c778f4829dde--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----c778f4829dde--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c778f4829dde--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c778f4829dde--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----c778f4829dde--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----c778f4829dde---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c778f4829dde--------------------------------) ·7分钟阅读·2023年5月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc778f4829dde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----c778f4829dde---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc778f4829dde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde&source=-----c778f4829dde---------------------bookmark_footer-----------)![](../Images/e855acaf7bff2a3df94f4786a4b3f626.png)

照片由 [Hassaan Qaiser](https://unsplash.com/es/@hassaanhre?utm_source=medium&utm_medium=referral) 拍摄，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到本系列的第4篇博客，我们继续探索物理信息神经网络（PINN）的设计模式之旅🙌

在这篇博客中，我们将探讨一篇提出了新变种PINN的研究论文，这种变种被称为*梯度增强* PINN。更具体地说，我们将深入研究**问题**、**解决方案**、**基准**以及**优缺点**，以提炼论文中提出的设计模式。

> 随着这一系列的持续扩展，PINN设计模式的合集变得更加丰富*🙌* 这里是对即将到来的内容的预览：
> 
> [PINN设计模式 01：优化残差点分布](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-series-01-8190df459527)
> 
> [PINN设计模式 02：动态解决方案区间扩展](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-02-2156516f2791)
> 
> [PINN设计模式 03：使用梯度提升训练PINN](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9)
> 
> [PINN设计模式 05：自动超参数调优](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-05-67a35a984b23)
> 
> [PINN设计模式 06：因果PINN训练](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2)
> 
> [PINN设计模式 07：使用PINN进行主动学习](/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-07-4ecb543b616a)

让我们开始吧！

# 1\. 论文概览 🔍
