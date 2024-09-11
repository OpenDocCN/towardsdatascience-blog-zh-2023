# OpenAI 功能调用简介

> 原文：[`towardsdatascience.com/an-introduction-to-openai-function-calling-e47e7cd7680e?source=collection_archive---------1-----------------------#2023-07-09`](https://towardsdatascience.com/an-introduction-to-openai-function-calling-e47e7cd7680e?source=collection_archive---------1-----------------------#2023-07-09)

## 不再有非结构化数据输出；将 ChatGPT 的完成结果转换为结构化的 JSON！

[](https://dkhundley.medium.com/?source=post_page-----e47e7cd7680e--------------------------------)![David Hundley](https://dkhundley.medium.com/?source=post_page-----e47e7cd7680e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e47e7cd7680e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e47e7cd7680e--------------------------------) [David Hundley](https://dkhundley.medium.com/?source=post_page-----e47e7cd7680e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F82498630db6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-openai-function-calling-e47e7cd7680e&user=David+Hundley&userId=82498630db6&source=post_page-82498630db6----e47e7cd7680e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e47e7cd7680e--------------------------------) ·16 分钟阅读·2023 年 7 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe47e7cd7680e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-openai-function-calling-e47e7cd7680e&user=David+Hundley&userId=82498630db6&source=-----e47e7cd7680e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe47e7cd7680e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-openai-function-calling-e47e7cd7680e&source=-----e47e7cd7680e---------------------bookmark_footer-----------)![](img/14ae6814c08319c738cd14cdd99e5771.png)

题图由作者创建

几个月前，OpenAI 向公众发布了他们的 API，这让许多希望系统化利用 ChatGPT 输出的开发者感到兴奋。虽然这非常令人兴奋，但对于我们这些程序员来说，也是一场噩梦，因为我们通常在 **结构化数据类型** 的领域工作。我们喜欢整数、布尔值和列表。处理非结构化字符串可能会很麻烦，为了获得一致的结果，程序员必须面对他们最害怕的事情：开发正则表达式（Regex）进行正确解析。🤢

当然，提示工程在这里确实可以帮上不少忙，但它仍然不完美。例如，如果你想让 ChatGPT 分析电影评论的情感倾向是积极还是消极，你可以构建一个类似这样的提示：

```py
prompt = f'''
Please perform a sentiment analysis on the following movie review:
{MOVIE_REVIEW_TEXT}
Please output your response as a single word: either "Positive" or "Negative".
'''
```

这个提示实际上表现得相当不错，但结果并不完全一致。例如，我见过 ChatGPT 在以下方面生成的输出…
