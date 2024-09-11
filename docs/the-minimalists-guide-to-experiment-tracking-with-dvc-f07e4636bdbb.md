# 使用 DVC 进行实验跟踪的极简指南

> 原文：[https://towardsdatascience.com/the-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb?source=collection_archive---------14-----------------------#2023-05-15](https://towardsdatascience.com/the-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb?source=collection_archive---------14-----------------------#2023-05-15)

![](../Images/40caa436b9fce122c4b24ed80894039f.png)

图片由 Zarak Khan 提供，来源于 [pexels.com](https://www.pexels.com/photo/a-minimalist-workspace-4256211/)

## 实验跟踪的最基本指南

[](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)[![Eryk Lewinson](../Images/56e09e19c0bbfecc582da58761d15078.png)](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----f07e4636bdbb---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------) ·6分钟阅读·2023年5月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff07e4636bdbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----f07e4636bdbb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff07e4636bdbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb&source=-----f07e4636bdbb---------------------bookmark_footer-----------)

这篇文章是一个系列的第三部分，展示了如何利用DVC及其VS Code扩展进行ML实验。在[第一部分](/turn-vs-code-into-a-one-stop-shop-for-ml-experiments-49c97c47db27)中，我介绍了ML项目的完整设置，并演示了如何在VS Code中跟踪和评估实验。在[第二部分](/enhance-your-ml-experimentation-workflow-with-real-time-plots-434106b1a1c2)中，我展示了如何使用不同类型的图表，包括实时图表，来进行实验评估。

阅读这些文章后，你可能对在下一个项目中使用DVC感兴趣。然而，你可能会认为设置它需要大量工作，例如定义管道和数据版本控制。也许对于下一个快速实验，这会显得过于繁琐，你决定不去尝试。这将是个遗憾！

尽管所有这些步骤都有很好的理由——你的项目将被完全跟踪和可复现——但我理解有时我们面临很大压力，需要快速实验和迭代。这就是为什么在这篇文章中，我将向你展示开始使用DVC跟踪实验所需的最基本内容。

# 从零开始，用几行代码进行实验跟踪
