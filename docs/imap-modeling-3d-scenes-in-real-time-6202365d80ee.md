# iMAP：实时建模3D场景

> 原文：[https://towardsdatascience.com/imap-modeling-3d-scenes-in-real-time-6202365d80ee?source=collection_archive---------9-----------------------#2023-05-12](https://towardsdatascience.com/imap-modeling-3d-scenes-in-real-time-6202365d80ee?source=collection_archive---------9-----------------------#2023-05-12)

## 使用手持RGB-D相机学习3D环境

[](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimap-modeling-3d-scenes-in-real-time-6202365d80ee&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----6202365d80ee---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------) · 15分钟阅读 · 2023年5月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6202365d80ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimap-modeling-3d-scenes-in-real-time-6202365d80ee&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----6202365d80ee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6202365d80ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimap-modeling-3d-scenes-in-real-time-6202365d80ee&source=-----6202365d80ee---------------------bookmark_footer-----------)![](../Images/75c66b8fbe2eb3a7bc26faba5f1ea272.png)

（照片由 [Brett Zeck](https://unsplash.com/@iambrettzeck?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/s/photos/map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

到目前为止，我们只见过用于建模 3D 场景的离线方法（例如，[NeRF](https://cameronrwolfe.substack.com/p/understanding-nerfs)，[SRNs](https://cameronrwolfe.substack.com/p/scene-representation-networks)，[DeepSDF](https://cameronrwolfe.substack.com/p/3d-generative-modeling-with-deepsdf) [2, 3, 4]）。尽管这些方法表现出色，但它们需要数天甚至数周的计算时间来训练基础神经网络。例如，NeRF 需要将近两天的时间来仅仅表示一个场景。此外，使用神经网络评估新场景视点也可能相当昂贵！考虑到这一点，我们可能会想知道是否有可能比这更快地学习场景表示。

这个问题在 [1] 中得到了探讨，提出了 iMAP，这是一种用于表示场景和定位（即，跟踪设备姿态）的实时系统。为了理解这意味着什么，请考虑一个移动穿越场景并捕捉周围环境的摄像机。iMAP 的任务是 *(i)* 接收这些数据，*(ii)* 建立观察到的场景的 3D 表示，并 (iii) 推断摄像机（即设备）在捕捉场景时的位置和方向！

iMAP 采用的方法与 NeRF [2] 非常相似，但有一些不同之处：

1.  它基于 RGB-D 数据。

1.  假设有一个流媒体设置。
