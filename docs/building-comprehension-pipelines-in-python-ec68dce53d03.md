# 在 Python 中构建理解管道

> 原文：[`towardsdatascience.com/building-comprehension-pipelines-in-python-ec68dce53d03?source=collection_archive---------4-----------------------#2023-02-17`](https://towardsdatascience.com/building-comprehension-pipelines-in-python-ec68dce53d03?source=collection_archive---------4-----------------------#2023-02-17)

## PYTHON 编程

## 理解管道是一个特定于 Python 的构建管道的概念

[](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-comprehension-pipelines-in-python-ec68dce53d03&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----ec68dce53d03---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------) · 12 分钟阅读 · 2023 年 2 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fec68dce53d03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-comprehension-pipelines-in-python-ec68dce53d03&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----ec68dce53d03---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec68dce53d03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-comprehension-pipelines-in-python-ec68dce53d03&source=-----ec68dce53d03---------------------bookmark_footer-----------)![](img/686b7a1a7f83a53912af9f9378a8391c.png)

理解管道将你直接带到目标。照片由[Anika Huizinga](https://unsplash.com/@iam_anih?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

生成器管道提供了一种 Pythonic 的方式来创建软件管道，即操作链，其中每个操作（除了第一个）都将前一个操作的输出作为输入：

[](/building-generator-pipelines-in-python-8931535792ff?source=post_page-----ec68dce53d03--------------------------------) ## 在 Python 中构建生成器管道

### 本文提出了一种优雅的方式来构建生成器管道

[towardsdatascience.com

它们使你能够应用**Thomas**和**Hunt**在他们伟大的书籍*《程序员修炼之道》*中描述的转换编程方法。

[## 程序员修炼之道 - 维基百科](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer?source=post_page-----ec68dce53d03--------------------------------)

### 来自维基百科，自由百科全书 *《程序员修炼之道：从学徒到大师》* 是一本关于计算机的书…

[维基百科](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer?source=post_page-----ec68dce53d03--------------------------------)

在 Python 中，一个典型的生成器管道在管道的每一步使用一个生成器。换句话说，管道的每一步都是构建为生成器的。**Thomas**和**Hunt**讨论了通过管道操作符实现的管道，该操作符在许多编程语言中都可用。虽然 Python 没有…
