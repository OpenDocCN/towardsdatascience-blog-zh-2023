# AI 解释性梯度的必要性

> 原文：[https://towardsdatascience.com/the-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848?source=collection_archive---------3-----------------------#2023-07-29](https://towardsdatascience.com/the-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848?source=collection_archive---------3-----------------------#2023-07-29)

## 细节过多可能会令人不知所措，而细节不足则可能会引导误解。

[](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)[![Kevin Berlemont, PhD](../Images/18697f38b76f1fb04870f565cfb04b4c.png)](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)[](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------) [Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3dea771eb493&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=post_page-3dea771eb493----743ee0bcb848---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------) ·4 分钟阅读·2023年7月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F743ee0bcb848&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=-----743ee0bcb848---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F743ee0bcb848&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848&source=-----743ee0bcb848---------------------bookmark_footer-----------)![](../Images/053eb6288d97c8586d780c90ace9e905.png)

照片由 [No Revisions](https://unsplash.com/@norevisions?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

“*任何足够先进的技术都与魔法无异*” — **阿瑟·克拉克**

随着自动驾驶汽车、计算机视觉的进步，以及最近的大型语言模型，科学有时会让人感觉像魔法一样！模型变得越来越复杂，每天都在进步，当尝试向新受众解释复杂模型时，可能会不自觉地挥手并喃喃自语一些关于反向传播和神经网络的术语。然而，有必要描述AI模型、其预期影响和潜在偏见，这就是**可解释AI**发挥作用的地方。

在过去十年中，随着AI方法的爆炸性发展，用户已经开始接受他们所得到的答案而不加质疑。整个算法过程通常被描述为一个黑匣子，即使是开发该模型的研究人员，也不总是能够直接或完全理解模型是如何得出具体结果的。为了建立用户的信任和信心，公司必须描述他们所使用的不同系统的公平性、透明度和基本决策过程。这种方法不仅导致对AI系统的负责任态度，而且…
