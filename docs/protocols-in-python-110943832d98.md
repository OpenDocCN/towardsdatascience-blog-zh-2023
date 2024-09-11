# Python 中的协议

> 原文：[`towardsdatascience.com/protocols-in-python-110943832d98?source=collection_archive---------0-----------------------#2023-07-27`](https://towardsdatascience.com/protocols-in-python-110943832d98?source=collection_archive---------0-----------------------#2023-07-27)

## 如何使用结构性子类型

[](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprotocols-in-python-110943832d98&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----110943832d98---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------) ·5 分钟阅读·2023 年 7 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F110943832d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprotocols-in-python-110943832d98&user=Oliver+S&userId=f2daf6260cca&source=-----110943832d98---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F110943832d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprotocols-in-python-110943832d98&source=-----110943832d98---------------------bookmark_footer-----------)

Python 3.8 引入了一个新特性： [协议](https://peps.python.org/pep-0544/)。协议是[抽象基类 (ABC)](https://docs.python.org/3/library/abc.html)的替代方案，允许结构性子类型——仅根据可用的属性和函数检查两个类是否兼容。在这篇文章中，我们将深入探讨这一点，并通过实际示例展示如何使用协议。

![](img/90dd020181af078f64485fe5957a0b41.png)

图片由[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/9cd8qOgeNIY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# Python 中的类型

让我们首先讨论一下 Python 的类型系统。它是一种动态类型语言，意味着类型是在运行时推断的，下面的代码运行没有问题：

```py
def add(x, y):
    return x + y

print(add(2, 3))
print(add("str1", "str2"))
```

第一次调用会返回整数相加的结果 5，第二次调用会返回字符串连接的结果“str1str2”。这与例如 C++ 这样的静态类型语言不同——我们必须提供类型声明：

```py
int add(int x, int y) {
    return x + y;
}

std::string add(std::string x, std::string y) {
    return x + y;
}

int main()
{
    std::cout<<add(2, 3);
    std::cout << add("str1", "str2");
    return 0;
}
```

静态类型提供了在编译时捕捉错误的潜力这一优势——而在动态类型语言中，我们只能在运行时遇到这些错误……
