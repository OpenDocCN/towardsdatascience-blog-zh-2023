# U-Net 解析：了解其图像分割架构

> 原文：[https://towardsdatascience.com/u-net-explained-understanding-its-image-segmentation-architecture-56e4842e313a?source=collection_archive---------1-----------------------#2023-03-08](https://towardsdatascience.com/u-net-explained-understanding-its-image-segmentation-architecture-56e4842e313a?source=collection_archive---------1-----------------------#2023-03-08)

## 跳跃连接如何让 CNN 在较少数据下进行准确的语义分割

[](https://conorosullyds.medium.com/?source=post_page-----56e4842e313a--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----56e4842e313a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56e4842e313a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56e4842e313a--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----56e4842e313a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fu-net-explained-understanding-its-image-segmentation-architecture-56e4842e313a&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----56e4842e313a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56e4842e313a--------------------------------) ·7 min read·Mar 8, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56e4842e313a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fu-net-explained-understanding-its-image-segmentation-architecture-56e4842e313a&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----56e4842e313a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56e4842e313a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fu-net-explained-understanding-its-image-segmentation-architecture-56e4842e313a&source=-----56e4842e313a---------------------bookmark_footer-----------)![](../Images/a5de3c10ab618dd3835101b13560d988.png)

(source: author)

U-Net 是一种流行的深度学习架构，用于语义分割。最初为医学图像开发，在该领域取得了巨大成功。但这仅仅是个开始！从卫星图像到手写字符，该架构在各种数据类型上的性能都有所提升。然而，其他 CNN 架构也可以进行分割，那么 U-Net 到底有什么特别之处呢？

为了回答这个问题，我们将深入探讨 U-Net 架构。我们将其与用于分类的 CNN 和自动编码器进行比较。这样，我们将理解 **跳跃连接** 是 U-Net 成功的关键。我们将看到它们如何使架构在数据较少的情况下进行准确的分割。

# 什么是语义分割？

我们将从了解 U-Net 的开发目的开始。**图像分割** 或 **语义分割** 是将每个像素分配到图像中的一个类别的任务。模型使用分割图作为目标变量进行训练。例如，参见图 1。我们有原始图像和一个二值分割图。该图将图像分为细胞像素和非细胞像素。
