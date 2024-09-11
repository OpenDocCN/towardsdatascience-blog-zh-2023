# 使用大型语言模型进行有效提示的制作

> 原文：[`towardsdatascience.com/crafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664?source=collection_archive---------6-----------------------#2023-10-24`](https://towardsdatascience.com/crafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664?source=collection_archive---------6-----------------------#2023-10-24)

## 人工智能与大型语言模型

## 在超过 2 年的经验基础上，提炼出关键点，并结合人工智能开发者的教程、实际操作和示例。

[](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcrafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----dbbdf019f664---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------) ·8 分钟阅读·2023 年 10 月 24 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdbbdf019f664&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcrafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664&source=-----dbbdf019f664---------------------bookmark_footer-----------)![](img/5d9c95adc4b403b4cdbf2d4155e8083a.png)

图片由 [Emiliano Vittoriosi](https://unsplash.com/@emilianovittoriosi?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在信息丰富的时代——想象一下每个可能话题上发表的大量文章、会议和演讲中花费的时间，或是你每天在工作中收到的电子邮件量——将复杂内容提炼成简洁、有洞察力的总结是无价的。摘要工具已经存在了一段时间，但最近出现的大型语言模型，如 GPT、Bard 或 LLaMa 系列中的一些最受欢迎的模型，将摘要提升到了新的水平。这是因为大型语言模型并不像固定规则的摘要工具那样工作，而是能够“理解”输入文本的内容，并以比不使用语言模型的工具更大的灵活性生成简洁的摘要。

特别是，给语言模型传递正确的提示不仅可以实现普通的摘要，还可以按照特定风格进行改写，转换被动和主动形式，改变叙述视角，甚至理解包含片段的文本。
