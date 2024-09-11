# 现代机器学习工程师的 Sklearn 管道：你不能忽视的 9 种技巧

> 原文：[`towardsdatascience.com/sklearn-pipelines-for-the-modern-ml-engineer-9-techniques-you-cant-ignore-637788f05df5?source=collection_archive---------1-----------------------#2023-05-29`](https://towardsdatascience.com/sklearn-pipelines-for-the-modern-ml-engineer-9-techniques-you-cant-ignore-637788f05df5?source=collection_archive---------1-----------------------#2023-05-29)

## 有很多方法可以构建它们……

[](https://ibexorigin.medium.com/?source=post_page-----637788f05df5--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----637788f05df5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----637788f05df5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----637788f05df5--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----637788f05df5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-pipelines-for-the-modern-ml-engineer-9-techniques-you-cant-ignore-637788f05df5&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----637788f05df5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----637788f05df5--------------------------------) · 10 分钟阅读 · 2023 年 5 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F637788f05df5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-pipelines-for-the-modern-ml-engineer-9-techniques-you-cant-ignore-637788f05df5&user=Bex+T.&userId=39db050c2ac2&source=-----637788f05df5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F637788f05df5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-pipelines-for-the-modern-ml-engineer-9-techniques-you-cant-ignore-637788f05df5&source=-----637788f05df5---------------------bookmark_footer-----------)![](img/d48b62f54a1b0bf20e303b7d4db6b1d8.png)

**图片由我使用 Midjourney 制作**

## 动机

今天，我出售的是：

```py
awesome_pipeline.fit(X, y)
```

`awesome_pipeline` 可能看起来只是另一个变量，但它在背后对 `X` 和 `y` 进行的处理如下：

1.  自动隔离 `X` 中的数值特征和类别特征。

1.  对数值特征中的缺失值进行填充。

1.  对偏斜特征进行对数变换，同时归一化其余特征。

1.  对类别特征中的缺失值进行填充，并进行独热编码。

1.  对目标数组`y`进行归一化处理以确保其准确性。

除了将近 100 行难以阅读的代码压缩成一行之外，`awesome_pipeline` 现在还可以被插入到交叉验证器或超参数调优器中，保护你的代码不受数据泄漏的影响，并使一切变得可重复、模块化且无忧无虑。

让我们来看看如何构建这个东西。

## 0\. 估算器 vs 转换器

首先，我们来解决术语问题。
