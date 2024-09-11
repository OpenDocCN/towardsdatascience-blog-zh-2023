# V-Net，U-Net在图像分割中的大哥

> 原文：[https://towardsdatascience.com/v-net-u-nets-big-brother-in-image-segmentation-906e393968f7?source=collection_archive---------4-----------------------#2023-07-28](https://towardsdatascience.com/v-net-u-nets-big-brother-in-image-segmentation-906e393968f7?source=collection_archive---------4-----------------------#2023-07-28)

## 欢迎阅读关于V-Net的指南，V-Net是著名的U-Net在3D图像分割中的“表亲”。你将彻底了解它！

[](https://medium.com/@francoisporcher?source=post_page-----906e393968f7--------------------------------)[![François Porcher](../Images/9ddb233f8cadbd69026bd79e2bd62dea.png)](https://medium.com/@francoisporcher?source=post_page-----906e393968f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----906e393968f7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----906e393968f7--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----906e393968f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e8e73046f53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fv-net-u-nets-big-brother-in-image-segmentation-906e393968f7&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=post_page-8e8e73046f53----906e393968f7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----906e393968f7--------------------------------) ·8分钟阅读·2023年7月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F906e393968f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fv-net-u-nets-big-brother-in-image-segmentation-906e393968f7&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=-----906e393968f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F906e393968f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fv-net-u-nets-big-brother-in-image-segmentation-906e393968f7&source=-----906e393968f7---------------------bookmark_footer-----------)

欢迎进入深度学习架构的激动人心的旅程！你可能已经熟悉了[U-Net](https://medium.com/@foporcher/cooking-your-first-u-net-for-image-segmentation-e812e37e9cd0)，这在计算机视觉领域是一个游戏规则的改变者，显著重塑了图像分割的格局。

今天，让我们将焦点转向U-Net的大哥——V-Net。

由 Fausto Milletari、Nassir Navab 和 Seyed-Ahmad Ahmadi 发表的论文[“VNet: Fully Convolutional Neural Networks for Volumetric Medical Image Segmentation”](https://arxiv.org/abs/1606.04797)介绍了一种突破性的 3D 图像分析方法。

这篇文章将带你参观这篇开创性的论文，揭示其独特的贡献和建筑进展。不论你是经验丰富的数据科学家、崭露头角的 AI 爱好者，还是对最新科技趋势感兴趣的普通人，这里都有适合你的内容！

# 关于 U-Net 的简短提醒

在深入了解 V-Net 的核心之前，让我们先花一点时间欣赏一下它的建筑灵感——U-Net。如果这是你第一次接触 U-Net，不用担心；我为你准备了一个[简明易懂的 U-Net 架构教程](https://medium.com/@foporcher/cooking-your-first-u-net-for-image-segmentation-e812e37e9cd0)。教程简洁到你在五分钟内就能掌握这个概念！
