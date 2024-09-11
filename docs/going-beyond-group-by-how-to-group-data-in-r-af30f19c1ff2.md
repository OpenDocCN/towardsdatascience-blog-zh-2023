# 如何在 R 中分组数据：超越“group_by”

> 原文：[https://towardsdatascience.com/going-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2?source=collection_archive---------12-----------------------#2023-02-15](https://towardsdatascience.com/going-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2?source=collection_archive---------12-----------------------#2023-02-15)

## 从初学者到高级用户，掌握这些分组工作流程

[](https://roryspanton.medium.com/?source=post_page-----af30f19c1ff2--------------------------------)[![Rory Spanton](../Images/6c35a3de7cb516aac09bc5cf417a6c70.png)](https://roryspanton.medium.com/?source=post_page-----af30f19c1ff2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af30f19c1ff2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----af30f19c1ff2--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----af30f19c1ff2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39501aa8ce39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2&user=Rory+Spanton&userId=39501aa8ce39&source=post_page-39501aa8ce39----af30f19c1ff2---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----af30f19c1ff2--------------------------------) ·8分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf30f19c1ff2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2&user=Rory+Spanton&userId=39501aa8ce39&source=-----af30f19c1ff2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf30f19c1ff2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-beyond-group-by-how-to-group-data-in-r-af30f19c1ff2&source=-----af30f19c1ff2---------------------bookmark_footer-----------)![](../Images/94f77c2b664024472c8b2639518e76a7.png)

图片由[Camille San Vicente](https://unsplash.com/@camillesanvicente?utm_source=medium&utm_medium=referral)提供，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

分组数据允许你对数据集的子集而非整个数据集执行操作。处理分组数据是数据分析的关键方面，在数据科学中有着近乎无限的应用。

在 R 中有很多方法来创建和操作分组。在本文中，我将解释 dplyr 包的分组工作流程，从基础到更高级的功能。

最后，你应该掌握从分组数据中提取有价值的见解所需的所有工具。本文中的所有代码也可以在 [GitHub](https://github.com/roryspanton/medium-code/blob/master/grouping/grouping.R) 上找到。

# 在 dplyr 中的基本分组

要在 dplyr 中对数据进行分组，你主要会使用 `group_by` 函数。你可以用这个函数来指定一个或多个变量来对数据进行分组。下面是使用 palmerpenguins 包中的 penguins 数据集的一个例子。你可以通过运行 `install.packages(“palmerpenguins”)` 安装这个包。一旦用 `library(palmerpenguins)` 加载了这个包，你就可以通过名称访问 `penguins` 数据集，如下所示。

```py
library(tidyverse)
library(palmerpenguins)

# The Palmer Penguins dataset
penguins
```
