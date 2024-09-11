# GPT-3.5 更擅长翻译段落

> 原文：[https://towardsdatascience.com/gpt-3-5-translates-paragraphs-better-5ed0031ece08?source=collection_archive---------10-----------------------#2023-05-25](https://towardsdatascience.com/gpt-3-5-translates-paragraphs-better-5ed0031ece08?source=collection_archive---------10-----------------------#2023-05-25)

## 并且在文学作品的翻译中优于 Google Translate

[](https://medium.com/@bnjmn_marie?source=post_page-----5ed0031ece08--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----5ed0031ece08--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ed0031ece08--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ed0031ece08--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----5ed0031ece08--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-3-5-translates-paragraphs-better-5ed0031ece08&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----5ed0031ece08---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ed0031ece08--------------------------------) ·9 分钟阅读·2023年5月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ed0031ece08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-3-5-translates-paragraphs-better-5ed0031ece08&user=Benjamin+Marie&userId=ad2a414578b3&source=-----5ed0031ece08---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ed0031ece08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-3-5-translates-paragraphs-better-5ed0031ece08&source=-----5ed0031ece08---------------------bookmark_footer-----------)![](../Images/ef7e5310e5962ca4223e72e1bf1cc46c.png)

图片来源：[Pixabay](https://pixabay.com/vectors/translate-language-translation-6089103/)

根据[之前的研究](https://arxiv.org/pdf/2211.09102.pdf)，GPT模型的表现与标准机器翻译系统（例如 Google Translate）一样好。

这些研究大多集中在句子级翻译上：机器翻译中使用的默认方法是逐句翻译，没有任何上下文。 

翻译段落或整个文档对标准机器翻译系统来说是非常困难的挑战。这些系统通常需要拆分输入或进行复杂的工程调整，以接受和利用更长的输入。

然而，从直观上看，遵循人工翻译的工作流程，我们可以期望机器翻译系统在处理上下文时表现更好，例如翻译整个文档或段落。

这正是大型语言模型如GPT模型可以发挥作用的地方。它们可以处理比典型机器翻译系统更长的输入提示。

但仍需评估以下内容：

1.  利用更多上下文是否有助于提高GPT的机器翻译质量。

1.  GPT模型在翻译长文本时的表现与标准机器翻译系统的比较。
