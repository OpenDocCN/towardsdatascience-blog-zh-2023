# 提高物理信息神经网络（PINNs）的10个有用提示和技巧

> 原文：[https://towardsdatascience.com/10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001?source=collection_archive---------2-----------------------#2023-02-08](https://towardsdatascience.com/10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001?source=collection_archive---------2-----------------------#2023-02-08)

## 一些有关采样、架构、激活函数、损失平衡、优化器、数据归一化等方面的技巧。

[](https://rabischof.medium.com/?source=post_page-----1a5dd7b86001--------------------------------)[![Rafael Bischof](../Images/a1d468ea5b61c26a18541f0c0f42c5c6.png)](https://rabischof.medium.com/?source=post_page-----1a5dd7b86001--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a5dd7b86001--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1a5dd7b86001--------------------------------) [Rafael Bischof](https://rabischof.medium.com/?source=post_page-----1a5dd7b86001--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F913c6c1e6a94&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001&user=Rafael+Bischof&userId=913c6c1e6a94&source=post_page-913c6c1e6a94----1a5dd7b86001---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a5dd7b86001--------------------------------) ·10 min read·2023年2月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a5dd7b86001&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001&user=Rafael+Bischof&userId=913c6c1e6a94&source=-----1a5dd7b86001---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a5dd7b86001&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-useful-hints-and-tricks-for-improving-pinns-1a5dd7b86001&source=-----1a5dd7b86001---------------------bookmark_footer-----------)![](../Images/5933c39bf996463c3efde583f3d27cb1.png)

图片由 [vackground.com](https://unsplash.com/@vackground?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[物理信息神经网络（PINNs）](https://www.sciencedirect.com/science/article/pii/S0021999118307125) [1] 近年来因其作为解决偏微分方程（PDEs）的连续、完全可微模型而受到关注。它们能够解决工程、科学、金融等多个领域的复杂问题。但正如机器学习的任何领域一样，存在令人吃惊的挑战，目前文献中尚无答案。在我与PINNs的工作中，我遇到过各种障碍，并开发了一套提示和技巧，以帮助提升其性能。

虽然这些技巧并不详尽，但它们基于在多样化问题上的经验观察，并且一贯地改善了性能。我想与您分享这些见解，以帮助您在PINNs的旅程中。这本指南希望能为您提供实用建议，帮助您改进模型并取得更好的结果。

如往常一样，这些文章附带有笔记本，您可以直接尝试…
