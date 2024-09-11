# 与 Hugging Face 数据集的工作

> 原文：[https://towardsdatascience.com/working-with-hugging-face-datasets-bba14dd8da68?source=collection_archive---------9-----------------------#2023-06-29](https://towardsdatascience.com/working-with-hugging-face-datasets-bba14dd8da68?source=collection_archive---------9-----------------------#2023-06-29)

## 学习如何访问 Hugging Face Hub 上的数据集，以及如何使用 DuckDB 和 Datasets 库远程加载它们

[](https://weimenglee.medium.com/?source=post_page-----bba14dd8da68--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----bba14dd8da68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bba14dd8da68--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bba14dd8da68--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----bba14dd8da68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-hugging-face-datasets-bba14dd8da68&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----bba14dd8da68---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bba14dd8da68--------------------------------) · 13分钟阅读 · 2023年6月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbba14dd8da68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-hugging-face-datasets-bba14dd8da68&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----bba14dd8da68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbba14dd8da68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-hugging-face-datasets-bba14dd8da68&source=-----bba14dd8da68---------------------bookmark_footer-----------)![](../Images/8b1abe24851b072185f9290f0bf2b4de.png)

照片由 [Lars Kienle](https://unsplash.com/@larskienle?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为一个人工智能平台，Hugging Face 构建、训练并部署最先进的开源机器学习模型。除了托管这些训练好的模型，Hugging Face 还托管数据集 ([https://huggingface.co/datasets](https://huggingface.co/datasets))，你可以将它们用于自己的项目。

在这篇文章中，我将展示如何访问 Hugging Face 上的数据集，并且如何将这些数据集以编程方式下载到本地计算机。具体来说，我将展示如何：

+   使用 DuckDB 对 **httpfs** 的支持远程加载数据集

+   使用 Hugging Face 的 **Datasets** 库流式处理数据集

# Hugging Face Datasets server

**Hugging Face Datasets server** 是一个轻量级的网页 API，用于可视化存储在 Hugging Face Hub 上的各种数据集。你可以使用提供的 REST API 查询存储在 Hugging Face Hub 上的数据集。以下部分提供了关于如何使用 API 的简短教程，网址为 `[https://datasets-server.huggingface.co/](https://datasets-server.huggingface.co/)`。

## 获取数据集列表…
