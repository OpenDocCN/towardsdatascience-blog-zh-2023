# BERT与GPT：比较NLP巨头

> 原文：[https://towardsdatascience.com/bert-vs-gpt-comparing-the-nlp-giants-329d105e34ec?source=collection_archive---------1-----------------------#2023-08-20](https://towardsdatascience.com/bert-vs-gpt-comparing-the-nlp-giants-329d105e34ec?source=collection_archive---------1-----------------------#2023-08-20)

## 它们的结构有何不同，这些差异如何影响模型的能力？

[](https://medium.com/@vuphuongthao9611?source=post_page-----329d105e34ec--------------------------------)[![Thao Vu](../Images/9d44a2f199cdc9c29da72d9dc4971561.png)](https://medium.com/@vuphuongthao9611?source=post_page-----329d105e34ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----329d105e34ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----329d105e34ec--------------------------------) [Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----329d105e34ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa836aac352ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbert-vs-gpt-comparing-the-nlp-giants-329d105e34ec&user=Thao+Vu&userId=a836aac352ca&source=post_page-a836aac352ca----329d105e34ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----329d105e34ec--------------------------------) ·7分钟阅读·2023年8月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F329d105e34ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbert-vs-gpt-comparing-the-nlp-giants-329d105e34ec&user=Thao+Vu&userId=a836aac352ca&source=-----329d105e34ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F329d105e34ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbert-vs-gpt-comparing-the-nlp-giants-329d105e34ec&source=-----329d105e34ec---------------------bookmark_footer-----------)![](../Images/2a6c1c6d9546726bba7434946b0bcdc0.png)

图片由作者使用稳定扩散生成。

2018年，NLP研究人员都被BERT论文[1]所震惊。这种方法简单，但结果令人印象深刻：它为11个NLP任务设定了新的基准。

> 不到一年时间，BERT已成为自然语言处理（NLP）实验中的普遍基准，超过150篇研究论文分析和改进了该模型。[2]

2022年，ChatGPT [3]以其生成类似人类的回应能力引爆了整个互联网。该模型能够理解广泛的话题，并能自然地进行长时间对话，这使其与所有传统聊天机器人不同。

BERT 和 ChatGPT 是自然语言处理领域的重大突破，但它们的方法不同。它们的结构有何不同，它们如何影响模型的能力？让我们深入探讨一下吧！

# 注意事项

我们必须首先回顾常用的注意力机制，以充分理解模型结构。注意力机制旨在捕捉和建模序列中标记之间的关系，这也是它们在自然语言处理任务中取得巨大成功的原因之一。

## 一个直观的…
