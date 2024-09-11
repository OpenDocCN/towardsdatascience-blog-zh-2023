# 解开物理信息神经网络的设计模式：第六部分

> 原文：[`towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2?source=collection_archive---------18-----------------------#2023-06-13`](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2?source=collection_archive---------18-----------------------#2023-06-13)

## 将因果关系引入 PINN 训练

[](https://shuaiguo.medium.com/?source=post_page-----bcb3557199e2--------------------------------)![Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----bcb3557199e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bcb3557199e2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcb3557199e2--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----bcb3557199e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----bcb3557199e2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bcb3557199e2--------------------------------) ·9 分钟阅读·2023 年 6 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbcb3557199e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----bcb3557199e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbcb3557199e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-06-bcb3557199e2&source=-----bcb3557199e2---------------------bookmark_footer-----------)![](img/0c5a39882aa4755f7434a2421e21550e.png)

图片由 [Delano Ramdas](https://unsplash.com/@delanodzr?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到这一系列的第 6 篇博客，我们将继续探索物理信息神经网络（PINN）的***设计模式***之旅🙌

在这一集里，我们将讨论将**因果关系**引入物理信息神经网络的训练中。正如我们今天将要查看的论文所建议的那样：尊重因果关系就是你所需要的全部！

和往常一样，让我们先讨论当前的问题，然后再讲解建议的解决方案、评估程序以及所提方法的优缺点。最后，我们将通过探讨潜在的机会来结束这篇博客。

> 随着这个系列的不断扩展，PINN 设计模式的集合变得更加丰富*🙌* 这里是对即将到来的内容的简要预览：
> 
> PINN 设计模式 01：优化残差点分布
> 
> PINN 设计模式 02：动态解空间扩展
> 
> [PINN 设计模式 03：使用梯度提升训练 PINN](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9)
> 
> [PINN 设计模式 04：梯度增强 PINN 学习](https://medium.com/towards-data-science/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-04-c778f4829dde)
> 
> PINN 设计模式 05：自动化超参数调优
