# 如何使用 OpenAI 的函数调用

> 原文：[https://towardsdatascience.com/how-to-use-openais-function-calling-e35bdac88ae7?source=collection_archive---------10-----------------------#2023-07-25](https://towardsdatascience.com/how-to-use-openais-function-calling-e35bdac88ae7?source=collection_archive---------10-----------------------#2023-07-25)

## 函数调用概述及其对构建 LLM 应用程序的影响

[](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-function-calling-e35bdac88ae7&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----e35bdac88ae7---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------) 发表 ·6 分钟阅读·2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe35bdac88ae7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-function-calling-e35bdac88ae7&user=John+Adeojo&userId=f933e1637e40&source=-----e35bdac88ae7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe35bdac88ae7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-function-calling-e35bdac88ae7&source=-----e35bdac88ae7---------------------bookmark_footer-----------)![](../Images/a09010bff67cd967314369c4fcabb3a3.png)

作者提供的图片：使用 Midjourney 生成

# 结构化非结构化内容

函数调用是 [OpenAI](https://openai.com/) 的一项创新，它扩展了围绕大型语言模型进行应用程序开发的可能性。

然而，我发现它仍然被一些人误解。在这篇文章中，我旨在在你冲一杯咖啡的时间里，阐明函数调用的概念。

如果你有志于构建 LLM 应用程序，将 LLM 集成到你的业务中，或者只是想在这个领域扩展你的知识，那么这篇文章适合你。

# 函数调用的伟大之处在哪里？

函数调用允许我们在现有 API 的基础上开发自然语言接口。如果这听起来让你感到困惑，不用担心——随着你继续阅读，细节会变得更加清晰。

那么，自然语言 API 看起来是什么样的呢？我相信用图示来演示是最好的。这里有一个示例应用，它使用函数调用来帮助用户查找航班。
