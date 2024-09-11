# 探索大型语言模型中的创造力：从GPT-2到GPT-4

> 原文：[https://towardsdatascience.com/exploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57?source=collection_archive---------3-----------------------#2023-04-11](https://towardsdatascience.com/exploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57?source=collection_archive---------3-----------------------#2023-04-11)

## 通过创造力测试分析大型语言模型中创造过程的演变

[](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----1c2d1779be57---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------) ·21分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c2d1779be57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57&user=Yennie+Jun&userId=12ca1ab81192&source=-----1c2d1779be57---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c2d1779be57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57&source=-----1c2d1779be57---------------------bookmark_footer-----------)![](../Images/7c92d68bd2a35c36d6c1f1499d438759.png)

**Midjourney** 设想中的创造力。由作者生成。

*该博客最初发布在* [https://www.artfish.ai/p/exploring-creativity-in-large-language](https://www.artfish.ai/p/exploring-creativity-in-large-language)

最近几周，人们利用大型语言模型（LLMs）生成了各种创意内容，如[书籍](https://www.reuters.com/technology/chatgpt-launches-boom-ai-written-e-books-amazon-2023-02-21/)、[闪小说](https://blog.yenniejun.com/p/creative-writing-with-gpt-3-from)、[说唱对决](https://twitter.com/mehran__jalali/status/1639846978850021377?lang=en)和[音乐和弦](/using-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268)。但这些模型的创造过程的水平是否可以更广泛地衡量呢？

人类创造力长期以来一直吸引着心理学家和研究人员。[从1950年代开始](https://www.ideatovalue.com/podc/nickskillicorn/2021/04/the-1950-speech-that-started-creativity-research/)，研究人员创建了一系列测试来比较个体的创造性表现和潜力。虽然没有单一的测试能完全捕捉创造力，但这些测试试图衡量和量化其不同方面。

在这篇文章中，我分析了2019年至2023年GPT模型在测量两种创造力的测试中的表现：聚合性（存在一个正确的解决方案）和发散性（开放式的；可能存在多个解决方案）[[1](#footnote-1)]。这些测试包括：

+   用第四个词连接三个看似无关的词（[远程联想测试](https://en.wikipedia.org/wiki/Remote_Associates_Test)）

+   产生大量日常物品的替代用途…
