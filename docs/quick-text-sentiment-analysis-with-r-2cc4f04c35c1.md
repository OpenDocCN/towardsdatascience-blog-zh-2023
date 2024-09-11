# 使用 R 进行快速文本情感分析

> 原文：[https://towardsdatascience.com/quick-text-sentiment-analysis-with-r-2cc4f04c35c1?source=collection_archive---------4-----------------------#2023-03-10](https://towardsdatascience.com/quick-text-sentiment-analysis-with-r-2cc4f04c35c1?source=collection_archive---------4-----------------------#2023-03-10)

## 使用 TidyText 在 R 中创建快速且高效的文本分析

[](https://gustavorsantos.medium.com/?source=post_page-----2cc4f04c35c1--------------------------------)[![Gustavo Santos](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----2cc4f04c35c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2cc4f04c35c1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2cc4f04c35c1--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----2cc4f04c35c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-text-sentiment-analysis-with-r-2cc4f04c35c1&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----2cc4f04c35c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2cc4f04c35c1--------------------------------) ·9 min read·2023年3月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2cc4f04c35c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-text-sentiment-analysis-with-r-2cc4f04c35c1&user=Gustavo+Santos&userId=4429d99b1245&source=-----2cc4f04c35c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2cc4f04c35c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-text-sentiment-analysis-with-r-2cc4f04c35c1&source=-----2cc4f04c35c1---------------------bookmark_footer-----------)![](../Images/5960e531934e6170b542d5661e786195.png)

图片由 [Kenny Eliason](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/jxmVsYjglnQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

文字无处不在！自从互联网遍布全球以来，我们每天生成的文本数据量巨大。仅每日发送的文本消息，据估计每天大约有180亿条在[日常交流中](https://seedscientific.com/how-much-data-is-created-every-day/)。

现在也要想象生成的新闻量。这是一个令人难以承受的庞大数量，因此有整个行业围绕新闻剪辑建立，以分离关于特定主题的最佳信息，帮助公司制定营销策略。

人工智能如何帮助这方面？显然，自然语言处理（NLP）在这方面发挥了重要作用，提供了良好的工具和算法来分析文本信息。作为数据科学家，我们可以利用`tidytext`，这是`R`中的一个优秀库，帮助我们快速构建分析工具，以检查文本内容。

接下来，我们来看实际操作。

# 文本分析

## 准备你的环境

为了能够跟着本文一起编程，请加载以下列出的库。

```py
# Installing libraries
install.packages('tidyverse')
install.packages('tidytext')

# Loading libraries…
```
