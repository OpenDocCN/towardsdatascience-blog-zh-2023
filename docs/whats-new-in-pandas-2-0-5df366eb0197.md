# Pandas 2.0 有什么新变化？

> 原文：[https://towardsdatascience.com/whats-new-in-pandas-2-0-5df366eb0197?source=collection_archive---------1-----------------------#2023-04-10](https://towardsdatascience.com/whats-new-in-pandas-2-0-5df366eb0197?source=collection_archive---------1-----------------------#2023-04-10)

## 了解关于这次重大发布的五件事

[](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)[![Jeff Hale](../Images/11d534200a7fdc5d997fa2ddbc66132b.png)](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------) [Jeff Hale](https://jeffhale.medium.com/?source=post_page-----5df366eb0197--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F451599b1142a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-0-5df366eb0197&user=Jeff+Hale&userId=451599b1142a&source=post_page-451599b1142a----5df366eb0197---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5df366eb0197--------------------------------) ·5分钟阅读·2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5df366eb0197&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-0-5df366eb0197&user=Jeff+Hale&userId=451599b1142a&source=-----5df366eb0197---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5df366eb0197&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-0-5df366eb0197&source=-----5df366eb0197---------------------bookmark_footer-----------)

Pandas 2.0 于 2023 年 4 月 3 日正式发布。让我们来看看有哪些功能比阳光下的柯基还要火热。☀️

![](../Images/64c4f87ef9ed00d3cb56323cded1e96c.png)

来源：pixabay.com

三年前我写了[《Pandas 1.0 的新变化》](/whats-new-in-pandas-1-0-ffa99bd43a58)。经历了一场大流行和一系列人工智能的进展后，我们现在迎来了 Pandas 2.0。

Pandas 是处理数据的标准、脑友好的 Python 库。2.0 更新的重点是让 pandas 更快、更节省内存。内存是人们离开 pandas 使用 Dask、Ray、SQL 数据库、Spark DataFrames 和其他工具的主要原因。你在使用 pandas 时减少内存使用的越多，生活就会变得越轻松。 🙂

正如你可能预料的那样，作为一个重大版本更新，pandas 2.0 有许多重要的变化。让我们深入了解一下吧！

## pyarrow *🐍➡️*

如果用一个词来总结这个版本，那就是*pyarrow*。

Pandas 是使用 NumPy 数据结构进行内存管理构建的。现在，你可以选择使用 [pyarrow](https://arrow.apache.org/docs/python/index.html) 作为你的后台内存格式。

使用 pyarrow 可以加快速度并使操作更节省内存，因为你可以利用 [Arrow](https://arrow.apache.org/docs/cpp/index.html) 的 C++ 实现。有趣的是，pandas 的创建者 [Wes McKinney](https://en.wikipedia.org/wiki/Wes_McKinney) 后来也参与了 Arrow 的开发…
