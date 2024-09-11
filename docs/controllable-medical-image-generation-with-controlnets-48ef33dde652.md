# 使用 ControlNets 进行可控的医学图像生成

> 原文：[https://towardsdatascience.com/controllable-medical-image-generation-with-controlnets-48ef33dde652?source=collection_archive---------6-----------------------#2023-06-13](https://towardsdatascience.com/controllable-medical-image-generation-with-controlnets-48ef33dde652?source=collection_archive---------6-----------------------#2023-06-13)

## 使用 ControlNets 控制潜在扩散模型的生成过程

[](https://medium.com/@walhugolp?source=post_page-----48ef33dde652--------------------------------)[![Walter Hugo Lopez Pinaya](../Images/0c132d0d1321790b0cea880800d231e0.png)](https://medium.com/@walhugolp?source=post_page-----48ef33dde652--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48ef33dde652--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----48ef33dde652--------------------------------) [Walter Hugo Lopez Pinaya](https://medium.com/@walhugolp?source=post_page-----48ef33dde652--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa1dadbc02295&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontrollable-medical-image-generation-with-controlnets-48ef33dde652&user=Walter+Hugo+Lopez+Pinaya&userId=a1dadbc02295&source=post_page-a1dadbc02295----48ef33dde652---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----48ef33dde652--------------------------------) ·9 min read·2023年6月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F48ef33dde652&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontrollable-medical-image-generation-with-controlnets-48ef33dde652&user=Walter+Hugo+Lopez+Pinaya&userId=a1dadbc02295&source=-----48ef33dde652---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F48ef33dde652&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontrollable-medical-image-generation-with-controlnets-48ef33dde652&source=-----48ef33dde652---------------------bookmark_footer-----------)

在这篇文章中，我们将介绍如何训练一个**ControlNet**，以使用户能够对**潜在扩散模型**（**例如 Stable Diffusion!**）的生成过程进行精确控制。我们的目标是展示这些模型在不同对比度脑部图像转换中的卓越能力。为此，我们将利用最近推出的**MONAI开源扩展**，[**MONAI Generative Models**](https://github.com/Project-MONAI/GenerativeModels)**!**

![](../Images/802d4dc3c60ef6342ff0aa9478c0bad4.png)

使用 ControlNet 从 FLAIR 图像（左）生成 T1 加权脑图像（右）

***我们的项目代码可在这个公共代码库中获取*** [***https://github.com/Warvito/generative_brain_controlnet***](https://github.com/Warvito/generative_brain_controlnet)

# 介绍

近年来，文本到图像的扩散模型取得了显著进展，使得根据开放领域的文本描述生成高度真实的图像成为可能。这些生成的图像具有丰富的细节、轮廓清晰、结构连贯和有意义的上下文表现。然而，尽管扩散模型取得了显著的成就，实现对生成过程的精确控制仍然是一个挑战。**即使有详细复杂的文本描述，准确捕捉用户期望的想法也可能很困难。**
