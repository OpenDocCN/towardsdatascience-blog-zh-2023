# 使用 NLP 进行上下文文本纠正

> 原文：[`towardsdatascience.com/contextual-text-correction-using-nlp-81a1363c5fc3?source=collection_archive---------17-----------------------#2023-01-18`](https://towardsdatascience.com/contextual-text-correction-using-nlp-81a1363c5fc3?source=collection_archive---------17-----------------------#2023-01-18)

## **检测和纠正涉及建模上下文的错误**

[](https://jagota-arun.medium.com/?source=post_page-----81a1363c5fc3--------------------------------)![阿伦·贾戈塔](https://jagota-arun.medium.com/?source=post_page-----81a1363c5fc3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81a1363c5fc3--------------------------------)![数据科学之路](https://towardsdatascience.com/?source=post_page-----81a1363c5fc3--------------------------------) [阿伦·贾戈塔](https://jagota-arun.medium.com/?source=post_page-----81a1363c5fc3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontextual-text-correction-using-nlp-81a1363c5fc3&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----81a1363c5fc3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81a1363c5fc3--------------------------------) ·23 分钟阅读·2023 年 1 月 18 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81a1363c5fc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontextual-text-correction-using-nlp-81a1363c5fc3&user=Arun+Jagota&userId=ef9ed921edad&source=-----81a1363c5fc3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81a1363c5fc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontextual-text-correction-using-nlp-81a1363c5fc3&source=-----81a1363c5fc3---------------------bookmark_footer-----------)![](img/bcadaafc6c2554d8c8a66ada008560c7.png)

图片由 [洛伦佐·卡法罗](https://pixabay.com/users/3844328-3844328/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1870721) 提供，来自 [Pixabay](https://pixabay.com/)

在上一篇文章中，我们讨论了使用统计自然语言处理方法检测和纠正文本中的常见错误的问题：

[](/text-correction-using-nlp-b68c7233b86?source=post_page-----81a1363c5fc3--------------------------------) ## 使用 NLP 进行文本纠正

### 检测和纠正常见错误：问题与方法

[towardsdatascience.com

那里我们列举了几个问题，并附有真实例子和讨论。以下是我们在那篇文章中没有完全解决的问题。（最后两个甚至都没有触及。）这些是需要处理背景的问题。

+   缺少逗号。

+   缺少或不正确的冠词。

+   使用单数而不是复数，或反之。

+   使用错误的介词或其他连接词。

在这篇文章中，我们从涉及冠词的问题开始。我们看了这种情况的详细例子，并深入研究了我们所说的每一个“问题”是什么意思。

然后我们描述了一种解决方法。它使用了自我监督的关键概念。
