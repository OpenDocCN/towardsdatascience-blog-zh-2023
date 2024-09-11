# ChatGPT API 的自定义内存

> 原文：[`towardsdatascience.com/custom-memory-for-chatgpt-api-artificial-intelligence-python-722d627d4d6d?source=collection_archive---------0-----------------------#2023-08-20`](https://towardsdatascience.com/custom-memory-for-chatgpt-api-artificial-intelligence-python-722d627d4d6d?source=collection_archive---------0-----------------------#2023-08-20)

## 对 LangChain 内存类型的温和介绍

[](https://medium.com/@andvalenzuela?source=post_page-----722d627d4d6d--------------------------------)![Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----722d627d4d6d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----722d627d4d6d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----722d627d4d6d--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----722d627d4d6d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f3f1654c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-memory-for-chatgpt-api-artificial-intelligence-python-722d627d4d6d&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=post_page-a6f3f1654c3----722d627d4d6d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----722d627d4d6d--------------------------------) ·8 分钟阅读·2023 年 8 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F722d627d4d6d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-memory-for-chatgpt-api-artificial-intelligence-python-722d627d4d6d&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=-----722d627d4d6d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F722d627d4d6d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-memory-for-chatgpt-api-artificial-intelligence-python-722d627d4d6d&source=-----722d627d4d6d---------------------bookmark_footer-----------)![](img/a89558351cd6f8d41f0994f05d4caebe.png)

自制的 gif。

如果你曾经使用过 OpenAI API，我相信你已经注意到了这一点。

*明白了吗？*

*没错!* 每次你调用 ChatGPT API 时，模型都没有记忆你之前的请求。换句话说：**每次 API 调用都是一个独立的交互**。

当你需要与模型进行后续交互时，这绝对令人烦恼。聊天机器人是需要进行后续交互的经典例子。

在这篇文章中，我们将探讨如何在使用 OpenAI API 时为 ChatGPT 提供记忆，使其能够记住我们之前的交互。

# 热身！

让我们与模型进行一些互动，以便体验这种默认的无记忆现象：

```py
prompt = "My name is Andrea"
response = chatgpt_call(prompt)
print(response)

# Output: Nice to meet you, Andrea! How can I assist you today?
```

但是当被问及后续问题时：

```py
prompt = "Do you remember my name?"
response = chatgpt_call(prompt)
print(response)

# Output: I'm sorry, as an AI language model, I don't have the ability 
# to remember specific information about individual users.
```
