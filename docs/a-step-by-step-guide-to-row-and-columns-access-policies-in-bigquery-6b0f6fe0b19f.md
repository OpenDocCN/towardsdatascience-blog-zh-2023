# BigQuery 中的行和列访问策略逐步指南

> 原文：[`towardsdatascience.com/a-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f`](https://towardsdatascience.com/a-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f)

## Google 的数据仓库提供了令人惊叹的方式来限制对表格和视图中信息的访问，无论是在垂直还是水平维度上，让我们来探索如何设置这些限制。

[](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)![Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------) [Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------) ·7 分钟阅读·2023 年 1 月 19 日

--

![](img/726d73195957ee54f18bfda9b8bf529a.png)

图片来自 [Martin Olsen](https://unsplash.com/@martinols3n) 在 [Unsplash](https://unsplash.com/photos/szfPB4X-FWk)

管理数据仓库意味着多个用户，具有不同的角色和权限，应该能够查询他们所需的数据。

你可能会决定为每个角色构建一个特定的表格，但这将会消耗时间和资源，更不用说随着时间推移维护这个系统的困难了。

在本教程中，我们将创建一个“EMPLOYEES”表，包含各种级别的信息，从员工所在的“国家”... 到他/她的工资... 并考虑 3 个角色：

![](img/03aacdca9a85d7f58da7ffedc6af0a45.png)

3 个角色的示意图 — 作者通过 [www.freepik.com](http://www.freepik.com) 创建的图像

+   **人力资源**：他们应该能够访问所有数据，以进行日常工作，没有任何限制。

+   **数据科学家**：他们可能只会访问一些为建模目的而选择的和匿名化的列。

+   **数据公民**：大多数列应该对他们隐藏，只允许创建高层次的查询和分析。

# 在 Google Cloud Platform 中创建一个新项目

我将假设你已经有了一个 GCP 帐户。如果没有，你可以轻松注册一个，并获得 30 天的 $300 额度。如果你已经有了，这个示例几乎不会花费你任何费用。

你应该从创建一个新项目开始，以隔离你的工作。

点击左上角的项目选择器，然后选择“新建项目”。

**确保你可以在组织（如你的公司）下创建项目，否则你将无法执行一些策略**。

![](img/ab13ed827771aeadf5d54d5f0a57bc37.png)![](img/c7be03ab945951b569bc6e431efd9d1f.png)

在 GCP 中创建一个与组织关联的新项目 — 图片来源于作者

*注意：这里创建的项目是“****row-columns-policies****”。*

# 创建一个虚假的员工信息表

![](img/a05d542ccdcc3acb4be036508c26b22d.png)

“salaries.csv” 内容显示在 BigQuery 中 — 图片来源于作者

我已经创建并上传了一个虚假的表格，您可以在我的 [GoogleDrive](https://drive.google.com/file/d/1YHKOPR92SmiO1ujm3qDKRDLcpxkq_Npg/view?usp=share_link) 上查看，其中包含每 8600 名员工的信息：

+   **名**

+   **姓**

+   **部门**

+   **国家**

+   **电子邮件**

+   **薪资**

+   **货币**

+   **薪资定位**

+   **最后绩效评估**

+   **资历**

+   **通勤时间**

让我们跳转到“BigQuery”，点击“**添加数据**”和“**本地存储**”，并牢记两个重要点：

1.  表必须存储在数据集中：我们将创建一个名为“EMPLOYEES”的数据集（见第二张截图）。**您应注意选择的区域：定义策略时必须使用相同的区域。**

1.  由于 CSV 文件的结构简单，我们可以选择“自动检测”方案选项，这样就无需逐一声明每列的类型。

![](img/1c7c11e6722a9e96e8111479d7a60d25.png)

在 BigQuery 中创建新表 — 图片来源于作者

![](img/51fa8306e2f242344a2c96bbb374046b.png)

在 BigQuery 中创建新数据集 — 图片来源于作者

我们的“薪资”表现在已正确构建（见第一个截图），简单的 SQL 查询（见第二个截图）将揭示相应的数据：

![](img/0846e1b8b4b7a064a9edf445ca047501.png)

BigQuery 中的 EMPLOYEES 表结构 — 图片来源于作者

```py
SELECT *
FROM `row-columns-policies.EMPLOYEES.SALARIES`
ORDER BY Country, Last_Name, First_Name
LIMIT 30
```

![](img/5076c50d9efe6ce01061a68a9b7a654b.png)

SQL 探索结果在 BigQuery 中 — 图片来源于作者

# 创建一个“标签政策”分类以限制访问

从 BigQuery 菜单中，我们切换到“**策略标签**”（如果被要求，启用“**Google Cloud Data Catalog**”和/或“**BigQuery 数据政策**”API）：

![](img/1572494b4a8f5048976a360c69a1c5a3.png)

点击“**创建分类**”后，我们定义了三个级别的员工信息：

+   **高**（关键数据如薪资）

+   **中等**（如“最后绩效”或“薪资定位”）对于数据科学家在建模任务中可能会有帮助。

+   **识别**（与员工直接相关的信息）

    + 两个子级：

+   **识别 > 姓名**

+   **识别 > 电子邮件**

*注意：下面的两个子标签（姓名和电子邮件）将允许我们以后使用不同的掩码策略。*

![](img/02032dfbf21992d80501dc13aaf4f975.png)

在 BigQuery/Dataplex 中创建新分类 — 图片来源于作者

*注意：如前所述，请确保使用与数据集相同的区域。*

一旦分类法创建完成，**最关键的部分是启用它**，如果你的项目不与组织相关，你可能会遇到问题（见 [Google 相关文档](https://cloud.google.com/resource-manager/docs/creating-managing-organization)）。

![](img/1416226c36660e65d02a52c1d5ddad5e.png)

在 BigQuery/Dataplex 中强制执行分类法 — 图片来自作者

# 将标签策略应用于表列

标签策略可以通过 GCP 的“Dataplex”部分应用。搜索引擎将帮助你快速识别“SALARIES”表：

![](img/5c6d78abdbf4be5fe5f7da38f9afbb83.png)

在 Dataplex 中搜索表 — 图片来自作者

通过“SCHEMA AND COLUMN TAGS”，我们可以轻松地将策略标签分配给一些敏感列：

![](img/7814f52c2bb4f8ed8d92dfd524a3c65d.png)

在 Dataplex 中应用策略标签 — 图片来自作者

# 向项目中添加一个“查看者”主体

![](img/439de62570b783adee18b171ff03fd86.png)

3 个角色的示意图 — 作者通过 [www.freepik.com](http://www.freepik.com) 创建的图片

假设一个数据公民想要访问这个项目，并进行关于员工在各个国家分布的高层次分析。

我们返回到项目的 IAM 部分，添加一个“查看者”角色的主体：

![](img/6cf4433a38c081d579c5e027b5fa6c33.png)

在 Google IAM 中分配“查看者”角色 — 图片来自作者

一旦连接，该主体将立即收到来自 BigQuery 的警告，告知他/她某些列的访问将受到限制：

![](img/28bb02c4f207718b8f04ea88a7fd8169.png)

使用 BigQuery 中的“查看者”角色查看 SALARIES 表 — 图片来自作者

实际上，由于我们拥有的权限最少，预览部分仅显示 3 列：

![](img/4fff750869a78a6e211415c745d47307.png)

使用 BigQuery 中的“查看者”角色预览 SALARIES 表 — 图片来自作者

但它仍允许我们进行我们想要的分析，并按国家和部门获取员工数量的详细信息：

![](img/364645f2cc26126b0f38753c220fe94f.png)

使用 BigQuery 中的“查看者”角色对 SALARIES 表进行 SQL 查询 — 图片来自作者

# 定义“掩码阅读者”角色及其对应的掩码策略

![](img/e4a9b7ef1c80fca88d6fcf1d97a103ef.png)

3 个角色的示意图 — 作者通过 [www.freepik.com](http://www.freepik.com) 创建的图片

现在假设人力资源部门要求数据科学家对员工进行一些统计分析。

她需要访问一些列，但不一定以其原始形式访问。

我们首先将“**掩码阅读者**”角色分配给这个新主体：

![](img/7ef308d69ab58af88a242b17ed38ad28.png)

在 Google IAM 中分配“掩码阅读者”角色 — 图片来自作者

然后定义不同的掩码策略，根据我们的需要转换每一列。例如：

+   **名字**和**姓氏**应转换为**NULL**

+   **电子邮件** 应该被 “**哈希化**”，以保持唯一标识符，但无法与原始员工关联

+   **薪资** 应该转换为 “**0**” ([整数的默认掩码策略](https://cloud.google.com/bigquery/docs/column-data-masking-intro#masking_options))

![](img/f3fc4e3233d49ecb11f28930a75f76e1.png)

在 Google BigQuery 中定义掩码规则的步骤 — 图片来自作者

![](img/d515649596a93a98a99f6cd1806002a2.png)

在 Google BigQuery 中的一组掩码规则 — 图片来自作者

最后，我们需要授予对 “Medium” 标签信息的访问权限，并将这个新主体添加为该类别的授权查看者。

![](img/caa3b05bb67a88f395f25caec7e696f5.png)

在 Google BigQuery 中将一个主体添加为掩码策略的查看者 — 图片来自作者

正如预期的那样，下面的 SQL 查询结果 — 当以“掩码读取器”角色连接时 — 将正确实现定义的规则：

```py
SELECT * EXCEPT(First_Name, Last_Name)
FROM `row-columns-policies.EMPLOYEES.SALARIES`
```

![](img/e675552af026653f55b63112b93214cd.png)

在 Google BigQuery 中，掩码策略对掩码读取器角色的影响 — 图片来自作者

# 将行级策略添加到表访问管理中

现在假设，作为全球人力资源经理，我们需要向本地加拿大人力资源授予对该表的访问权限……同时不披露其他国家的数字。

我们可以为该用户设置行级安全性：

```py
|CREATE ROW ACCESS POLICY Canadian_filter
ON `row-columns-policies.EMPLOYEES.SALARIES`
GRANT TO ('user:user@domain.com')
FILTER USING (Country = 'Canada');
```

这就像自动将任何由这个主体执行的查询扩展为 “ WHERE Country = ‘Canada’ ” 语句：

![](img/747d57586f8af2aa2d2c7971b379f8fb.png)

行级安全性在 Google BigQuery 中的结果 — 图片来自作者

这是减少一个或多个维度上可用信息的非常好方法！

这个最后的例子结束了我想在本文中探索的不同用例。

作为提醒，“列掩码”和“行级”策略提供了一种直接在数据平台中过滤数据的好方法。数据仓库中仅管理一个表，允许不同的解决方案和/或用户无缝查询，而不影响保密规则。

🍒 画龙点睛的是，这些功能是免费的，所以完全没有理由不利用它们！

像往常一样，我尝试确定所有必需的步骤，但如果我的教程中有任何遗漏的指令，请随时联系我！

（感谢我们的首席数据工程师 Ilyes Touzene 的校对！）

并且不要犹豫，浏览我在 Medium 上的其他贡献：

[](https://pl-bescond.medium.com/pierre-louis-besconds-articles-on-medium-f6632a6895ad?source=post_page-----6b0f6fe0b19f--------------------------------) [## Pierre-Louis Bescond 在 Medium 上的文章

### 数据科学、机器学习和创新

[pl-bescond.medium.com](https://pl-bescond.medium.com/pierre-louis-besconds-articles-on-medium-f6632a6895ad?source=post_page-----6b0f6fe0b19f--------------------------------)
