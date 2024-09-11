# 自动驾驶中的可驾驶空间——学术界

> 原文：[https://towardsdatascience.com/drivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15?source=collection_archive---------4-----------------------#2023-05-18](https://towardsdatascience.com/drivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15?source=collection_archive---------4-----------------------#2023-05-18)

## 2023年关于可驾驶空间的学术研究趋势

[](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)[![Patrick Langechuan Liu](../Images/fecbf85146a9bde21e6b2251538ddd65.png)](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------) [Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd875946648f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdrivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=post_page-d875946648f7----ef1a6aa4dc15---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------) ·13分钟阅读·2023年5月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef1a6aa4dc15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdrivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=-----ef1a6aa4dc15---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef1a6aa4dc15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdrivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15&source=-----ef1a6aa4dc15---------------------bookmark_footer-----------)

可驾驶空间或自由空间在自动驾驶中扮演着至关重要的安全角色。在之前的博客文章中，我们回顾了这一常被忽视的感知特征的定义和重要性。在本文中，我们将回顾最新的学术研究趋势。

[](/drivable-space-in-autonomous-driving-the-concept-df699bb8682f?source=post_page-----ef1a6aa4dc15--------------------------------) [## 自动驾驶中的可驾驶空间——概念

### 可驾驶空间的定义及其重要性

[towardsdatascience.com](/drivable-space-in-autonomous-driving-the-concept-df699bb8682f?source=post_page-----ef1a6aa4dc15--------------------------------)

可行驶空间检测算法可以在两个维度上进行测量：输入和输出。关于**输入传感器模态**，可行驶空间检测方法可以分为基于视觉的、基于激光雷达的或视觉-激光雷达融合的方法。关于**输出空间表示**，它们可以分为二维透视图像空间、三维空间和鸟瞰视图（BEV）空间。

视觉图像本质上是二维的，而激光雷达点云测量本质上是三维的。正如在[之前的博客文章](/monocular-bev-perception-with-transformers-in-autonomous-driving-c41e4a893944)中讨论的那样，BEV 空间本质上是一个简化或退化的三维空间，我们将在整个博客中将 BEV 空间和三维空间交替使用。实质上，我们有一个 2x2 的输入-输出矩阵来评估所有可行驶空间算法，如下图所示。图中绿色的右上象限是北极星，它具有最佳……
