# 复古数据科学：测试 YOLO 的早期版本

> 原文：[`towardsdatascience.com/retro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7?source=collection_archive---------8-----------------------#2023-06-22`](https://towardsdatascience.com/retro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7?source=collection_archive---------8-----------------------#2023-06-22)

## 让我们回到 8 年前

[](https://dmitryelj.medium.com/?source=post_page-----799b9c1835d7--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----799b9c1835d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----799b9c1835d7--------------------------------)![数据科学的未来](https://towardsdatascience.com/?source=post_page-----799b9c1835d7--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----799b9c1835d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----799b9c1835d7---------------------post_header-----------) 发表在 [数据科学的未来](https://towardsdatascience.com/?source=post_page-----799b9c1835d7--------------------------------) ·8 分钟阅读·2023 年 6 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F799b9c1835d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----799b9c1835d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F799b9c1835d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7&source=-----799b9c1835d7---------------------bookmark_footer-----------)![](img/6868311449632a8f33870a1ec215e355.png)

YOLO 的对象检测，图片由作者提供

数据科学的世界不断变化。通常，我们无法看到这些变化，因为它们进展缓慢，但过一段时间后，很容易回顾并发现景象已经发生了巨大变化。曾经处于前沿的工具和库，如今可能已经完全被遗忘。

YOLO（You Only Look Once）是一个流行的目标检测库。它的第一个版本发布于很久以前，2015 年。YOLO 运行速度快，结果很好，而且预训练模型公开可用。这个模型迅速流行开来，并且这个项目至今仍在积极改进。这让我们有机会看到数据科学工具和库在这些年来是如何发展的。在本文中，我将测试不同的 YOLO 版本，从最早的 V1 到最新的 V8。

为了进一步测试，我将使用这张图片：

![](img/c80b38ffa792b0fd9df9cd624b0d83d4.png)

测试图片，由作者制作

开始吧。

## YOLO V1..V3
