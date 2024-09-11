# 探索标记概率作为过滤 GPT-3 回答的手段

> 原文：[`towardsdatascience.com/exploring-token-probabilities-as-a-means-to-filter-gpt-3s-answers-3e7dfc9ca0c?source=collection_archive---------2-----------------------#2023-01-19`](https://towardsdatascience.com/exploring-token-probabilities-as-a-means-to-filter-gpt-3s-answers-3e7dfc9ca0c?source=collection_archive---------2-----------------------#2023-01-19)

## 为构建更好的 GPT-3 驱动聊天机器人

[](https://lucianosphere.medium.com/?source=post_page-----3e7dfc9ca0c--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----3e7dfc9ca0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e7dfc9ca0c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e7dfc9ca0c--------------------------------)[LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----3e7dfc9ca0c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-token-probabilities-as-a-means-to-filter-gpt-3s-answers-3e7dfc9ca0c&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----3e7dfc9ca0c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e7dfc9ca0c--------------------------------) ·12 分钟阅读·Jan 19, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e7dfc9ca0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-token-probabilities-as-a-means-to-filter-gpt-3s-answers-3e7dfc9ca0c&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----3e7dfc9ca0c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e7dfc9ca0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-token-probabilities-as-a-means-to-filter-gpt-3s-answers-3e7dfc9ca0c&source=-----3e7dfc9ca0c---------------------bookmark_footer-----------)![](img/9db8b157fe2ab50887c47f5f7f1ac837.png)

GPT-3 为构成显示的句子的每个标记产生的对数概率，用于测试系统的图片。该图片由作者通过网页应用程序的截图组成，并链接在文章末尾。

随着强大的语言模型变得越来越普及，对其生成内容的控制需求变得愈加迫切。这些模型在大量文本数据的训练下，能够生成从新闻文章到社交媒体帖子等极具说服力的书面内容。然而，如果没有适当的监督，它们也可能产生虚假信息或各种有害内容。因此，使用这些语言模型的应用程序必须尝试检查这些 AI 系统生成的信息的真实性，以防止虚假、误导或有害信息的传播。

像许多人一样，可能包括你自己因为你正在阅读这篇文章，我已经相当多地使用了 GPT-3 和 ChatGPT，在这个过程中，我发现它们经常非常有说服力但却错误地回答问题。事实上，当 GPT-3 发布时，我深入探讨了这个问题，采用了类似于我会对学生进行的科学学科考试的形式：
