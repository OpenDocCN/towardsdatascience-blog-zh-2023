# 跟我说：一个模型正在阅读多少个词

> 原文：[https://towardsdatascience.com/speak-to-me-how-many-words-a-model-is-reading-331e3af86d27?source=collection_archive---------5-----------------------#2023-07-14](https://towardsdatascience.com/speak-to-me-how-many-words-a-model-is-reading-331e3af86d27?source=collection_archive---------5-----------------------#2023-07-14)

## | 人工智能 | 大型语言模型 | 自然语言处理 |
## | --- | --- | --- |

## 为什么以及如何克服大型语言模型的内在限制

[](https://salvatore-raieli.medium.com/?source=post_page-----331e3af86d27--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----331e3af86d27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----331e3af86d27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----331e3af86d27--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----331e3af86d27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeak-to-me-how-many-words-a-model-is-reading-331e3af86d27&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----331e3af86d27---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----331e3af86d27--------------------------------) ·20分钟阅读·2023年7月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F331e3af86d27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeak-to-me-how-many-words-a-model-is-reading-331e3af86d27&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----331e3af86d27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F331e3af86d27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeak-to-me-how-many-words-a-model-is-reading-331e3af86d27&source=-----331e3af86d27---------------------bookmark_footer-----------)![](../Images/e002ac8712d8db5b9d0e12618db308e8.png)

图片由 [C D-X](https://unsplash.com/@cdx2?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[大型语言模型](https://en.wikipedia.org/wiki/Large_language_model) 最近展示了它们的技能，证明它们在各种任务中都很熟练。所有这些都是通过一种互动模式：提示。

最近几个月，语言模型的背景范围被迅速扩展。**但这对语言模型有什么影响？**

本文分为不同的部分，每个部分我们将回答这些问题：

+   什么是提示（prompt），如何构建一个好的提示？

+   什么是上下文窗口？它可以有多长？是什么限制了模型输入序列的长度？这为什么重要？

+   我们如何克服这些限制？

+   模型是否使用长上下文窗口？

# 如何与模型进行对话？

![](../Images/ec3fcf415b52be011dc84ba7b7c582fb.png)

图片由[Jamie Templeton](https://unsplash.com/@jamietempleton?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 什么是提示（prompt），什么是好的提示？
