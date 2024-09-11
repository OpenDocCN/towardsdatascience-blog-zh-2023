# Meta AI 的另一项革命性大规模模型 — 用于图像特征提取的 DINOv2

> 原文：[https://towardsdatascience.com/meta-ais-another-revolutionary-large-scale-model-dinov2-for-image-feature-extraction-1114b287eadd?source=collection_archive---------6-----------------------#2023-06-26](https://towardsdatascience.com/meta-ais-another-revolutionary-large-scale-model-dinov2-for-image-feature-extraction-1114b287eadd?source=collection_archive---------6-----------------------#2023-06-26)

## DINOv2 是最好的自监督 ViT 基础深度学习模型之一，用于图像特征提取

[](https://medium.com/@gkeretchashvili?source=post_page-----1114b287eadd--------------------------------)[![Gurami Keretchashvili](../Images/4da78f113a0046c2deb8224e09dd9e3d.png)](https://medium.com/@gkeretchashvili?source=post_page-----1114b287eadd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1114b287eadd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1114b287eadd--------------------------------) [Gurami Keretchashvili](https://medium.com/@gkeretchashvili?source=post_page-----1114b287eadd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba1f382fdaca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeta-ais-another-revolutionary-large-scale-model-dinov2-for-image-feature-extraction-1114b287eadd&user=Gurami+Keretchashvili&userId=ba1f382fdaca&source=post_page-ba1f382fdaca----1114b287eadd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1114b287eadd--------------------------------) ·8分钟阅读·2023年6月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1114b287eadd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeta-ais-another-revolutionary-large-scale-model-dinov2-for-image-feature-extraction-1114b287eadd&source=-----1114b287eadd---------------------bookmark_footer-----------)

# 引言

Meta AI 推出了图像特征提取模型的新版本，称为 DINOv2，它可以自动从图像中提取视觉特征。这是 AI 领域特别是在计算机视觉领域中，数据和模型扩展的另一项革命性进步。

![](../Images/b5509b6f4b08541f8e6df946fd86e4c3.png)

DINOv2 的演示由 ai.facebook.com 提供

# 动机 — 我们为什么要关心？

DINOv2 是一个自监督模型，不需要微调且性能良好。此外，它可以作为许多不同计算机视觉任务的骨干，如：

+   分类，细粒度分类 — 例如猫与狗，或例如狗品种识别。

+   图像检索 — 例如，从大量互联网上的图像中找到与你的包相似的包。

+   语义图像分割 — 将标签或类别关联到图像中的每一个像素。

+   视频理解 — 自动识别和解释视频的各种方面，如物体、动作、事件、场景，甚至更高层次的概念。
