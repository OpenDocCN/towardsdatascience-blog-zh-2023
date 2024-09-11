# **Pd.Get_Dummies**的优点、缺点和不足

> 原文：[`towardsdatascience.com/the-good-the-bad-and-the-ugly-of-pd-get-dummies-75c87e2aadc9?source=collection_archive---------11-----------------------#2023-07-26`](https://towardsdatascience.com/the-good-the-bad-and-the-ugly-of-pd-get-dummies-75c87e2aadc9?source=collection_archive---------11-----------------------#2023-07-26)

## 这是为那些**pd.get_dummies**的忠实粉丝准备的

[](https://adamrossnelson.medium.com/?source=post_page-----75c87e2aadc9--------------------------------)![Adam Ross Nelson](https://adamrossnelson.medium.com/?source=post_page-----75c87e2aadc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----75c87e2aadc9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----75c87e2aadc9--------------------------------) [Adam Ross Nelson](https://adamrossnelson.medium.com/?source=post_page-----75c87e2aadc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4c558804d1cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-good-the-bad-and-the-ugly-of-pd-get-dummies-75c87e2aadc9&user=Adam+Ross+Nelson&userId=4c558804d1cf&source=post_page-4c558804d1cf----75c87e2aadc9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----75c87e2aadc9--------------------------------) ·5 分钟阅读·2023 年 7 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F75c87e2aadc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-good-the-bad-and-the-ugly-of-pd-get-dummies-75c87e2aadc9&user=Adam+Ross+Nelson&userId=4c558804d1cf&source=-----75c87e2aadc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F75c87e2aadc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-good-the-bad-and-the-ugly-of-pd-get-dummies-75c87e2aadc9&source=-----75c87e2aadc9---------------------bookmark_footer-----------)

大家好 🤠

好吧，我明白了。在 Python 中，将分类变量转换为虚拟变量数组的最简单方法之一是使用 Pandas `pd.get_dummies()`。为什么要花时间导入 `OneHotEncoder` 从 sklearn，执行 `.fit_transform()` 等等呢？这真是繁琐！

本文将首先介绍一个简单的数据集，用于演示包含训练集中未出现的分类数据的测试集。然后，将演示如何使用 `pd.get_dummies()` 可能会导致演示数据出现问题。最后，展示如何使用 sklearn 的 `OneHotEncoder` 避免这个问题。

![](img/fa8e91379f5a2edc4d962a235f228a29.png)

图片来源：作者在 Canva 中使用文本到图像的插图。提示：“三只穿着乡村西部牛仔服装的熊猫。”

# 一个用于演示的简单数据集

在这里，我们有一个简单的数据集，其中包含一个名为 OS 的分类特征。OS 列列出了计算机操作系统。我们将使用这些虚构的数据进行演示。在`train_df`中将是虚构的演示训练数据。而在`test_df`中，我们有虚构的演示测试数据。

在我们虚构的演示案例中，测试集包含训练集中不存在的类别值。这种不匹配会导致问题。

```py
import pandas as pd…
```
