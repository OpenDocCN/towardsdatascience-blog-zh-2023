# 在亚马逊 Web 服务上构建生成式 AI 应用——我的第一次体验

> 原文：[`towardsdatascience.com/building-generative-ai-apps-on-amazon-web-services-my-first-experience-c54f60a21fb1?source=collection_archive---------4-----------------------#2023-09-22`](https://towardsdatascience.com/building-generative-ai-apps-on-amazon-web-services-my-first-experience-c54f60a21fb1?source=collection_archive---------4-----------------------#2023-09-22)

## 48 小时的黑客马拉松，主题为 Amazon Bedrock 和 SageMaker

[](https://col-jung.medium.com/?source=post_page-----c54f60a21fb1--------------------------------)![Col Jung](https://col-jung.medium.com/?source=post_page-----c54f60a21fb1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c54f60a21fb1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c54f60a21fb1--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----c54f60a21fb1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d4e2c520037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-generative-ai-apps-on-amazon-web-services-my-first-experience-c54f60a21fb1&user=Col+Jung&userId=8d4e2c520037&source=post_page-8d4e2c520037----c54f60a21fb1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c54f60a21fb1--------------------------------) · 11 分钟阅读 · 2023 年 9 月 22 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc54f60a21fb1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-generative-ai-apps-on-amazon-web-services-my-first-experience-c54f60a21fb1&user=Col+Jung&userId=8d4e2c520037&source=-----c54f60a21fb1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc54f60a21fb1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-generative-ai-apps-on-amazon-web-services-my-first-experience-c54f60a21fb1&source=-----c54f60a21fb1---------------------bookmark_footer-----------)![](img/f8073c7b4b636b8d93676f9be170a725.png)

图片来源：以色列·安德拉德 ([Unsplash](https://unsplash.com/photos/YI_9SivVt_s))

大公司还不完全确定如何利用生成式 AI，但他们希望做*一些事情*。

一些人正在通过**内部黑客马拉松**探索这项技术。

**更新**：我现在在 [YouTube](https://www.youtube.com/@col_builds) 上发布分析教程。

作为澳大利亚“四大”银行之一的工程师和 data scientist，我在过去一个月中参与了三次这些令人兴奋的活动。

为什么是黑客马拉松？

它们是一个[很好的方式](https://generativeai.pub/how-big-companies-are-scrambling-to-adopt-generative-ai-d52456fb4c69)，让公司的知识工作者——无论是技术型还是非技术型——头脑风暴生成性 AI 的应用场景，测试市场上现有的 AI 工具堆栈，并迅速制作一些工作原型供决策者审阅。

你能获得什么？

+   跳出日常工作。（这是个玩笑，老板！）

+   提升自己在初创公司和创新技能方面的能力。

+   生成性 AI 是颠覆性技术。赶上这趟列车，否则就会被甩在后头。

+   获得与来自不同云服务提供商的 AI 工具堆栈的**实际操作经验**，例如 *AWS* 与 *Microsoft*。
