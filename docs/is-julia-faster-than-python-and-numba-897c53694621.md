# Julia 比 Python 和 Numba 更快吗？

> 原文：[`towardsdatascience.com/is-julia-faster-than-python-and-numba-897c53694621?source=collection_archive---------0-----------------------#2023-09-19`](https://towardsdatascience.com/is-julia-faster-than-python-and-numba-897c53694621?source=collection_archive---------0-----------------------#2023-09-19)

## 优化

## Numba 非常快，但它够快吗？

[](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)![Mike Clayton](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)[](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------) [Mike Clayton](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51dce1c5bc03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-julia-faster-than-python-and-numba-897c53694621&user=Mike+Clayton&userId=51dce1c5bc03&source=post_page-51dce1c5bc03----897c53694621---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------) ·16 分钟阅读·2023 年 9 月 19 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F897c53694621&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-julia-faster-than-python-and-numba-897c53694621&user=Mike+Clayton&userId=51dce1c5bc03&source=-----897c53694621---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F897c53694621&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-julia-faster-than-python-and-numba-897c53694621&source=-----897c53694621---------------------bookmark_footer-----------)![](img/69fa7749831310e9eb6a02054d9ee87d.png)

图片由 [Stanos](https://unsplash.com/@stan_remnev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/2Yj6MBvJ0sg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**Numba 是一个广泛使用的 Python 优化库，它将函数执行时间提升到与 C 语言相当的水平，而 C 语言无疑是快速的。**

**这种优化水平足以与像 Julia 这样的新型、专门构建的、针对性的语言竞争吗？如果可以的话，Python 实现这种执行速度有什么注意事项？**

# 引言

![](img/4aa570daa0aa0c472205cffbdcfa200e.png)

图片由 [Ann H](https://www.pexels.com/photo/yellow-letter-tiles-1888003/) 提供，来自 [Pexels](https://www.pexels.com/#)

我之前写过一篇 比较 NumPy 和 Julia 的文章。结果基本上是 Julia 确实比 NumPy 更快，通常情况下。不过，这比那更复杂一些，因此我鼓励你查看那篇文章，了解完整的故事。

[](/is-julia-really-faster-than-python-and-numpy-242e0a5fe34f?source=post_page-----897c53694621--------------------------------) ## Julia 是否真的比 Python 和 Numpy 更快？

### C 的速度与 Python 的简单性

towardsdatascience.com

对那篇文章最常见的回应之一是类似于以下内容：
