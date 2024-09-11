# 超越 NeRFs（第二部分）

> 原文：[https://towardsdatascience.com/beyond-nerfs-part-two-83af2bd17d0?source=collection_archive---------11-----------------------#2023-06-13](https://towardsdatascience.com/beyond-nerfs-part-two-83af2bd17d0?source=collection_archive---------11-----------------------#2023-06-13)

## 在实际应用中成功使用 NeRFs 的技巧与窍门

[](https://wolfecameron.medium.com/?source=post_page-----83af2bd17d0--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----83af2bd17d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----83af2bd17d0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----83af2bd17d0--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----83af2bd17d0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-nerfs-part-two-83af2bd17d0&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----83af2bd17d0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----83af2bd17d0--------------------------------) · 16 分钟阅读 · 2023年6月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F83af2bd17d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-nerfs-part-two-83af2bd17d0&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----83af2bd17d0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F83af2bd17d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-nerfs-part-two-83af2bd17d0&source=-----83af2bd17d0---------------------bookmark_footer-----------)![](../Images/84d3b26f3db9121dd04bc299b88a98e0.png)

（照片由 [Ashim D’Silva](https://unsplash.com/@randomlies?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/wild-west?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

在表示和渲染3D场景的领域中，神经辐射场（NeRFs）在准确性方面取得了巨大突破。给定几个底层场景的图像，NeRFs可以从任意视角重建该场景的高分辨率2D渲染图。与之前的技术，如[局部光场融合（LLFF）](https://cameronrwolfe.substack.com/p/local-light-field-fusion) [5]和[场景表示网络（SRNs）](https://cameronrwolfe.substack.com/p/scene-representation-networks) [6]相比，NeRFs在捕捉场景外观和几何形状的复杂组件（例如视角依赖的反射和复杂的材料）方面更具能力。

NeRFs有潜力彻底改变虚拟现实、计算机图形等应用。例如，可以设想使用NeRFs根据网上提供的房屋图像重建待售房屋的3D渲染图，甚至使用基于真实场景训练的NeRFs设计视频游戏环境。然而，在其最初的形式中，NeRFs主要在简单、受控环境中的图像上进行了评估。当在真实场景图像上训练时，NeRFs的表现往往不如预期（见下文），这使得它们在实际应用中不够实用。
