# ReLoRa: 在你的GPU上预训练大型语言模型

> 原文：[https://towardsdatascience.com/relora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf?source=collection_archive---------3-----------------------#2023-07-20](https://towardsdatascience.com/relora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf?source=collection_archive---------3-----------------------#2023-07-20)

## LoRa，但有多个连续重置

[](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frelora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----d104756f9ddf---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------) · 8分钟阅读 · 2023年7月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd104756f9ddf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frelora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf&source=-----d104756f9ddf---------------------bookmark_footer-----------)![](../Images/a84220fa206495cacbda94863cf4dfe4.png)

ReLoRa框架 — 图片由作者提供

2021年，[胡等人](https://arxiv.org/abs/2106.09685)提出了用于LLMs的低秩适配器（LoRa）。该方法通过仅训练几个附加参数（低秩网络），同时保持LLM的原始参数（高秩网络）冻结，从而显著降低了对大型语言模型（LLMs）进行微调的成本。

使用LoRa，我们仍然需要一个现有的预训练模型进行微调，即由于低秩限制，它不能从头开始预训练一个好的LLM。这使得大多数个人和组织难以承担预训练的成本。

为了降低这个成本，[Lialin 等（2023）](https://arxiv.org/pdf/2307.05695.pdf) 提出了 ReLoRa。这是对 LoRa 的一种修改，允许从头开始预训练 LLM。

在本文中，我首先解释 ReLoRa 的工作原理。接着，我分析并评论描述 ReLoRa 的科学论文中的结果。在最后一部分，我展示了如何在你的计算机上设置和运行 ReLoRa。

*关于许可证的说明：* [*描述 ReLoRa 的科学论文*](https://arxiv.org/abs/2307.05695) *以 CC BY 4.0 许可证发布。* [*ReLoRa 的源代码已发布在 GitHub 上*](https://github.com/guitaricet/peft_pretraining) *并以 Apache 2.0 许可证发布，允许商业使用。*

# ReLoRa：从低秩到高秩网络
