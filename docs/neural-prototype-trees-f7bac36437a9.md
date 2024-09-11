# 神经原型树

> 原文：[`towardsdatascience.com/neural-prototype-trees-f7bac36437a9?source=collection_archive---------6-----------------------#2023-06-02`](https://towardsdatascience.com/neural-prototype-trees-f7bac36437a9?source=collection_archive---------6-----------------------#2023-06-02)

## 通过模拟人类推理来解释图像分类。

[](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)![Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-prototype-trees-f7bac36437a9&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----f7bac36437a9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------) ·6 分钟阅读·2023 年 6 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff7bac36437a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-prototype-trees-f7bac36437a9&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----f7bac36437a9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff7bac36437a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-prototype-trees-f7bac36437a9&source=-----f7bac36437a9---------------------bookmark_footer-----------)

机器学习和人工智能现在被广泛应用于众多领域，但随着这种使用的增加，也带来了模型需要通过的更大风险和伦理考验。

让我们以最近的新闻为例：[一辆特斯拉在自动驾驶模式下撞上树木](https://upnorthlive.com/news/local/woman-recovering-after-tesla-crashes-in-self-driving-mode)。根据当局的说法，司机表示车辆在她将其置于自动驾驶模式后，向右偏移，驶出道路，并撞上了一棵树。目前，这些说法正在调查中，但想象一下识别车辆突然出现的异常决策背后的推理是多么困难。它是否错误分类了？它看到什么使它困惑？对于传统的黑箱模型，调查模型内部非常困难且成本高昂。

那么替代方案是什么呢？有没有一种可解释的图像分类方法？有的，通过原型学习 [2] 和神经原型树 [1]！使用这些架构，模型采用一种非常直观的预测方法：识别看起来熟悉的部分。这只鸟的喙长吗？它有红色的喉部吗？那它一定是蜂鸟！

在这篇文章中，我旨在提供这些模型如何工作的相关信息，并讨论使用这些模型的一些优点和缺点。我将经常引用的两篇主要论文如下，非常鼓励有兴趣的读者阅读：
