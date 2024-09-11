# 掌握 ChatGPT：使用 LLMs 的有效总结

> 原文：[https://towardsdatascience.com/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce?source=collection_archive---------2-----------------------#2023-05-22](https://towardsdatascience.com/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce?source=collection_archive---------2-----------------------#2023-05-22)

## 如何提示 ChatGPT 以获得高质量的总结

[](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)[![安德里亚·瓦伦苏埃拉](../Images/ddfc1534af92413fd91076f826cc49b6.png)](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------) [安德里亚·瓦伦苏埃拉](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f3f1654c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=post_page-a6f3f1654c3----16cf0e3625ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------) ·10 min 阅读·2023年5月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16cf0e3625ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=-----16cf0e3625ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16cf0e3625ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce&source=-----16cf0e3625ce---------------------bookmark_footer-----------)![](../Images/6c875157182b9485516e809510ba0514.png)

自制 gif。

你是否属于那种每次去新餐厅都会在 Google 地图上留下评价的人？

或许你是那种在 Amazon 购买时，特别是遇到低质量产品时会分享意见的人？

*别担心，我不会责怪你——我们都有这样的时刻！*

在今天的数据世界中，我们以多种方式贡献于数据洪流。我发现特别有趣的一种数据类型是文本数据，因为它的多样性和解释的难度，例如每天在互联网上发布的大量评论。*你是否曾经考虑过标准化和精简文本数据的重要性？* **欢迎来到摘要代理的世界！**

![](../Images/b1300eb548d9ed867e48cea796690892.png)

摘要代理是由 AI 图像生成工具 Dall-E 想象出来的。

摘要代理已经无缝地融入我们的日常生活，精简信息并在众多应用和平台中提供快速访问相关内容的服务。
