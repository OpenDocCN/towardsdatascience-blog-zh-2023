# 在一台机器上管理多个CUDA版本：综合指南

> 原文：[https://towardsdatascience.com/managing-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc?source=collection_archive---------1-----------------------#2023-10-27](https://towardsdatascience.com/managing-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc?source=collection_archive---------1-----------------------#2023-10-27)

## 如何在你的开发环境中处理不同的CUDA版本

[](https://medium.com/@chimso1994?source=post_page-----97db1b22acdc--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----97db1b22acdc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----97db1b22acdc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----97db1b22acdc--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----97db1b22acdc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----97db1b22acdc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----97db1b22acdc--------------------------------) ·6分钟阅读·2023年10月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F97db1b22acdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----97db1b22acdc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F97db1b22acdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc&source=-----97db1b22acdc---------------------bookmark_footer-----------)![](../Images/7a74ac782e86782cc59be80291bc583f.png)

[照片由Nikola Majksner](https://unsplash.com/@majksner?utm_source=medium&utm_medium=referral)拍摄，刊登于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前担任人工智能顾问的一个角色中，我被要求利用虚拟环境作为管理和隔离 Python 环境的工具。由于项目依赖于 GPU 加速，我遇到了一个情况，安装的 CUDA 版本与项目所需版本不符。为了解决这个问题，我不得不安装必要的 CUDA 版本，并配置我的环境以使用它，而不影响系统的 CUDA 设置。据我所知，缺乏全面的端到端教程来解决这个特定需求。因此，本教程对于那些希望了解如何安全管理其项目中多个 CUDA 工具包版本的人来说是一个宝贵的资源。

***目录：***

· [1\. 简介](#7e48)

· [2\. 可用的 CUDA 版本](#b45b)

· [3\. 下载和提取二进制文件](#7d61)

· [4\. 安装 CUDA 工具包](#b5bd)

· [5\. 项目设置](#e81b)

· [6\. 结论](#a709)

# 1\. 简介
