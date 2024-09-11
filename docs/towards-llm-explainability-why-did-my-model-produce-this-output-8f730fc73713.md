# 面向大型语言模型的可解释性：为什么我的模型会产生这个输出？

> 原文：[https://towardsdatascience.com/towards-llm-explainability-why-did-my-model-produce-this-output-8f730fc73713?source=collection_archive---------6-----------------------#2023-12-15](https://towardsdatascience.com/towards-llm-explainability-why-did-my-model-produce-this-output-8f730fc73713?source=collection_archive---------6-----------------------#2023-12-15)

## 最近几个月发布的更大、更先进的大型语言模型展示了新能力，同时也引发了对人工智能安全性的广泛关注。大型语言模型的可解释性研究旨在拓展我们对这些模型工作原理的理解。

[](https://medium.com/@georgiadeaconu?source=post_page-----8f730fc73713--------------------------------)[![Georgia Deaconu](../Images/39ba1bea77aa46bb39b2975108c3adaa.png)](https://medium.com/@georgiadeaconu?source=post_page-----8f730fc73713--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f730fc73713--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f730fc73713--------------------------------) [Georgia Deaconu](https://medium.com/@georgiadeaconu?source=post_page-----8f730fc73713--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc4a98f38b0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-llm-explainability-why-did-my-model-produce-this-output-8f730fc73713&user=Georgia+Deaconu&userId=c4a98f38b0e&source=post_page-c4a98f38b0e----8f730fc73713---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f730fc73713--------------------------------) · 9分钟阅读 · 2023年12月15日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f730fc73713&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-llm-explainability-why-did-my-model-produce-this-output-8f730fc73713&user=Georgia+Deaconu&userId=c4a98f38b0e&source=-----8f730fc73713---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f730fc73713&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-llm-explainability-why-did-my-model-produce-this-output-8f730fc73713&source=-----8f730fc73713---------------------bookmark_footer-----------)

大型语言模型（LLMs）在过去一年中取得了很多发展，例如最近发布的 GPT-4 和 Claude 2。这些模型相对于其前身展现了新能力，但大多数是通过事后分析发现的，并非有目的的训练计划的一部分。它们是模型在参数数量、训练数据和计算资源方面扩展的结果。

从概念层面上看，我喜欢将 LLM 比作压缩算法。大量的互联网数据输入后，经过许多 FLOPS，我们得到一个几百 GB 的文件，包含了 LLM 的参数。模型无法精确检索初始知识，但大多数时候仍能产生相关的输出。

![](../Images/dbaf98a73c1b3f342cb9792dc0587ca1.png)

图片由作者和 DALL-E 3 提供（灵感来源于 Karpathy 的 [llmintro](https://drive.google.com/file/d/1pxx_ZI7O-Nwl7ZLNk5hI3WzAsTLwvNU7/view)）。

LLM 的神秘性不在于技术架构或计算的复杂性。如果…
