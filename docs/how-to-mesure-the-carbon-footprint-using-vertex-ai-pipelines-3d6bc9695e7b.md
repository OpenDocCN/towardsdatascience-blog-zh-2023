# 如何使用 Python 和 Vertex AI Pipelines 测量碳足迹

> 原文：[`towardsdatascience.com/how-to-mesure-the-carbon-footprint-using-vertex-ai-pipelines-3d6bc9695e7b?source=collection_archive---------14-----------------------#2023-01-31`](https://towardsdatascience.com/how-to-mesure-the-carbon-footprint-using-vertex-ai-pipelines-3d6bc9695e7b?source=collection_archive---------14-----------------------#2023-01-31)

## 使用 Vertex AI Pipelines 跟踪碳排放的逐步指南

[](https://medium.com/@anna.bildea?source=post_page-----3d6bc9695e7b--------------------------------)![Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----3d6bc9695e7b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d6bc9695e7b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d6bc9695e7b--------------------------------) [Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----3d6bc9695e7b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc57d3db39a47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mesure-the-carbon-footprint-using-vertex-ai-pipelines-3d6bc9695e7b&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=post_page-c57d3db39a47----3d6bc9695e7b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d6bc9695e7b--------------------------------) ·9 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d6bc9695e7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mesure-the-carbon-footprint-using-vertex-ai-pipelines-3d6bc9695e7b&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=-----3d6bc9695e7b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d6bc9695e7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mesure-the-carbon-footprint-using-vertex-ai-pipelines-3d6bc9695e7b&source=-----3d6bc9695e7b---------------------bookmark_footer-----------)![](img/6f604577434a098bfe1758e826444eb3.png)

由作者使用 Midjourney 生成的图像。

# 动机

机器学习已成为我们日常生活的一部分，因此是时候考虑它对环境的潜在影响了。否则，大自然可能会以自然灾害的形式给我们一个‘*我早就告诉过你*’的警告，导致严重的人类痛苦。我们应对气候变化的一个方法是开始测量和减少我们机器学习模型的碳足迹。碳足迹测量的是由服务、产品、个人、组织或事件引起的总温室气体排放。在机器学习的情况下，它包括训练和运行模型所需的能量，以及运行这些模型的硬件所消耗的能量。

在本文中，我将对两个开源 Python 库提供反馈，它们是 [CodeCarbon](https://pypi.org/project/codecarbon/) 和 [CarbonTracker](https://github.com/lfwa/carbontracker)，这些库可以估算碳足迹。我还将包含一个逐步指南，介绍如何在 Vertex AI 管道中使用它们。最后，我将列出减少碳足迹的实际考虑因素。所以，让我们在为时已晚之前开始为拯救地球尽一份力吧！💚

# I. Python 中的碳足迹…
