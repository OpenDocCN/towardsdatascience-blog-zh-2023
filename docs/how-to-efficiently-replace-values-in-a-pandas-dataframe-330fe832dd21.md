# 如何高效替换 Pandas DataFrame 中的值

> 原文：[https://towardsdatascience.com/how-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21?source=collection_archive---------9-----------------------#2023-07-12](https://towardsdatascience.com/how-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21?source=collection_archive---------9-----------------------#2023-07-12)

## PYTHON

## 对 Pandas 替换方法的详细讲解，以及如何通过几个简单的示例使用它

[](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)[![Byron Dolon](../Images/9ff32138c7b1913be24cc7ab971752b0.png)](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----330fe832dd21---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------) ·8 分钟阅读·2023年7月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F330fe832dd21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21&user=Byron+Dolon&userId=6b5d063df5dd&source=-----330fe832dd21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F330fe832dd21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21&source=-----330fe832dd21---------------------bookmark_footer-----------)![](../Images/53c719fb6e620fc207e6e6e861e91571.png)

图片使用了我才华横溢的妹妹 [ohmintyartz](https://www.instagram.com/ohmintyartz/) 的许可

Pandas 库提供了各种内置方法，可以用来处理和清理数据，使其准备好进行分析和机器学习。

当你处理不同种类的数据时，你通常会发现需要根据条件删除整行数据或更新部分字符串值作为数据清理的一部分。你可能还会想要在特征工程过程中从现有列中创建新列。

Pandas 允许你对对象和字符串数据类型执行各种操作，通过其原生的转换方法。在这篇文章中，我们将特别关注如何在你的 DataFrame 列中替换整个值和/或子字符串。

随意在笔记本中跟随本篇中的示例进行操作吧！你可以从 Kaggle 下载 [数据集](https://www.kaggle.com/datasets/drahulsingh/top-largest-universities/versions/1?resource=download)，该数据集在 Open Data Commons Public Domain Dedication and License (PDDL) v1.0 下可免费使用。然后导入并运行以下内容，我们就可以开始了！

```py
import pandas as pd
df_raw = pd.read_csv("Top-Largest-Universities.csv")
```
