# 你自己的个人 LLaMa

> 原文：[`towardsdatascience.com/your-own-personal-llama-8cf24a9237cb?source=collection_archive---------2-----------------------#2023-10-09`](https://towardsdatascience.com/your-own-personal-llama-8cf24a9237cb?source=collection_archive---------2-----------------------#2023-10-09)

## 如何找到、检索和输入你的自定义数据到 LLaMa 2，以提升响应效果

[](https://robgon.medium.com/?source=post_page-----8cf24a9237cb--------------------------------)![Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----8cf24a9237cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8cf24a9237cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8cf24a9237cb--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----8cf24a9237cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-llama-8cf24a9237cb&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----8cf24a9237cb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8cf24a9237cb--------------------------------) ·12 分钟阅读·2023 年 10 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8cf24a9237cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-llama-8cf24a9237cb&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----8cf24a9237cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8cf24a9237cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-own-personal-llama-8cf24a9237cb&source=-----8cf24a9237cb---------------------bookmark_footer-----------)![](img/1b4813b49c3698de27945718255e006d.png)

**“一幅描绘可爱小羊驼在艺术课堂上站立的极简主义画作，”** 我*使用 AI 图像创作程序* Midjourney 创建，并由作者编辑

在我[上一篇文章](https://medium.com/p/cb0512091e3f)中，我展示了如何微调 OpenAI 的 ChatGPT 来提升文本格式化等任务的结果。虽然微调帮助大型语言模型（LLM）更好地完成格式化任务，但该模型似乎并未充分吸收训练数据中的知识。

在这篇文章中，我探讨了使用开源 LLM、Meta 的 LLaMa 2 [1] 和一种叫做检索增强生成（RAG）[2] 的技术，以自动查找相关的参考文本，帮助模型回答问题。我使用了我在 Medium 上的 36 篇文章中的文本作为参考材料，看看是否能创建一个私人定制的聊天机器人，回答有关我写作的问题。

例如，在没有提供任何参考文本的情况下，LLaMa 2 对一个关于 我其中一篇文章 的问题作出了如下回应。

> ***RobG:*** *什么是 Muybridge Derby？*
> 
> **LLaMa 2:** 感谢你的提问！我在这里帮助你解决你的问题。然而，我必须告诉你，“Muybridge Derby”在任何背景下都不是一个真实或公认的术语。它可能是拼写错误或虚构的。
> 
> 术语，我不能提供关于不存在的事物的信息。
> 
> 如果你能…
