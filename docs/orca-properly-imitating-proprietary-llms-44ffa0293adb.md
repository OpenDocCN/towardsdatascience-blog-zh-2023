# Orca: 正确模仿专有LLMs

> 原文：[https://towardsdatascience.com/orca-properly-imitating-proprietary-llms-44ffa0293adb?source=collection_archive---------3-----------------------#2023-09-30](https://towardsdatascience.com/orca-properly-imitating-proprietary-llms-44ffa0293adb?source=collection_archive---------3-----------------------#2023-09-30)

## 利用模仿创建高质量、开源的LLMs……

[](https://wolfecameron.medium.com/?source=post_page-----44ffa0293adb--------------------------------)[![卡梅隆·R·沃尔夫博士](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----44ffa0293adb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----44ffa0293adb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----44ffa0293adb--------------------------------) [卡梅隆·R·沃尔夫博士](https://wolfecameron.medium.com/?source=post_page-----44ffa0293adb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forca-properly-imitating-proprietary-llms-44ffa0293adb&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----44ffa0293adb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----44ffa0293adb--------------------------------) ·16 min read·2023年9月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F44ffa0293adb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forca-properly-imitating-proprietary-llms-44ffa0293adb&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----44ffa0293adb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F44ffa0293adb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forca-properly-imitating-proprietary-llms-44ffa0293adb&source=-----44ffa0293adb---------------------bookmark_footer-----------)![](../Images/aa2e365a5a760e33e8968360e016c5b1.png)

（照片由 [Thomas Lipke](https://unsplash.com/@t_lipke?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/p5nDU-d3Y0s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供）

随着对大型语言模型（LLMs）的研究进展，一个尚未解决的关键问题是，现有的高质量LLM是否可以用来有效地训练另一个LLM。目前，围绕这个话题存在很多争议和讨论。近期开源模仿模型的爆炸性增长最初表明，像ChatGPT这样的专有LLM可以以低成本轻松复制。然而，随后的研究得出结论，认为这些模型的评估是不完整和具有误导性的，发现这些模型实际上在理解上存在很大的差距。在这篇概述中，我们将研究旨在通过更强有力的方法解决专有LLM开源副本局限性的工作[1]。特别是，我们将看到，通过策划一个包含更多详细信息的大型数据集，可以使模仿学习变得更有效。

> “随着这些模型的不断演变和变得更加强大，一个引人入胜的问题出现了：我们是否可以利用模型本身来监督其自身行为或其他人工智能模型的行为？” *— 来自 [1]*
