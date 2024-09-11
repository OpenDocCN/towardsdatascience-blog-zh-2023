# 如何通过配置参数改善你的 ChatGPT 输出

> 原文：[`towardsdatascience.com/how-to-improve-your-chatgpt-outputs-using-configuration-parameters-0eebd575646e?source=collection_archive---------6-----------------------#2023-12-13`](https://towardsdatascience.com/how-to-improve-your-chatgpt-outputs-using-configuration-parameters-0eebd575646e?source=collection_archive---------6-----------------------#2023-12-13)

## ChatGPT，生成性 AI

## 关注配置 ChatGPT 提示中的温度、Top P、频率惩罚和存在惩罚

[](https://alod83.medium.com/?source=post_page-----0eebd575646e--------------------------------)![Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----0eebd575646e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0eebd575646e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----0eebd575646e--------------------------------) [Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----0eebd575646e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8bc34d63aee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-your-chatgpt-outputs-using-configuration-parameters-0eebd575646e&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=post_page-f8bc34d63aee----0eebd575646e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0eebd575646e--------------------------------) ·9 分钟阅读·2023 年 12 月 13 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0eebd575646e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-your-chatgpt-outputs-using-configuration-parameters-0eebd575646e&source=-----0eebd575646e---------------------bookmark_footer-----------)![](img/c1db168e5e52dd71355c933addc5f3c2.png)

图片由 [Growtika](https://unsplash.com/@growtika?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我最近读了一本由 David Clinton 撰写的非常有趣的书，名为[生成式 AI 完全废弃指南](https://www.manning.com/books/the-complete-obsolete-guide-to-generative-ai)，由 Manning Publications 出版。在第二章中，作者描述了 AI 模型的主要参数是什么以及如何配置它们以适应需求。这些配置参数包括温度、Top P 值、频率惩罚和存在惩罚。

配置这些参数可以帮助 ChatGPT 理解你想要生成的输出类型。例如，如果你希望 ChatGPT 生成更具确定性的输出（严格与输入相关），可以设置一些参数。相反，如果你希望 ChatGPT 在生成输出时更具创造性，则可以设置其他参数。

为了理解输出类型的工作方式，David Clinton 在他的书中举了一个与天气相关的例子。一个更具确定性的输出可能是：*今天晴天，气温为 30°C*。一个更具创造性的输出可能是：*今天晴天，你可以去散步*。
