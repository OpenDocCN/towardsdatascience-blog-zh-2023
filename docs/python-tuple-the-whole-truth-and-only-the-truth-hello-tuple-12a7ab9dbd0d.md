# Python 元组，全部真相与唯一真相：你好，元组！

> 原文：[`towardsdatascience.com/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d?source=collection_archive---------0-----------------------#2023-01-21`](https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d?source=collection_archive---------0-----------------------#2023-01-21)

## PYTHON PROGRAMMING

## 学习元组的基础知识及其用法

[](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----12a7ab9dbd0d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------) ·16 分钟阅读·2023 年 1 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F12a7ab9dbd0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----12a7ab9dbd0d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12a7ab9dbd0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d&source=-----12a7ab9dbd0d---------------------bookmark_footer-----------)![](img/74860d4642efa6cd08de3d7411205690.png)

元组通常被认为是记录。照片由 [Samuel Regan-Asante](https://unsplash.com/@fkaregan?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

元组是 Python 中一种不可变的集合类型。它是 Python 中三种最流行的集合类型之一，另外两种是列表和字典。我认为许多初学者和中级开发者对这两种类型了解很多，但他们可能会在真正理解元组是什么以及如何工作的方面遇到问题。即使是高级 Python 开发者也不必了解有关元组的所有内容——鉴于该类型的特性，这对我来说并不令人惊讶。

作为一个初学者甚至中级的 Python 开发者，我对元组了解不多。让我给你一个例子；假设我写了一段类似以下的代码：

```py
from pathlib import Path

ROOT = Path(__file__).resolve().parent

basic_names = [
    "file1",
    "file2",
    "file_miss_x56",
    "xyz_settings",
]
files = [
    Path(ROOT) / f"{name}.csv"
    for name in basic_names
]
```

如你所见，我使用了列表字面量来定义 `basic_names` 列表——但为什么不使用 *元组字面量* 呢？它看起来会是这样：

```py
basic_names = (
    "file1",
    "file2",
    "file_miss_x56",
    "xyz_settings",
)
```
