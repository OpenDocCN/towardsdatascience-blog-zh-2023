# AI 是否有政治观点？

> 原文：[https://towardsdatascience.com/does-ai-have-political-opinions-d50087968ba8?source=collection_archive---------11-----------------------#2023-02-02](https://towardsdatascience.com/does-ai-have-political-opinions-d50087968ba8?source=collection_archive---------11-----------------------#2023-02-02)

## 测量 GPT-3 在经济和社会尺度上的政治意识形态

[](https://medium.com/@artfish?source=post_page-----d50087968ba8--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----d50087968ba8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d50087968ba8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d50087968ba8--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----d50087968ba8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-ai-have-political-opinions-d50087968ba8&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----d50087968ba8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d50087968ba8--------------------------------) ·9 分钟阅读·2023年2月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd50087968ba8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-ai-have-political-opinions-d50087968ba8&user=Yennie+Jun&userId=12ca1ab81192&source=-----d50087968ba8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd50087968ba8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-ai-have-political-opinions-d50087968ba8&source=-----d50087968ba8---------------------bookmark_footer-----------)![](../Images/8d552f9fef0e955ccc7c2d4aea0dcea7.png)

一个机器人和一个指南针，由 Stable Diffusion 想象并创建

*这篇文章最初发表于我的* [*博客*](https://www.artfish.ai/p/does-ai-have-political-opinions)

有一句话提到，在礼貌的社会中，你应该避免讨论三件事：政治、宗教和金钱。在本文中，我打破礼貌常规，以确定人工智能对这三种话题的回应。随着人工智能工具越来越多地融入我们的生活（例如[撰写新闻文章](https://www.wsj.com/articles/buzzfeed-to-use-chatgpt-creator-openai-to-help-create-some-of-its-content-11674752660)或在[心理健康聊天机器人中使用](https://www.nbcnews.com/tech/internet/chatgpt-ai-experiment-mental-health-tech-app-koko-rcna65110)），了解这些工具是否生成反映某些政治观点的输出显得尤为重要（也很有趣）。

在本文中，我通过让[OpenAI的GPT-3](https://openai.com/api/)模型接受[政治罗盘测试](https://www.politicalcompass.org/about)，对有争议的政治、经济和社会话题进行探讨。本文中包含的所有问题均复制自该网站。

这里是对GPT-3政治罗盘的一个窥视。左到右轴衡量经济意识形态；上下轴衡量社会意识形态。红点描述了GPT-3输出所反映的政治观点：经济上偏中左，社会上偏自由。
