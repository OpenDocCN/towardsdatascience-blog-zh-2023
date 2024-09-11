# PySpark中的逻辑回归简介

> 原文：[https://towardsdatascience.com/introduction-to-logistic-regression-in-pyspark-9f894299c32d?source=collection_archive---------9-----------------------#2023-11-04](https://towardsdatascience.com/introduction-to-logistic-regression-in-pyspark-9f894299c32d?source=collection_archive---------9-----------------------#2023-11-04)

## 在Databricks中运行你的第一个分类模型教程

[](https://gustavorsantos.medium.com/?source=post_page-----9f894299c32d--------------------------------)[![Gustavo Santos](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----9f894299c32d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f894299c32d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f894299c32d--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----9f894299c32d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-logistic-regression-in-pyspark-9f894299c32d&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----9f894299c32d---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----9f894299c32d--------------------------------) ·10分钟阅读·2023年11月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f894299c32d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-logistic-regression-in-pyspark-9f894299c32d&user=Gustavo+Santos&userId=4429d99b1245&source=-----9f894299c32d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f894299c32d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-logistic-regression-in-pyspark-9f894299c32d&source=-----9f894299c32d---------------------bookmark_footer-----------)![](../Images/2728b2918920168e3a496a57722adcf0.png)

照片由 [Ibrahim Rifath](https://unsplash.com/@ripeyyy_?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 拍摄，发布在 [Unsplash](https://unsplash.com/photos/stacked-round-gold-colored-coins-on-white-surface-OApHds2yEGQ?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 简介

大数据。大规模数据集。云计算…

这些词语无处不在，跟随在我们周围，并存在于客户、面试官、经理和董事的思维中。随着数据变得越来越丰富，数据集的规模也在不断扩大，以至于有时在本地环境——换句话说，在单台机器上——运行机器学习模型变得不可行。

这个问题要求我们适应并寻找其他解决方案，比如使用Spark建模，它是大数据最常用的技术之一。Spark接受SQL、Python、Scala、R等语言，并具有自己的方法和属性，包括自己的机器学习库[MLlib]。例如，当你在Spark中使用Python时，它被称为*PySpark*。

此外，还有一个名为*Databricks*的平台，它将Spark包装在一个非常出色的层中，使数据科学家可以像使用Anaconda一样使用它。

当我们在Databricks中创建ML模型时，它也支持Scikit Learn模型，但由于我们更关注大数据，本教程全部使用**Spark的MLlib**，这更适合大规模数据集，并且通过这种方式我们添加了一个新的……
