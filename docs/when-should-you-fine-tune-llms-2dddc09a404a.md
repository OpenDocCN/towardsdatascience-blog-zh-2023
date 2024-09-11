# 你什么时候应该微调 LLM？

> 原文：[https://towardsdatascience.com/when-should-you-fine-tune-llms-2dddc09a404a?source=collection_archive---------1-----------------------#2023-05-15](https://towardsdatascience.com/when-should-you-fine-tune-llms-2dddc09a404a?source=collection_archive---------1-----------------------#2023-05-15)

## 目前有很多令人兴奋的可以微调的开源 LLM。但这与使用一个封闭的 API 相比如何呢？

[](https://skanda-vivek.medium.com/?source=post_page-----2dddc09a404a--------------------------------)[![Skanda Vivek](../Images/9d25bee2fb75176ca7f7ea6eff7d7ab5.png)](https://skanda-vivek.medium.com/?source=post_page-----2dddc09a404a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2dddc09a404a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2dddc09a404a--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----2dddc09a404a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220d9bbb8014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-should-you-fine-tune-llms-2dddc09a404a&user=Skanda+Vivek&userId=220d9bbb8014&source=post_page-220d9bbb8014----2dddc09a404a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2dddc09a404a--------------------------------) ·7 min read·2023年5月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2dddc09a404a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-should-you-fine-tune-llms-2dddc09a404a&user=Skanda+Vivek&userId=220d9bbb8014&source=-----2dddc09a404a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2dddc09a404a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-should-you-fine-tune-llms-2dddc09a404a&source=-----2dddc09a404a---------------------bookmark_footer-----------)

我经常收到这个问题——无论是在 LinkedIn 上被人询问如何微调开源模型如 LLaMA，还是公司在尝试弄清楚出售 LLM 托管和部署解决方案的商业案例，或者公司希望利用 AI 和 LLM 应用于其产品。但当我问他们为什么不使用像 ChatGPT 这样的闭源模型时，他们并没有真正的答案。所以我决定以每天应用 LLM 解决业务问题的身份写这篇文章。

# 对闭源 API 的论证

你是否尝试过为你的用例实现 ChatGPT API？也许你想要总结文档或回答问题，或者只是想在你的网站上添加一个聊天机器人。更多时候，你会发现 ChatGPT 在多语言任务上表现得相当出色。

一个常见的看法是这些模型过于昂贵。但以每千个 token 0.002 美元的价格，我敢打赌你至少可以在几百个样本上试用一下，评估 LLM 是否适合你的特定应用。实际上，每天几千次 API 调用或在这个范围内，ChatGPT API 的成本远低于托管自定义开源模型的基础设施[正如我在这篇博客中写到的](/llm-economics-chatgpt-vs-open-source-dfc29f69fec1)。
