# Python 标准库中的 NaN 值

> 原文：[`towardsdatascience.com/nan-values-in-the-python-standard-library-798d9ed946c0?source=collection_archive---------9-----------------------#2023-10-07`](https://towardsdatascience.com/nan-values-in-the-python-standard-library-798d9ed946c0?source=collection_archive---------9-----------------------#2023-10-07)

## PYTHON 编程

## NaN 意味着 Not-a-Number。你可以在数值库中使用它——也可以在 Python 标准库中使用。

[](https://medium.com/@nyggus?source=post_page-----798d9ed946c0--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----798d9ed946c0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----798d9ed946c0--------------------------------)![数据科学进展](https://towardsdatascience.com/?source=post_page-----798d9ed946c0--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----798d9ed946c0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnan-values-in-the-python-standard-library-798d9ed946c0&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----798d9ed946c0---------------------post_header-----------) 发布于 [数据科学进展](https://towardsdatascience.com/?source=post_page-----798d9ed946c0--------------------------------) · 11 分钟阅读 · 2023 年 10 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F798d9ed946c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnan-values-in-the-python-standard-library-798d9ed946c0&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----798d9ed946c0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F798d9ed946c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnan-values-in-the-python-standard-library-798d9ed946c0&source=-----798d9ed946c0---------------------bookmark_footer-----------)![](img/f4310ba7fa2b1ab8177d7c517788116b.png)

图片由 [cyrus gomez](https://unsplash.com/@cyrusgomez?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

`NaN` 代表 *Not-a-Number*（非数字）。因此，一个 `NaN` 对象表示它的名字所传达的含义——即不是一个数字。它可以是一个缺失值，也可以是一个数值变量中的非数值。在纯粹的数值容器中我们不应该使用非数值，因此我们将这种值表示为 *not-a-number*，即 `NaN`。换句话说，我们可以说 `NaN` 代表一个 *缺失的数值*。

在本文中，我们将讨论 Python 标准库中可用的 `NaN` 对象。

`NaN` 值在数值数据中频繁出现。如果你对这个值的细节感兴趣，你可以在这里找到相关信息：

[](https://en.wikipedia.org/wiki/NaN?source=post_page-----798d9ed946c0--------------------------------) [## NaN - 维基百科

### 在计算中，NaN（），即“非数字”（Not a Number），是数值数据类型（通常是浮点数）的一个特定值。

en.wikipedia.org](https://en.wikipedia.org/wiki/NaN?source=post_page-----798d9ed946c0--------------------------------)

在本文中，我们不会讨论 `NaN` 值的所有细节。¹ 相反，我们将讨论如何在 Python 中处理 `NaN` 值的几个示例。
