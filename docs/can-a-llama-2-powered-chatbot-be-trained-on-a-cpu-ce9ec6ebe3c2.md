# 如何使用 Llama2 和 LangChain 构建本地聊天机器人

> 原文：[https://towardsdatascience.com/can-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2?source=collection_archive---------3-----------------------#2023-10-12](https://towardsdatascience.com/can-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2?source=collection_archive---------3-----------------------#2023-10-12)

## 概述及使用 Python 的实现

[](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----ce9ec6ebe3c2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------) ·6 min read·2023年10月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce9ec6ebe3c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2&user=Aashish+Nair&userId=3087ba81e065&source=-----ce9ec6ebe3c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce9ec6ebe3c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2&source=-----ce9ec6ebe3c2---------------------bookmark_footer-----------)![](../Images/ded2ef377ff10804f982df906fed5cbe.png)

图片由 [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 目录

∘ [介绍](#6563)

∘ [案例研究](#dcc3)

∘ [步骤 1 — 创建向量存储](#9999)

∘ [步骤 2—创建 QA 链](#b94f)

∘ [步骤 3 — 创建用户界面](#a748)

∘ [评估聊天机器人](#347d)

∘ [结果](#3580)

∘ [最终评判](#b61a)

∘ [参考文献](#ec13)

## 介绍

本地模型的出现受到了希望构建自己定制LLM应用程序的企业的欢迎。它们使开发者能够构建能够离线运行并符合隐私和安全要求的解决方案。

这些LLM最初非常庞大，主要面向那些有资金和资源来配置GPU并在大量数据上训练模型的企业。

然而，现在本地的LLM已经有了更小的尺寸，这就引发了一个问题：是否有可能让使用基本CPU的个人也能利用这些相同的工具和技术？

这是一个值得考虑的问题，因为用户通过构建自己个人的本地聊天机器人可以获得很多好处，这些聊天机器人…
