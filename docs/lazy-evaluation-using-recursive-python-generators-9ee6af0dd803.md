# 使用递归 Python 生成器的惰性求值

> 原文：[`towardsdatascience.com/lazy-evaluation-using-recursive-python-generators-9ee6af0dd803?source=collection_archive---------16-----------------------#2023-01-04`](https://towardsdatascience.com/lazy-evaluation-using-recursive-python-generators-9ee6af0dd803?source=collection_archive---------16-----------------------#2023-01-04)

## 递归函数能否使用 *“惰性求值”*？——是的，它们可以——通过使用 Python 的生成器函数！

[](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)![Martin Heinz](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------) [Martin Heinz](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4cbd6b36e62a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flazy-evaluation-using-recursive-python-generators-9ee6af0dd803&user=Martin+Heinz&userId=4cbd6b36e62a&source=post_page-4cbd6b36e62a----9ee6af0dd803---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------) · 5 min read · 2023 年 1 月 4 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9ee6af0dd803&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flazy-evaluation-using-recursive-python-generators-9ee6af0dd803&user=Martin+Heinz&userId=4cbd6b36e62a&source=-----9ee6af0dd803---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9ee6af0dd803&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flazy-evaluation-using-recursive-python-generators-9ee6af0dd803&source=-----9ee6af0dd803---------------------bookmark_footer-----------)![](img/84db954d46d54cd7ea660368b0737fa3.png)

使用 Stable Diffusion 生成

我们都熟悉 Python 的生成器及其所有好处。但如果我告诉你，我们可以通过将其与递归结合来让它们更强大呢？那么，让我们来看看如何利用它们实现 *“惰性递归”* 并极大提升我们在 Python 中使用生成器的能力！

# 为什么要费心呢？

在我们深入代码之前，先问自己 *“为什么要费心？我们真的需要递归生成器吗？”*。答案是……这要看情况。自然，递归生成器将共享生成器以及普通递归函数的优缺点。

对于生成器，使用它们的首要原因是*“懒惰”*计算——即一次计算一个元素，而不是一次性计算所有元素。至于递归，它在某些算法或问题中显得非常合理，可以优雅而简洁地解决问题，比如树遍历。

因此，递归生成器会显得合理的情况自然是递归算法，这些算法可能处理大量数据或元素，因此如果*“贪婪”*地运行，可能会消耗大量内存。
