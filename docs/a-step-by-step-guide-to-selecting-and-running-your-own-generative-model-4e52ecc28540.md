# 选择和运行自己的生成模型的逐步指南

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540?source=collection_archive---------11-----------------------#2023-10-07](https://towardsdatascience.com/a-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540?source=collection_archive---------11-----------------------#2023-10-07)

## 如何在每天发布的众多模型中进行导航？

[](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)[![Kevin Berlemont, PhD](../Images/18697f38b76f1fb04870f565cfb04b4c.png)](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------) [Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3dea771eb493&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=post_page-3dea771eb493----4e52ecc28540---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------) ·5分钟阅读·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e52ecc28540&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=-----4e52ecc28540---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e52ecc28540&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540&source=-----4e52ecc28540---------------------bookmark_footer-----------)![](../Images/57dcd30f489d1c312588752fbfe2e016.png)

由 [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 提供，照片来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

过去几个月，各种生成模型的参数规模大幅减少，例如新推出的Mistral AI模型。尺寸的减少为在本地计算机上启用个人助理AI提供了可能性。这种本地推断非常诱人，可以确保对数据的保密计算。随着这些新发展的出现，部署和管理AI工作负载的方式与6个月前大相径庭，并且不断演变。如何使用这些模型进行实验，甚至将其托管在公司的基础设施上呢？

我认为，在使用任何由他人托管的API模型之前，尝试不同类型的模型以了解这些不同模型家族的表现是件好事。所以，假设你不会立即使用API模型。你该如何下载模型并使用它呢？

为此，你有两种类型的模型：专有模型和开放访问模型。专有模型包括OpenAI、Cohere等，它们都有自己的API。开放访问模型可以是完全开放的，也可以是半限制的模型，原因是…
