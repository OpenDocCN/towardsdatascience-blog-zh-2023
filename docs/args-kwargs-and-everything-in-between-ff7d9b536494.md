# *args, **kwargs** 和介于两者之间的一切

> 原文：[`towardsdatascience.com/args-kwargs-and-everything-in-between-ff7d9b536494?source=collection_archive---------12-----------------------#2023-07-18`](https://towardsdatascience.com/args-kwargs-and-everything-in-between-ff7d9b536494?source=collection_archive---------12-----------------------#2023-07-18)

## Python 中函数参数和实参的基础知识

[](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec0e018f30da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-kwargs-and-everything-in-between-ff7d9b536494&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=post_page-ec0e018f30da----ff7d9b536494---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------) ·6 分钟阅读·2023 年 7 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff7d9b536494&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-kwargs-and-everything-in-between-ff7d9b536494&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=-----ff7d9b536494---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff7d9b536494&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-kwargs-and-everything-in-between-ff7d9b536494&source=-----ff7d9b536494---------------------bookmark_footer-----------)![](img/f4942f955c3c59bada72dac594277ddb.png)

图片由 [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 因其多功能性、简洁性和强大的库，已经成为数据科学中的首选语言。函数通过封装可重用代码的能力，在简化和增强 Python 数据科学工作流程中发挥了关键作用。理解函数参数和实参的细微差别，对于在数据科学背景下发挥 Python 函数的真正潜力至关重要。

## 参数 v 论点

在使用 Python 函数时，首先需要了解的是参数与实参的区别。参数是函数定义中的一个变量，而实参是你在调用函数时传入函数参数的内容。例如：

```py
def my_func(param1, param2):
    print(f"{param1} {param2}")

my_func("Arg1", "Arg2")

# Out:
# Arg1 Arg2
```

`param1` 和 `param2` 是函数参数，而 `"Arg1"` 和 `"Arg2"` 是实参。

## 位置参数与关键字参数

在这个例子中，“Arg1”和“Arg2”作为位置参数传入。这是因为每个参数所关联的参数没有在...
