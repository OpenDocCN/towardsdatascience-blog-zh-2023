# 你可能犯的 6 个尴尬的 Sklearn 错误及如何避免

> 原文：[`towardsdatascience.com/6-embarrassing-sklearn-mistakes-you-may-be-making-and-how-to-avoid-them-6be5bbdbb987?source=collection_archive---------0-----------------------#2023-06-05`](https://towardsdatascience.com/6-embarrassing-sklearn-mistakes-you-may-be-making-and-how-to-avoid-them-6be5bbdbb987?source=collection_archive---------0-----------------------#2023-06-05)

## 没有错误信息——这就是它们的微妙之处

[](https://ibexorigin.medium.com/?source=post_page-----6be5bbdbb987--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----6be5bbdbb987--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6be5bbdbb987--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6be5bbdbb987--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----6be5bbdbb987--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-embarrassing-sklearn-mistakes-you-may-be-making-and-how-to-avoid-them-6be5bbdbb987&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----6be5bbdbb987---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6be5bbdbb987--------------------------------) ·10 分钟阅读·2023 年 6 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6be5bbdbb987&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-embarrassing-sklearn-mistakes-you-may-be-making-and-how-to-avoid-them-6be5bbdbb987&user=Bex+T.&userId=39db050c2ac2&source=-----6be5bbdbb987---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6be5bbdbb987&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-embarrassing-sklearn-mistakes-you-may-be-making-and-how-to-avoid-them-6be5bbdbb987&source=-----6be5bbdbb987---------------------bookmark_footer-----------)

学习如何通过 Sklearn 避免与机器学习理论相关的六个最严重的错误，这些错误初学者经常会犯。

![](img/e79383dd47c27048f5947ee350129670.png)

图片由我使用 Leonardo AI 制作

通常，当你犯错时，Sklearn 会抛出红色的错误信息和警告。这些信息表明你的代码中存在严重问题，阻止了 Sklearn 的魔法发挥作用。

但如果你没有得到任何错误或警告，那会发生什么？这是否意味着你到目前为止表现得很出色？*不一定*。许多调整和设置使得 Sklearn 成为最优秀的机器学习库，其世界级的 *代码设计* 就是一个例子。

编写 Sklearn 代码时的错误可以很容易地被修正。*可能* 被忽视的是与 Sklearn 算法和转换器驱动的 *内部逻辑* 和机器学习理论相关的错误。

这些错误在你还是初学者时尤其常见且微妙。因此，这篇文章将讨论我作为初学者时所犯的六个错误，并学习如何避免它们。

## 1️⃣. 到处使用 `fit` 或 `fit_transform`

让我们从最严重的错误开始——一个与 *数据泄露* 相关的错误。数据……
