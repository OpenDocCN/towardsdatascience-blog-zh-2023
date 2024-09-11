# 在 PySpark 中使用 PCA 排名钻石

> 原文：[https://towardsdatascience.com/ranking-diamonds-with-pca-in-pyspark-a59cab7f4f1a?source=collection_archive---------11-----------------------#2023-12-22](https://towardsdatascience.com/ranking-diamonds-with-pca-in-pyspark-a59cab7f4f1a?source=collection_archive---------11-----------------------#2023-12-22)

## 在 PySpark 中运行主成分分析的挑战

[](https://gustavorsantos.medium.com/?source=post_page-----a59cab7f4f1a--------------------------------)[![Gustavo Santos](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----a59cab7f4f1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a59cab7f4f1a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a59cab7f4f1a--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----a59cab7f4f1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Franking-diamonds-with-pca-in-pyspark-a59cab7f4f1a&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----a59cab7f4f1a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a59cab7f4f1a--------------------------------) ·8 min read·2023年12月22日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa59cab7f4f1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Franking-diamonds-with-pca-in-pyspark-a59cab7f4f1a&source=-----a59cab7f4f1a---------------------bookmark_footer-----------)![](../Images/829083190a338cf53e34959cadcae252.png)

图片由 [Edgar Soto](https://unsplash.com/@edgardo1987?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/two-diamond-studded-silver-rings-gb0BZGae1Nk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

这是关于 PySpark 的另一篇文章。我很享受写关于这个主题的内容，因为我感觉我们缺乏关于 PySpark 的优质博客文章，特别是当我们谈论 MLlib 中的机器学习时——顺便提一下，这是 Spark 的**M**achine **L**earning **Lib**rary，旨在与大数据在并行环境中一起工作。

我可以说，Spark 文档非常优秀。它组织得非常有条理，易于跟随示例。不过，在 Spark 中进行机器学习并不是最友好的事情。

在这篇文章中，我使用 PCA 模型来帮助我创建钻石的排名，并且遇到了一些挑战，我们将在接下来的内容中讨论这些挑战。

我已经[之前写过关于 PCA 的内容](https://medium.com/towards-data-science/pca-beyond-the-dimensionality-reduction-e352eb0bdf52)，以及它在降维方面的实用性，也有[关于创建排名的内容](https://medium.com/towards-data-science/creating-scores-and-rankings-with-pca-c2c3081fdb26)。然而，这还是我第一次使用 Spark 来进行这项工作，目的是在大数据环境中重现该技术。

让我们来看一下结果。

# 编码

让我们从需要导入的模块开始我们的编码。

```py
from pyspark.sql.functions import col, sum, when, mean…
```
