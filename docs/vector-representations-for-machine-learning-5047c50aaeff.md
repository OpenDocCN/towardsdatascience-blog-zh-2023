# 机器学习的向量表示

> 原文：[https://towardsdatascience.com/vector-representations-for-machine-learning-5047c50aaeff?source=collection_archive---------16-----------------------#2023-04-25](https://towardsdatascience.com/vector-representations-for-machine-learning-5047c50aaeff?source=collection_archive---------16-----------------------#2023-04-25)

## *数据科学家如何将现实世界的对象转换为数值表示，以开发机器学习模型*

[](https://medium.com/@theDrewDag?source=post_page-----5047c50aaeff--------------------------------)[![安德烈亚·达戈斯蒂诺](../Images/58c7c218815f25278aae59cea44d8771.png)](https://medium.com/@theDrewDag?source=post_page-----5047c50aaeff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5047c50aaeff--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5047c50aaeff--------------------------------) [安德烈亚·达戈斯蒂诺](https://medium.com/@theDrewDag?source=post_page-----5047c50aaeff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-representations-for-machine-learning-5047c50aaeff&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----5047c50aaeff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5047c50aaeff--------------------------------) ·8分钟阅读·2023年4月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5047c50aaeff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-representations-for-machine-learning-5047c50aaeff&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----5047c50aaeff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5047c50aaeff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-representations-for-machine-learning-5047c50aaeff&source=-----5047c50aaeff---------------------bookmark_footer-----------)![](../Images/f867bba38db6c28d77fe559a6a5983f2.png)

图片由 [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习工程师利用世界的数值表示来构建和训练预测算法。

在监督学习的背景下，这些表示使计算机能够学习它们与目标变量之间的关系。

让我们把向量简单地想象成一个数字列表

```py
X = [1, 2, 3, 4, 5]
```

这个列表与目标变量`y`有关

```py
X = [1, 2, 3, 4, 5]; y = 1
```

机器学习模型学习特征与目标之间的关系，并输出预测——在这种情况下是一个分类，其中一个类别用数字1标识。

在这篇文章中，**我将讨论如何使用向量以数字格式表示复杂概念。**

> 理由在于机器学习模型无法从未以数字格式提供给它的观察数据中学习。

文本、图像、声音及其他输入观察数据必须首先转换为适合的数字格式…
