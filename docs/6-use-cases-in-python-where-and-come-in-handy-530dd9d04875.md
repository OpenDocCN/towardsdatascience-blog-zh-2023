# 6 个 Python 使用 * 和 ** 的场景

> 原文：[`towardsdatascience.com/6-use-cases-in-python-where-and-come-in-handy-530dd9d04875?source=collection_archive---------10-----------------------#2023-06-19`](https://towardsdatascience.com/6-use-cases-in-python-where-and-come-in-handy-530dd9d04875?source=collection_archive---------10-----------------------#2023-06-19)

## 通过示例解释

[](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)[](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-use-cases-in-python-where-and-come-in-handy-530dd9d04875&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----530dd9d04875---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------) ·5 min read·Jun 19, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F530dd9d04875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-use-cases-in-python-where-and-come-in-handy-530dd9d04875&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----530dd9d04875---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F530dd9d04875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-use-cases-in-python-where-and-come-in-handy-530dd9d04875&source=-----530dd9d04875---------------------bookmark_footer-----------)![](img/bee7c0a318f34a43becb83b38cfb4d44.png)

照片由 [Szabolcs Toth](https://unsplash.com/@szabolcs_taking_pictures?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/FYt8CIOosOw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

如果你曾经查看过 Python 库的文档，你一定会注意到 * 或 ** 在多个地方的使用。

但，* 和 ** 到底有什么作用？

我们将学习 7 种 * 和 ** 发挥作用的使用场景，包括你在文档中注意到的情况。

最好通过示例来解释这些使用场景，所以我们直接开始吧。

## 1\. 使用可迭代对象调用函数

假设你定义了一个函数，它接受 5 个位置参数，将它们相加并返回总和。

```py
# define the function
def calculate_sum(a, b, c, d, e):
    return a + b + c + d + e

# call the function
result = calculate_sum(1, 4, 3, 2, 6)
print(result)
16
```

如果作为参数传递给函数的值存储在一个可迭代对象（例如列表）中，你可以直接使用列表调用函数，但需要使用*，如下所示：

```py
# call the function
numbers = [1, 4, 3, 2, 6]

calculate_sum(*numbers)
print(result)
16
```

如果你尝试调用函数`calculate_sum(numbers)`，Python 会报错，抛出一个`TypeError`。* 的作用是解包可迭代对象中的值，有时称为…
