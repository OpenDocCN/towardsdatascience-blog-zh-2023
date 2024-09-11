# 5 个最佳 Python 合成数据生成器及其在数据不足时的使用方法

> 原文：[`towardsdatascience.com/5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c?source=collection_archive---------5-----------------------#2023-01-23`](https://towardsdatascience.com/5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c?source=collection_archive---------5-----------------------#2023-01-23)

## 获取更多数据

[](https://ibexorigin.medium.com/?source=post_page-----f62bcf62d43c--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----f62bcf62d43c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f62bcf62d43c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f62bcf62d43c--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----f62bcf62d43c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----f62bcf62d43c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f62bcf62d43c--------------------------------) · 8 分钟阅读 · 2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff62bcf62d43c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c&user=Bex+T.&userId=39db050c2ac2&source=-----f62bcf62d43c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff62bcf62d43c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c&source=-----f62bcf62d43c---------------------bookmark_footer-----------)![](img/4ec4344f18ad73a5f9dd05b5af784f43.png)

**照片由** [**Maxim Berg**](https://unsplash.com/@maxberg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

2021 年，每天产生了 2.5 亿亿字节（2.5 百万 TB）的数据。今天，这一数字甚至更多。但显然，这还不够，因为 Python 生态系统中有许多库可以生成合成数据。有些库可能只是为了生成合成数据而创建，但大多数有着有益的应用，例如：

+   机器学习：当现实世界的数据不可用或难以获取用于模型训练时

+   数据隐私和安全：用逼真但非真实的数据替换数据集中的敏感信息

+   测试和调试：在受控环境中使用合成数据进行软件测试和调试

+   数据增强：使用机器学习或统计方法从现有数据中人工生成更多数据点

本文将展示六个用于上述目的的 Python 库及其使用方法。

## 使用 Faker 生成随机用户信息

Faker 是最早的 Python 库之一，用于生成各种类型的随机信息。Faker 生成的一些常用属性包括：

+   个人信息：姓名、生日…
