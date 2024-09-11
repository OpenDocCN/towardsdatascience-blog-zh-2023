# Python 中的 bug？Pdb 来救援！

> 原文：[`towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71?source=collection_archive---------11-----------------------#2023-09-21`](https://towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71?source=collection_archive---------11-----------------------#2023-09-21)

## PYTHON 编程

## Pdb 调试器值得学习和使用吗？

[](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbugs-in-python-pdb-to-the-rescue-d88a56a2ca71&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----d88a56a2ca71---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------) · 13 分钟阅读 · 2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd88a56a2ca71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbugs-in-python-pdb-to-the-rescue-d88a56a2ca71&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----d88a56a2ca71---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd88a56a2ca71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbugs-in-python-pdb-to-the-rescue-d88a56a2ca71&source=-----d88a56a2ca71---------------------bookmark_footer-----------)![](img/dd08cfd6f7862fe8ce97e843516e22a3.png)

调试帮助你从失败中学习。图片来源：[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

各种工具可以用来调试 Python 代码，从最简单的 `print()` 函数，到静态但更先进的 `[icecream](https://github.com/gruns/icecream)` 及其兄弟 `[ycecream](https://github.com/salabim/ycecream)`，再到 IDE 提供的各种交互式调试器。然而，我的选择一直是内置的 `pdb` 调试器以及内置的 `breakpoint()` 函数。

# 调试

调试是编程的核心。你开始学习编程时就开始调试，当你承诺自己已经写完了最后一行代码时你就停止调试——如果你能遵守这个承诺的话。

你可能会认为，减少调试时间的一种方法是编写优质代码。事实是：编写优质代码往往意味着在开发过程中需要进行大量调试。当然，一名优秀的程序员会编写更好的代码，犯更少的错误——但这并不意味着他或她不需要调试。

然而，有一种方法可以减少调试：为了减少调试，编写良好的单元测试。

> 为了减少调试，编写良好的单元测试。

无论你是否使用[测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，都要编写好的测试。编写*好的*…
