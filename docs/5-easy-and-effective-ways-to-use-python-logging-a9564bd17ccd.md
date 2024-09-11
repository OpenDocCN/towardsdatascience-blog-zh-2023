# 5 种简单而有效的 Python 日志使用方法

> 原文：[`towardsdatascience.com/5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd?source=collection_archive---------6-----------------------#2023-06-16`](https://towardsdatascience.com/5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd?source=collection_archive---------6-----------------------#2023-06-16)

## 像专家一样使用 Python 日志

[](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----a9564bd17ccd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------) ·5 分钟阅读·2023 年 6 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa9564bd17ccd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----a9564bd17ccd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa9564bd17ccd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd&source=-----a9564bd17ccd---------------------bookmark_footer-----------)![](img/15b156437e425ed1bab6abb488109fac.png)

图片由作者生成

我敢打赌几乎每个 Python 开发者有时会使用“print”进行调试。对于原型设计来说，这没有什么问题，但对于生产环境来说，有更有效的日志处理方式。在这篇文章中，我将展示 Python “logging” 为什么更灵活、更强大，以及为什么如果你还没有使用它，你绝对应该开始使用它。

让我们开始吧。

## 代码

为了使内容更加实用，我们来考虑一个简单的例子。我创建了一个小应用程序，它计算两个 Python 列表的线性回归：

```py
import numpy as np
from sklearn.linear_model import LinearRegression
from typing import List, Optional

def do_regression(arr_x: List, arr_y: List) -> Optional[List]:
    """ LinearRegression for X and Y lists """
    try:
        x_in = np.array(arr_x).reshape(-1, 1)
        y_in = np.array(arr_y).reshape(-1, 1)
        print(f"X: {x_in}")
        print(f"y: {y_in}")

        reg = LinearRegression().fit(x_in, y_in)
        out = reg.predict(x_in)
        print(f"Out: {out}")
        print(f"Score: {reg.score(x_in, arr_y)}")
        print(f"Coef: {reg.coef_}")
        return out.reshape(-1).tolist()
    except ValueError as err:
        print(f"ValueError…
```
