# 为什么我们甚至需要神经网络？

> 原文：[https://towardsdatascience.com/why-do-we-even-have-neural-networks-72410cb9348e?source=collection_archive---------5-----------------------#2023-12-16](https://towardsdatascience.com/why-do-we-even-have-neural-networks-72410cb9348e?source=collection_archive---------5-----------------------#2023-12-16)

## 神经网络的替代方案：泰勒级数与傅里叶级数

[](https://medium.com/@egorhowell?source=post_page-----72410cb9348e--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----72410cb9348e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72410cb9348e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----72410cb9348e--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----72410cb9348e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-even-have-neural-networks-72410cb9348e&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----72410cb9348e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72410cb9348e--------------------------------) ·7分钟阅读·2023年12月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72410cb9348e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-even-have-neural-networks-72410cb9348e&user=Egor+Howell&userId=1cac491223b2&source=-----72410cb9348e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72410cb9348e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-do-we-even-have-neural-networks-72410cb9348e&source=-----72410cb9348e---------------------bookmark_footer-----------)![](../Images/25f1853ad21413f753796df43b84dfb0.png)

”[https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network)。”标题为“神经网络图标” 神经网络图标由imaginationlol创建 — Flaticon。

最近我一直在撰写一系列文章，解释现代神经网络的关键概念：

![Egor Howell](../Images/e969a9f3c3357e1c80dcd0092d9a1288.png)

[Egor Howell](https://medium.com/@egorhowell?source=post_page-----72410cb9348e--------------------------------)

## 神经网络

[查看列表](https://medium.com/@egorhowell/list/neural-networks-616db722dbbb?source=post_page-----72410cb9348e--------------------------------)9个故事![](../Images/66f86f14ddf20d9da9cac53dee54bae3.png)![](../Images/d968b0bd4358bb0d60bd296aef08a720.png)![](../Images/30f3f4354836e6d3d56175fe20cf6b7c.png)

神经网络如此强大和受欢迎的一个原因是它们展示了[***通用逼近定理***](https://en.wikipedia.org/wiki/Universal_approximation_theorem)***。*** 这意味着神经网络可以“学习”任何函数，无论其复杂程度如何。

> [***“函数描述世界。”***](https://www.youtube.com/watch?v=PAZTIAfaNr8)

一个函数，***f(x)***，接受某些输入，***x***，并给出一个输出***y***：

![](../Images/9c132aa6dab2a0b9da872eb5ead558c9.png)

数学函数如何工作的示意图，由作者绘制。

这个函数定义了输入和输出之间的关系。在大多数情况下，我们拥有输入和相应的输出，神经网络的目标是学习或逼近将它们映射之间的函数。
