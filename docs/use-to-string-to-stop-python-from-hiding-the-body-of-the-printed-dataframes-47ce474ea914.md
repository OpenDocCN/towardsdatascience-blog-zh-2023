# 使用to_string()来防止Python隐藏打印的DataFrame的内容。

> 原文：[https://towardsdatascience.com/use-to-string-to-stop-python-from-hiding-the-body-of-the-printed-dataframes-47ce474ea914?source=collection_archive---------15-----------------------#2023-04-10](https://towardsdatascience.com/use-to-string-to-stop-python-from-hiding-the-body-of-the-printed-dataframes-47ce474ea914?source=collection_archive---------15-----------------------#2023-04-10)

## 3分钟Pandas

## 我们应该怎么做才能在Python脚本执行后看到整个打印的DataFrame？

[](https://jianan-lin.medium.com/?source=post_page-----47ce474ea914--------------------------------)[![Yufeng](../Images/8b1a4c165aaac045ea819f850017b7cd.png)](https://jianan-lin.medium.com/?source=post_page-----47ce474ea914--------------------------------)[](https://towardsdatascience.com/?source=post_page-----47ce474ea914--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----47ce474ea914--------------------------------) [Yufeng](https://jianan-lin.medium.com/?source=post_page-----47ce474ea914--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9cda0369fb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-to-string-to-stop-python-from-hiding-the-body-of-the-printed-dataframes-47ce474ea914&user=Yufeng&userId=9cda0369fb2&source=post_page-9cda0369fb2----47ce474ea914---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----47ce474ea914--------------------------------) ·4分钟阅读·2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F47ce474ea914&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-to-string-to-stop-python-from-hiding-the-body-of-the-printed-dataframes-47ce474ea914&user=Yufeng&userId=9cda0369fb2&source=-----47ce474ea914---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F47ce474ea914&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-to-string-to-stop-python-from-hiding-the-body-of-the-printed-dataframes-47ce474ea914&source=-----47ce474ea914---------------------bookmark_footer-----------)![](../Images/82ece6c195a037aa5d625ace0831eddf.png)

照片由[Pascal Müller](https://unsplash.com/@millerthachiller?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

有时通过 Python 脚本运行而不报告任何错误并不是调试过程的唯一任务。我们需要确保函数按预期执行。这是探索性数据分析中的一个典型步骤，用于检查在某些特定数据处理之前和之后数据的情况。

所以，我们需要在脚本执行过程中打印一些数据框或关键变量，以检查它们是否“正确”。然而，简单的打印命令有时只能显示数据框的顶部和底部行（如下例所示），这使得检查过程不必要地困难。

通常，数据框的格式是`pandas.DataFrame`，如果直接使用打印命令，你可能会得到如下内容，

```py
import pandas as pd
import numpy as np

data = np.random.randn(5000, 5)
df = pd.DataFrame(data, columns=['A', 'B', 'C', 'D', 'E'])

print(df.head(100))
```
