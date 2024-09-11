# 您的个人 ChatGPT

> 原文：[https://towardsdatascience.com/your-own-personal-chatgpt-cb0512091e3f?source=collection_archive---------0-----------------------#2023-09-08](https://towardsdatascience.com/your-own-personal-chatgpt-cb0512091e3f?source=collection_archive---------0-----------------------#2023-09-08)

## 如何使用自定义数据微调 OpenAI 的 GPT-3.5 Turbo 模型，以执行新任务

[](https://robgon.medium.com/?source=post_page-----cb0512091e3f--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----cb0512091e3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb0512091e3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb0512091e3f--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----cb0512091e3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-chatgpt-cb0512091e3f&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----cb0512091e3f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb0512091e3f--------------------------------) ·15分钟阅读·2023年9月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb0512091e3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-chatgpt-cb0512091e3f&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----cb0512091e3f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb0512091e3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-chatgpt-cb0512091e3f&source=-----cb0512091e3f---------------------bookmark_footer-----------)![](../Images/7464d9a6182286a2f69d41a6779a233c.png)

**“一幅描绘可爱机器人上艺术课的极简画，”** 该图像使用 AI 图像生成程序 Midjourney 创建，并由作者编辑

当我收到来自 OpenAI 的邮件，宣布可以对 ChatGPT 进行微调时，我非常兴奋。这个更新是为了回应开发者和企业的请求，他们希望自定义模型以更好地满足特定需求。通过利用这一微调功能，现在可以改善可控性，获得更一致的输出格式，并建立所需的自定义语气。另一个值得注意的方面是，用户可以发送更短的提示，而不会显著影响性能。

这是 OpenAI 在其开发博客上所说的内容[1]。

> 此次更新使开发者能够自定义更适合其使用场景的模型，并在大规模环境中运行这些自定义模型。早期测试显示，经过微调的 GPT-3.5 Turbo 版本可以在某些狭窄任务上匹配甚至超越基础 GPT-4 的能力。与我们所有的 API 一样，发送进出微调 API 的数据由客户所有，OpenAI 或其他任何组织均不会使用这些数据来训练其他模型。— Andrew Peng 等，OpenAI

在这篇文章中，我将演示如何使用我的 Medium 文章中的文本作为训练和测试数据，将纯文本转换为 Markdown 格式…
