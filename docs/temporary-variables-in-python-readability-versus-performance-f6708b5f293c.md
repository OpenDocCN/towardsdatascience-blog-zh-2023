# Python中的临时变量：可读性与性能

> 原文：[https://towardsdatascience.com/temporary-variables-in-python-readability-versus-performance-f6708b5f293c?source=collection_archive---------10-----------------------#2023-06-01](https://towardsdatascience.com/temporary-variables-in-python-readability-versus-performance-f6708b5f293c?source=collection_archive---------10-----------------------#2023-06-01)

## PYTHON 编程

## 临时变量可以使代码更清晰。那么，这种代码的性能如何呢？

[](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporary-variables-in-python-readability-versus-performance-f6708b5f293c&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----f6708b5f293c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------) ·8 min read·2023年6月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff6708b5f293c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporary-variables-in-python-readability-versus-performance-f6708b5f293c&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----f6708b5f293c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff6708b5f293c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporary-variables-in-python-readability-versus-performance-f6708b5f293c&source=-----f6708b5f293c---------------------bookmark_footer-----------)![](../Images/df7237943e419001b72e9b50f1dae8b8.png)

Python快捷方式快吗？照片由 [Stefan Steinbauer](https://unsplash.com/@usinglight?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

临时变量是生命周期很短的变量：

[](https://en.wikipedia.org/wiki/Temporary_variable?source=post_page-----f6708b5f293c--------------------------------#:~:text=In%20computer%20programming%2C%20a%20temporary,a%20variable%20with%20local%20scope) [## 临时变量 - 维基百科

### 来自维基百科，自由的百科全书在计算机编程中，临时变量是具有短生命周期的变量…

[en.wikipedia.org](https://en.wikipedia.org/wiki/Temporary_variable?source=post_page-----f6708b5f293c--------------------------------#:~:text=In%20computer%20programming%2C%20a%20temporary,a%20variable%20with%20local%20scope)

它们在编程中经常被使用，并且你不必知道这个术语来使用临时变量。其中一个最常见的用例是使代码更清晰，例如，在流水线中：

```py
input → tempvar_1 := func_1(input) →
        tempvar_2 := func_2(tempvar_1) →
        func_3(tempvar_2) → output
```

在这里，我使用了Python的[海象操作符](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions)来直观地表示一个赋值，就像在Python代码中使用的方式一样。在这个流水线中，我们有两个临时变量：`tempvar_1` 和 `tempvar_2`。它们的生命周期在代码中数据流动的角度来看很短暂，尽管在实际时间上可能很长。`tempvar_1` 仅用于一个目的：将流水线的第一步的结果传递到下一步。但请注意，从技术上讲，它是不必要的：
