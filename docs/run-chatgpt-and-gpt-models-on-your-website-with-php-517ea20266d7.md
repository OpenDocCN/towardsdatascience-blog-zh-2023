# 在您的网站上使用 PHP 运行 ChatGPT 和 GPT 模型

> 原文：[https://towardsdatascience.com/run-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7?source=collection_archive---------5-----------------------#2023-05-02](https://towardsdatascience.com/run-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7?source=collection_archive---------5-----------------------#2023-05-02)

## 一种非常简单的解决方案，可以将 AI 和 GPT 模型提供给您的用户

[](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----517ea20266d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------) · 12 分钟阅读 · 2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F517ea20266d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7&user=Benjamin+Marie&userId=ad2a414578b3&source=-----517ea20266d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F517ea20266d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7&source=-----517ea20266d7---------------------bookmark_footer-----------)![](../Images/674c26153559ff00625bba226d4b3458.png)

图片来自 [Pixabay](https://pixabay.com/illustrations/media-internet-message-network-3683580/)。

GPT 模型可以改善网站和网络应用的用户体验。它们可以进行翻译、总结、回答问题以及完成许多其他任务。

将所有这些功能集成到您的在线服务中，通过 OpenAI API 是相当简单的。目前，OpenAI 仅提供对 Python 和 NodeJS 绑定的官方支持。

许多 [第三方绑定](https://github.com/openai-php/client) 已由社区开发，以方便在其他编程语言中部署。

在这篇文章中，我将向你展示如何在 PHP 中连接你的网站到 OpenAI 的 API。我还会解释如何解析和处理 API 返回的结果。

我只会涉及 GPT 模型，但你可以按照相同的流程操作 DALL-E 和 Whisper 模型。

# 先决条件

## GPT 模型

你不需要熟悉 GPT 模型才能理解和实现这篇文章，但我仍建议你阅读我对 GPT 模型的简单介绍：
