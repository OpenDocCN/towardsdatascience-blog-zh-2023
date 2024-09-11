# 《Python 枚举如何使数据配置优雅》

> 原文：[`towardsdatascience.com/how-python-enumerations-make-data-configuration-elegant-a7d8356657bd?source=collection_archive---------4-----------------------#2023-05-28`](https://towardsdatascience.com/how-python-enumerations-make-data-configuration-elegant-a7d8356657bd?source=collection_archive---------4-----------------------#2023-05-28)

## Python 中用于机器学习项目的枚举介绍

[](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7722f981eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-python-enumerations-make-data-configuration-elegant-a7d8356657bd&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=post_page-7722f981eb----a7d8356657bd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------) · 7 min read · 2023 年 5 月 28 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa7d8356657bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-python-enumerations-make-data-configuration-elegant-a7d8356657bd&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=-----a7d8356657bd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7d8356657bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-python-enumerations-make-data-configuration-elegant-a7d8356657bd&source=-----a7d8356657bd---------------------bookmark_footer-----------)![](img/f50c10b5f705134f0d6b76ef93de6254.png)

图片来源：[Joshua Woroniecki](https://unsplash.com/@joshua_j_woroniecki?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 你的旅程概览

1.  介绍

1.  一个初步的方法

1.  枚举的救援！

1.  总结

# 1 — 介绍

在机器学习项目中编写 Python 代码时，几乎总是需要**配置代码**。这段代码跟踪用于配置代码其他组件的信息。虽然这个定义可能对那些不理解它是什么意思的人帮助不大，但几个例子确实很有帮助：

+   **超参数：** 在机器学习中，超参数用于训练多个略有不同的模型。超参数可能是随机森林中的树木数量。另一个例子是 Ridge 回归或 Lasso 回归的松弛参数。

+   **访问信息：** 当连接到数据库或云托管的存储账户时，需要访问信息。这包括存储系统的名称、密码和其他附加属性。类似地，比如你正在订阅一个发布/订阅系统，如 Kafka 或 MQTT 代理。那么主题的名称、密码和端口号……
