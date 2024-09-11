# 高效服务开源 LLMs

> 原文：[https://towardsdatascience.com/efficiently-serving-open-source-llms-5f0bf5d8fd59?source=collection_archive---------6-----------------------#2023-08-14](https://towardsdatascience.com/efficiently-serving-open-source-llms-5f0bf5d8fd59?source=collection_archive---------6-----------------------#2023-08-14)

[](https://medium.com/@ryanshrott?source=post_page-----5f0bf5d8fd59--------------------------------)[![Ryan Shrott](../Images/186524066383b4b02c994692aebb3ea5.png)](https://medium.com/@ryanshrott?source=post_page-----5f0bf5d8fd59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f0bf5d8fd59--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f0bf5d8fd59--------------------------------) [Ryan Shrott](https://medium.com/@ryanshrott?source=post_page-----5f0bf5d8fd59--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Faba7ffb1d8f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficiently-serving-open-source-llms-5f0bf5d8fd59&user=Ryan+Shrott&userId=aba7ffb1d8f5&source=post_page-aba7ffb1d8f5----5f0bf5d8fd59---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f0bf5d8fd59--------------------------------) · 5分钟阅读 · 2023年8月14日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f0bf5d8fd59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficiently-serving-open-source-llms-5f0bf5d8fd59&user=Ryan+Shrott&userId=aba7ffb1d8f5&source=-----5f0bf5d8fd59---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f0bf5d8fd59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficiently-serving-open-source-llms-5f0bf5d8fd59&source=-----5f0bf5d8fd59---------------------bookmark_footer-----------)![](../Images/c16d4589aafa99416a26da8bff1b5afe.png)

照片由 [Mariia Shalabaieva](https://unsplash.com/@maria_shalabaieva?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文解释了我个人使用6种常见方法为开源LLM服务的经验：AWS Sage Maker、Hugging Face、Together.AI、VLLM 和 Petals.ml。

## 其中的困难…

你曾经体验过服务自己精心调整的开源LLM的痛苦、挣扎和荣耀，但最终因为成本、推理时间、可靠性和技术挑战决定回到Open AI或Anthropic :( 你也放弃了租用A100 GPU（许多供应商的GPU已经预订到2023年底！）。而且你没有100K来购买2级A100服务器。尽管如此，你仍然梦想着，真的希望开源能够为你的解决方案服务。也许你的公司不想将其私人数据发送到Open AI，或者你需要一个针对非常特定任务的精细调整模型？在这篇文章中，我将概述并比较一些2023年最有效的开源LLM推理方法/平台。我将对比6种方法，并解释在何时应使用其中的某一种。我个人尝试了这6种方法，并将详细描述我的个人经验：**AWS Sage Maker、Hugging Face推理端点、Together.AI、VLLM和Petals.ml**。我没有所有的答案，但我会尽力详细说明我的经验。我与这些提供商没有任何金钱上的联系，只是分享我的…
