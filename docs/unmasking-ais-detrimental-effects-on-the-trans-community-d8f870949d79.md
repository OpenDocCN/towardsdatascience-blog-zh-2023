# 揭示人工智能对跨性别群体的有害影响

> 原文：[https://towardsdatascience.com/unmasking-ais-detrimental-effects-on-the-trans-community-d8f870949d79?source=collection_archive---------2-----------------------#2023-06-20](https://towardsdatascience.com/unmasking-ais-detrimental-effects-on-the-trans-community-d8f870949d79?source=collection_archive---------2-----------------------#2023-06-20)

## 性别识别软件的危险、不足的医疗模型和跨性别恐惧内容的放大

[](https://conorosullyds.medium.com/?source=post_page-----d8f870949d79--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----d8f870949d79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8f870949d79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d8f870949d79--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----d8f870949d79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funmasking-ais-detrimental-effects-on-the-trans-community-d8f870949d79&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----d8f870949d79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8f870949d79--------------------------------) ·8 min read·2023年6月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8f870949d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funmasking-ais-detrimental-effects-on-the-trans-community-d8f870949d79&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----d8f870949d79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8f870949d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funmasking-ais-detrimental-effects-on-the-trans-community-d8f870949d79&source=-----d8f870949d79---------------------bookmark_footer-----------)![](../Images/28609d24d49c8cb734ed53626d0017e3.png)

图片由 [Delia Giandeini](https://unsplash.com/@dels?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

关于人工智能风险的讨论往往集中在人工通用智能（AGI）和末日情景的假设危险上。机器人不会接管世界。然而，当前水平的人工智能确实带来了实际的风险，尤其是对已经受到这项技术影响的跨性别和性别非顺应群体。

我们将重点阐述对该社区的危险，具体包括：

+   **自动性别识别**

+   **医疗模型**的局限性

+   社交媒体上**跨性别恐惧内容的放大**

虽然跨性别社区直接感受到这些后果，但这些危险影响到我们所有人。它们传播仇恨，限制了多样性的丰富性，约束了我们共同完全表达自己的能力。我们必须理解作为技术专业人员，我们如何能够支持跨性别人士，并创建一个更强大的社会。

> 我们正处于可以大规模部署人工智能的阶段，这仅仅是因为我们拥有大量的数据和…
