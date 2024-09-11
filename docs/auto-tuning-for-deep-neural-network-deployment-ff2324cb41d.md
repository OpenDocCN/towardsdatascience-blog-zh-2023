# 深度神经网络部署的自动调优

> 原文：[https://towardsdatascience.com/auto-tuning-for-deep-neural-network-deployment-ff2324cb41d?source=collection_archive---------6-----------------------#2023-09-01](https://towardsdatascience.com/auto-tuning-for-deep-neural-network-deployment-ff2324cb41d?source=collection_archive---------6-----------------------#2023-09-01)

## 什么，为什么，以及最重要的……如何？

[](https://pecciaf.medium.com/?source=post_page-----ff2324cb41d--------------------------------)[![Federico Peccia](../Images/48ad8401c28e87717718f58336cc64cf.png)](https://pecciaf.medium.com/?source=post_page-----ff2324cb41d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff2324cb41d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ff2324cb41d--------------------------------) [Federico Peccia](https://pecciaf.medium.com/?source=post_page-----ff2324cb41d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce527bed0faf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fauto-tuning-for-deep-neural-network-deployment-ff2324cb41d&user=Federico+Peccia&userId=ce527bed0faf&source=post_page-ce527bed0faf----ff2324cb41d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff2324cb41d--------------------------------) ·7分钟阅读·2023年9月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff2324cb41d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fauto-tuning-for-deep-neural-network-deployment-ff2324cb41d&user=Federico+Peccia&userId=ce527bed0faf&source=-----ff2324cb41d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff2324cb41d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fauto-tuning-for-deep-neural-network-deployment-ff2324cb41d&source=-----ff2324cb41d---------------------bookmark_footer-----------)![](../Images/58fccebb5870c0dfc860e5b9927f920f.png)

照片由[S. Tsuchiya](https://unsplash.com/@s_tsuchiya?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

用来比较不同神经网络（NN）架构的指标之一是它们的训练时间。需要几个小时？几天？*几周*？通常情况下，通过更新用于训练的硬件可以改善这一点。用更强大的GPU替换较弱的GPU，将训练并行化到多个GPU等等。推理步骤也有类似的情况。我们会将训练好的网络部署到嵌入式设备上，比如微控制器吗？还是要在移动设备上运行它？也许网络太大了，我们需要一个嵌入式GPU甚至是服务器规模的GPU来执行它。

让我们选其中一个。我们拿出我们的NN，为我们的设备编译它，然后测试它的运行速度。哦，不好了！它不能满足我们的延迟要求！我们需要NN在1秒内运行，而我们的NN需要2秒！现在有什么选择呢？

+   **用更强大的设备替换：** 这可能会带来很大问题，特别是当存在严格的应用约束条件时。也许你只能使用特定的已认证硬件。或者你有难以满足的能量限制。

+   **减少NN的复杂性：** 这也可能很难，因为你可能会失去...
