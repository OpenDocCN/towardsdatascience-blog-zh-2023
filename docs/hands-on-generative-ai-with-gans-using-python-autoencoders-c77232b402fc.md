# 使用 Python 的生成对抗网络（GANs）进行生成式 AI 实践：自动编码器

> 原文：[`towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc?source=collection_archive---------3-----------------------#2023-03-21`](https://towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc?source=collection_archive---------3-----------------------#2023-03-21)

![](img/6fa218e9894ae7f7c804d4ccfef9edc1.png)

图片来源：[GR Stocks](https://unsplash.com/@grstocks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 从自动编码器开始，以更好地理解 GANs

[](https://medium.com/@marcellopoliti?source=post_page-----c77232b402fc--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----c77232b402fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c77232b402fc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c77232b402fc--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----c77232b402fc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----c77232b402fc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c77232b402fc--------------------------------) ·6 分钟阅读·2023 年 3 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc77232b402fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc&user=Marcello+Politi&userId=7390355d40fe&source=-----c77232b402fc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc77232b402fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc&source=-----c77232b402fc---------------------bookmark_footer-----------)

## 介绍

近年来，生成模型因人工智能能够生成几乎无法与真实数据区分的合成实例而受到关注。像 Chat GPT 这样的神经网络可以生成文本，DALLE 可以生成完全原创的图像，你可能对此有所了解。

网站 [thispersondoesnotexist.com](https://this-person-does-not-exist.com/en) 是一个著名的生成网络示例，每次访问该链接时，都会显示一张由 AI 生成的不存在的人的图像。这只是生成 AI 神奇可能性的众多例子之一。

随着时间的推移，生成 AI 已经发展起来，随着研究的进展，不同的架构应运而生，以解决许多应用案例。但为了开始学习生成 AI 的主题，你需要熟悉一种架构：生成对抗网络（GANs）。

## GANs 概述

生成网络的最终**目标是生成与其训练集具有相同分布的新数据**。生成网络通常被认为是机器学习中的无监督学习的一部分，因为它们不需要…
