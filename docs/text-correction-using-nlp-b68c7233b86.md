# 使用 NLP 的文本纠正

> 原文：[`towardsdatascience.com/text-correction-using-nlp-b68c7233b86?source=collection_archive---------11-----------------------#2023-01-13`](https://towardsdatascience.com/text-correction-using-nlp-b68c7233b86?source=collection_archive---------11-----------------------#2023-01-13)

## **检测和纠正常见错误：问题与方法**

[](https://jagota-arun.medium.com/?source=post_page-----b68c7233b86--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----b68c7233b86--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b68c7233b86--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b68c7233b86--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----b68c7233b86--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-correction-using-nlp-b68c7233b86&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----b68c7233b86---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b68c7233b86--------------------------------) ·19 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb68c7233b86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-correction-using-nlp-b68c7233b86&user=Arun+Jagota&userId=ef9ed921edad&source=-----b68c7233b86---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb68c7233b86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-correction-using-nlp-b68c7233b86&source=-----b68c7233b86---------------------bookmark_footer-----------)![](img/bcadaafc6c2554d8c8a66ada008560c7.png)

图片由 [Lorenzo Cafaro](https://pixabay.com/users/3844328-3844328/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1870721) 提供，来自 [Pixabay](https://pixabay.com/)

任何人写文本时都会漏掉某个逗号。或者在某些上下文中使用错误的介词。或者拼写错误。或者措辞笨拙。或者使用过于复杂或过长的句子。或者段落过长。或者过度废话。

在所有写作中，除了最简短的那些，几乎都可能存在上述问题，甚至更多。

我有一个学生在写的所有内容中都漏掉了如 *a* 或 *the* 这样的冠词。我读了他几百页的文章。一个冠词都没找到。

我也曾一再犯下这些错误，并且至今仍然如此。即便是在我的简短写作中，如电子邮件。

对于内容非常详细的材料，如整本书籍或甚至短小的博客文章，文本问题自然会更加繁多。这就是为什么我们需要编辑员，他们的职责包括校对和编辑内容。

这也是为什么基于 NLP 的工具，如 Grammarly，越来越受欢迎。这些工具可以帮助人们在几分钟内找到并纠正短文本中的错误，如电子邮件。对于较长的文本，它们可能会发现更多错误，这当然意味着修正这些错误会花费更多时间。
