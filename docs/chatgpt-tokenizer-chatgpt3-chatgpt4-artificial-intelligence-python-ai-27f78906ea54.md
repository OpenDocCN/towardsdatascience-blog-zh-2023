# 解放 ChatGPT 令牌器

> 原文：[`towardsdatascience.com/chatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54?source=collection_archive---------3-----------------------#2023-07-06`](https://towardsdatascience.com/chatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54?source=collection_archive---------3-----------------------#2023-07-06)

## 实操！ChatGPT 如何管理令牌？

[](https://medium.com/@andvalenzuela?source=post_page-----27f78906ea54--------------------------------)![安德烈亚·巴伦苏埃拉](https://medium.com/@andvalenzuela?source=post_page-----27f78906ea54--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27f78906ea54--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----27f78906ea54--------------------------------) [安德烈亚·巴伦苏埃拉](https://medium.com/@andvalenzuela?source=post_page-----27f78906ea54--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f3f1654c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=post_page-a6f3f1654c3----27f78906ea54---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----27f78906ea54--------------------------------) · 9 分钟阅读 · 2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F27f78906ea54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=-----27f78906ea54---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F27f78906ea54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54&source=-----27f78906ea54---------------------bookmark_footer-----------)![](img/d6d00dc92aaafbfe984040c4f28c2509.png)

自制 gif。

**你是否曾经想过 ChatGPT 背后的关键组件是什么？**

我们都被告知同样的事情：ChatGPT 预测下一个词。但实际上，这种说法有点不准确。**它并不预测下一个词，ChatGPT 预测的是下一个令牌**。

*令牌？* 是的，令牌是大型语言模型（LLMs）的文本单位。

的确，**ChatGPT 在处理任何提示时的第一步就是将用户输入分割成令牌**。而这正是所谓的 **令牌器** 的工作。

在这篇文章中，我们将揭示 ChatGPT 分词器如何工作，并通过 OpenAI 原用的`tiktoken`库进行实践操作。

*TikTok-en… 有趣得很 :)*

让我们深入探讨分词器实际执行的步骤，以及它的行为如何真正影响 ChatGPT 输出的质量。

# 分词器如何工作

在文章掌握 ChatGPT：利用 LLM 进行有效总结中，我们已经看到了一些 ChatGPT 分词器的奥秘，但让我们从头开始。

分词器出现在文本生成过程的第一步。**它是**…
