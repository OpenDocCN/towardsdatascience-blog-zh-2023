# **扩散概率模型**和**文本到图像生成**

> 原文：[https://towardsdatascience.com/diffusion-probabilistic-models-and-text-to-image-generation-9f441d0bc786?source=collection_archive---------16-----------------------#2023-03-29](https://towardsdatascience.com/diffusion-probabilistic-models-and-text-to-image-generation-9f441d0bc786?source=collection_archive---------16-----------------------#2023-03-29)

## 你能想到的任何东西的逼真生成

[](https://taying-cheng.medium.com/?source=post_page-----9f441d0bc786--------------------------------)[![Tim Cheng](../Images/d15f96a7c415f1d8348ee084af39bc66.png)](https://taying-cheng.medium.com/?source=post_page-----9f441d0bc786--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f441d0bc786--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f441d0bc786--------------------------------) [Tim Cheng](https://taying-cheng.medium.com/?source=post_page-----9f441d0bc786--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff941951a4588&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiffusion-probabilistic-models-and-text-to-image-generation-9f441d0bc786&user=Tim+Cheng&userId=f941951a4588&source=post_page-f941951a4588----9f441d0bc786---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f441d0bc786--------------------------------) ·4分钟阅读·2023年3月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f441d0bc786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiffusion-probabilistic-models-and-text-to-image-generation-9f441d0bc786&user=Tim+Cheng&userId=f941951a4588&source=-----9f441d0bc786---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f441d0bc786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiffusion-probabilistic-models-and-text-to-image-generation-9f441d0bc786&source=-----9f441d0bc786---------------------bookmark_footer-----------)![](../Images/52d4c0fb80be58d4a941361526c36696.png)

图1\. 文本到图像生成。图像由作者制作。

如果你是最新计算机视觉论文的狂热追随者，你会对生成网络在创建图像方面的惊人结果感到惊讶。许多以前的文献基于开创性的生成对抗网络（GAN）理念，但最近的论文却不再如此。事实上，如果你仔细查看最新的论文，如ImageN和Staple Diffusion，你会不断看到一个陌生的术语：**扩散概率模型**。

本文深入探讨了这一新兴趋势模型的基础知识，简要概述了它的学习过程，以及随之而来的令人兴奋的应用。

# 从逐渐添加高斯噪声开始……

![](../Images/7b4ab211af6d43025fb87095464a5d2c.png)

图2\. 去噪扩散概率模型概述。图片来源：[https://arxiv.org/abs/2006.11239](https://arxiv.org/abs/2006.11239)。

设想一张图像，给它添加了一些高斯噪声。图像可能会变得有些嘈杂，但原始内容大概率仍然可以被识别。现在重复这个步骤，一遍又一遍；最终图像会变成几乎完全的高斯噪声。这被称为扩散的前向过程……
