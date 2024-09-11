# Python 数据工程师

> 原文：[`towardsdatascience.com/python-for-data-engineers-f3d5db59b6dd?source=collection_archive---------0-----------------------#2023-10-21`](https://towardsdatascience.com/python-for-data-engineers-f3d5db59b6dd?source=collection_archive---------0-----------------------#2023-10-21)

## 针对初学者的高级 ETL 技术

[](https://mshakhomirov.medium.com/?source=post_page-----f3d5db59b6dd--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----f3d5db59b6dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f3d5db59b6dd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f3d5db59b6dd--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----f3d5db59b6dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-for-data-engineers-f3d5db59b6dd&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----f3d5db59b6dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f3d5db59b6dd--------------------------------) ·17 min read·2023 年 10 月 21 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff3d5db59b6dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-for-data-engineers-f3d5db59b6dd&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----f3d5db59b6dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff3d5db59b6dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-for-data-engineers-f3d5db59b6dd&source=-----f3d5db59b6dd---------------------bookmark_footer-----------)![](img/9c664876939298ade749c4c53cb490c8.png)

图片由 [Boitumelo](https://unsplash.com/@writecodenow?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个故事中，我将讨论 Python 中的高级数据工程技术。毫无疑问，Python 是处理数据的最受欢迎的编程语言。在我近十二年的数据工程职业生涯中，我遇到了各种代码问题。这篇故事简要总结了我如何解决这些问题以及学会编写更好代码的过程。我将展示一些技术，这些技术可以让我们的 ETL 更快，并帮助提高代码的性能。

## 列表推导式

想象一下你在遍历一个表的列表。通常，我们会这样做：

```py
data_pipelines = ['p1','p2','p3']
processed_tables = []
for table in data_pipelines:
    processed_tables.append(table)
```

但我们可以使用列表推导式。它们不仅更快，而且还减少了代码，使其更简洁：

```py
processed_tables = [table for table in data_pipelines]
```

例如，遍历一个超大的文件来转换（ETL）每一行数据变得前所未有的简单：

```py
def etl(item):
    # Do some data transformation here
    return json.dumps(item)

data = u"\n".join(etl(item) for item in json_data)
```
