# 女性都去哪儿了？

> 原文：[`towardsdatascience.com/where-are-all-the-women-3c79dabfdfc2?source=collection_archive---------5-----------------------#2023-07-26`](https://towardsdatascience.com/where-are-all-the-women-3c79dabfdfc2?source=collection_archive---------5-----------------------#2023-07-26)

## 探索大型语言模型在历史知识中的偏见

[](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)![Yennie Jun](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-are-all-the-women-3c79dabfdfc2&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----3c79dabfdfc2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------) · 10 min read · 2023 年 7 月 26 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c79dabfdfc2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-are-all-the-women-3c79dabfdfc2&user=Yennie+Jun&userId=12ca1ab81192&source=-----3c79dabfdfc2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c79dabfdfc2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-are-all-the-women-3c79dabfdfc2&source=-----3c79dabfdfc2---------------------bookmark_footer-----------)![](img/84e3040a1892146c1f28f18f09536c4d.png)

GPT-4 和 Claude 最常提到的一些顶级历史人物。图像来源于维基百科。拼贴图由作者创建。

*(这篇文章最初发布在* [*我的个人博客*](https://www.artfish.ai/p/where-are-all-the-women)*)*

像 ChatGPT 这样的**大型语言模型（LLMs）**在教育和专业领域中的使用越来越广泛。在将这些模型整合到现有应用程序和我们的日常生活中之前，理解和研究这些模型中存在的各种偏见是非常重要的。

我在我的[上一篇文章](https://blog.yenniejun.com/p/world-history-through-ai)中研究了一个偏见，涉及历史事件。我深入探讨了 LLMs，以了解它们如何以主要历史事件的形式编码历史知识。我发现它们对理解主要历史事件存在严重的西方偏见。

在类似的脉络下，在这篇文章中，我探讨了语言模型对重要历史人物的理解。我询问了两个 LLM 历史上最重要的人物是谁。我将这个过程重复了 10 次，涉及 10 种不同的语言。一些名字，如甘地和耶稣，出现得非常频繁。其他名字，如玛丽·居里或克娄巴特拉，出现得较少。与模型生成的男性名字数量相比，女性名字极少。

我最大的疑问是：所有的女性都在哪里？
