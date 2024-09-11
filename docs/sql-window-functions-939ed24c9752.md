# 利用 SQL 中的分析函数进行更快的数据提取

> 原文：[`towardsdatascience.com/sql-window-functions-939ed24c9752?source=collection_archive---------19-----------------------#2023-07-18`](https://towardsdatascience.com/sql-window-functions-939ed24c9752?source=collection_archive---------19-----------------------#2023-07-18)

## 分析函数提供了一种非常强大且易于实现的数据处理和分析方式。本文将展示如何在 SQL 语句中使用分析函数。

[](https://guenterroehrich.medium.com/?source=post_page-----939ed24c9752--------------------------------)![Günter Röhrich](https://guenterroehrich.medium.com/?source=post_page-----939ed24c9752--------------------------------)[](https://towardsdatascience.com/?source=post_page-----939ed24c9752--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----939ed24c9752--------------------------------) [Günter Röhrich](https://guenterroehrich.medium.com/?source=post_page-----939ed24c9752--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3718a9423801&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-window-functions-939ed24c9752&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=post_page-3718a9423801----939ed24c9752---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----939ed24c9752--------------------------------) ·8 分钟阅读·2023 年 7 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F939ed24c9752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-window-functions-939ed24c9752&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=-----939ed24c9752---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F939ed24c9752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-window-functions-939ed24c9752&source=-----939ed24c9752---------------------bookmark_footer-----------)

作为一名分析专业人员，你可能会发现自己需要查询数据以进行分析。通常，数据是从 SQL 数据库中获取的，然后通过如 Pandas 或 NumPy 等强大框架的编程语言如 Python 导入。这是一种处理数据的完全可行的管道，然而，繁重的处理主要由本地机器完成。对于小数据集来说这不是问题，但当涉及到较大的数据集时，可能会遇到仅在 PC 本地内存中进行重处理的问题。

*你可能认为这不是一个常见的问题。因此，让我给一个日常的例子来证明这个假设是错误的：*

想象一下你在一家制造公司工作，并且你对收集机器传感器数据感兴趣。这些数据通常会被频繁收集，并且可能非常嘈杂。为了更好地了解你的机器情况，对密集收集的数据进行平滑和预处理（例如，测量可能每秒收集几次）很快会导致数据集的规模巨大！假设我们有 150 个传感器放置在…
