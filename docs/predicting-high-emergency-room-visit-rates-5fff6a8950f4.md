# 预测高急诊室利用率

> 原文：[https://towardsdatascience.com/predicting-high-emergency-room-visit-rates-5fff6a8950f4?source=collection_archive---------20-----------------------#2023-05-23](https://towardsdatascience.com/predicting-high-emergency-room-visit-rates-5fff6a8950f4?source=collection_archive---------20-----------------------#2023-05-23)

## 使用Python分析社会健康决定因素（SDOH）

[](https://meaganburkhart.medium.com/?source=post_page-----5fff6a8950f4--------------------------------)[![Meagan Burkhart](../Images/d977e25bd5f9bd6e83f92511adaef3f1.png)](https://meaganburkhart.medium.com/?source=post_page-----5fff6a8950f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fff6a8950f4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5fff6a8950f4--------------------------------) [Meagan Burkhart](https://meaganburkhart.medium.com/?source=post_page-----5fff6a8950f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37a56d8d1d6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-high-emergency-room-visit-rates-5fff6a8950f4&user=Meagan+Burkhart&userId=37a56d8d1d6a&source=post_page-37a56d8d1d6a----5fff6a8950f4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fff6a8950f4--------------------------------) ·10 min 阅读·2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5fff6a8950f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-high-emergency-room-visit-rates-5fff6a8950f4&user=Meagan+Burkhart&userId=37a56d8d1d6a&source=-----5fff6a8950f4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fff6a8950f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-high-emergency-room-visit-rates-5fff6a8950f4&source=-----5fff6a8950f4---------------------bookmark_footer-----------)![](../Images/8243a7071de7f2d759a8422c5f9c8dec.png)

由[国家癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)提供的照片，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这个项目的目标是利用[AHRQ](https://www.ahrq.gov/sdoh/data-analytics/sdoh-data.html)提供的按县划分的社会健康决定因素（SDoH）数据，寻找特定变量与县急诊室访问率之间的关系。最终，我希望开发一个与高急诊室访问率相关的顶级特征的预测模型。我决定查看2019年和2020年的数据（2018年的数据不可用）。这个数据集在[HRSA](https://www.hrsa.gov/)的明确许可下使用。

这个逐步教程介绍了我加载、清理、分析和建模数据的过程。

# 加载数据

第一步是加载两个数据文件并检查其形状。

由于两个数据框的列数不同，我将导入数据字典并提取相同的列。

我在列名上合并了数据字典（内连接），以获得最终的公共列列表。一旦我得到了这些列，我就用这些列从每个数据框中选择一个子集，并用`axis=0`进行连接，以垂直方式添加它们。我的df_final包括2019年和2020年在公共列上的数据。

```py
dictionary2019=pd.read_csv('Data/datadictionary2019.csv'…
```
