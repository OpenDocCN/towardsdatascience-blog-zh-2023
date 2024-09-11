# 在 Julia 中使用 MS SQL Server

> 原文：[https://towardsdatascience.com/working-with-ms-sql-server-in-julia-fc6fcd55d69a?source=collection_archive---------11-----------------------#2023-07-17](https://towardsdatascience.com/working-with-ms-sql-server-in-julia-fc6fcd55d69a?source=collection_archive---------11-----------------------#2023-07-17)

## 是时候提升你的数据分析工作流程了

[](https://medium.com/@vikas.negi10?source=post_page-----fc6fcd55d69a--------------------------------)[![Vikas Negi](../Images/3f5974d44cfdbdecb77e3b4cb3098af0.png)](https://medium.com/@vikas.negi10?source=post_page-----fc6fcd55d69a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc6fcd55d69a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc6fcd55d69a--------------------------------) [Vikas Negi](https://medium.com/@vikas.negi10?source=post_page-----fc6fcd55d69a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbad3267739cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-ms-sql-server-in-julia-fc6fcd55d69a&user=Vikas+Negi&userId=bad3267739cc&source=post_page-bad3267739cc----fc6fcd55d69a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc6fcd55d69a--------------------------------) ·6 分钟阅读·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc6fcd55d69a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-ms-sql-server-in-julia-fc6fcd55d69a&user=Vikas+Negi&userId=bad3267739cc&source=-----fc6fcd55d69a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc6fcd55d69a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-ms-sql-server-in-julia-fc6fcd55d69a&source=-----fc6fcd55d69a---------------------bookmark_footer-----------)![](../Images/9f7f39a686701078d2e8dfa7ef35f988.png)

照片由 [Venti Views](https://unsplash.com/@ventiviews?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SQL 数据库是全球最广泛部署的软件之一。它们构成了众多应用程序的核心，从业务数据分析到天气预报等。当前存在多种客户端-服务器实现，[微软的 SQL Server](https://cloudblogs.microsoft.com/sqlserver/2022/11/16/sql-server-2022-is-now-generally-available/) 就是其中之一。功能齐全的 [开发者版](https://www.microsoft.com/en-in/sql-server/sql-server-downloads) 可以免费获取。它可以在 Windows、Linux 和 Docker 上运行。

数据科学家常常需要与存储在SQL数据库中的数据进行交互。虽然很容易找到关于如何使用Python进行此操作的指南，但Julia的教程则非常稀少。因此，在本文中，我将专注于如何使用Julia与SQL Server进行工作。示例代码是使用[Pluto notebook](https://github.com/vnegi10/MS_SQL_analysis)生成的，Julia 1.9.1在Linux（Elementary OS）上运行。

# 前提条件

1.  SQL Server 2022

你需要在本地运行SQL服务器。最简单的设置方式是通过Docker。SQL Server 2022的说明[在这里](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-linux-ver16&preserve-view=true&pivots=cs1-bash#pullandrun2022)。要验证docker容器是否正在运行，请使用以下命令：

```py
watch -n 2 sudo docker ps -a
```

这将每2秒更新一次，STATUS列应显示类似于“Up X minutes”的内容，其中X是从容器启动以来经过的时间。

2\. Linux版Microsoft ODBC驱动程序17

说明[在这里](https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver16&tabs=alpine18-install%2Cubuntu17-install%2Cdebian8-install%2Credhat7-13-install%2Crhel7-offline#17)。我无法使用更新的驱动程序18连接到数据库，因此不能推荐使用该驱动程序。

3\. sqlcmd实用程序（可选）

sqlcmd实用程序允许你输入Transact-SQL语句，非常适合测试一切是否正常工作。详细说明[在这里](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools?view=sql-server-linux-ver16&tabs=redhat-install%2Credhat-offline#install-tools-on-linux)

# 加载包

将需要以下Julia包。当使用Pluto notebook时，其内置的包管理器将自动下载和安装它们。

```py
using ODBC, DBInterface, DataFrames
```

# 检查驱动程序

开放数据库连接（ODBC）驱动程序允许我们与SQL服务器建立连接。使用ODBC.jl包，我们可以检查系统上当前可用的驱动程序：

![](../Images/79ba7bdc20c596bace680f1525107983.png)

作者提供的图片

也可以在知道驱动程序位置后安装它。

![](../Images/7ec2324a8c4b1418f1604dfae6fe2805.png)

作者提供的图片

要移除驱动程序，请使用：

![](../Images/c8e50a0b01d995734149f6c1d75b3a08.png)

作者提供的图片

# 添加连接

使用完整的[连接字符串](https://www.connectionstrings.com/microsoft-odbc-driver-17-for-sql-server/)，我们现在可以连接到之前设置的本地运行SQL服务器。需要IP地址、端口、现有数据库名称、用户ID和密码。请注意，如果数据库名称未知，我们可以连接到‘master’，因为这个名称默认始终存在。

![](../Images/43d6843518f72b91eb4966564224e341.png)

作者提供的图片

# 列出所有现有数据库

使用**conn_master**对象，我们现在可以在服务器上执行查询。让我们列出所有数据库。

![](../Images/0b12897aec1ebe7ca3728c20742c3752.png)

作者提供的图片

# 创建新数据库

为了创建新数据库，我们应首先使用 **list_db** 函数检查名称是否已存在。如果没有，则按照下面的示例创建它，以‘FruitsDB’为例。

![](../Images/38c9b85cef2e6759f7497a9ed0cc6d5b.png)

作者提供的图片

再次列出所有数据库，我们可以验证‘FruitsDB’现在已经创建。

![](../Images/783a8be5329eeaac3bb7cd78e9e7bfb1.png)

作者提供的图片

# 创建新表

SQL Server 数据库可以包含多个表，这些表只是数据的有序集合。表本身是一系列行的集合，也称为记录。在我们开始填充表格之前，我们首先需要在现有数据库中创建它。作为示例，让我们在‘FruitsDB’中创建一个名为‘Price_and_Origin’的表。该表将包含三列——名称（String）、价格（Float）和来源（String）。请注意，VARCHAR(50) 用于表示 [可变长度字符串](https://learn.microsoft.com/en-us/sql/t-sql/data-types/char-and-varchar-transact-sql?view=sql-server-ver16) 数据。50 是字节大小，对于单字节编码，它也表示字符串的长度。

# 添加到新表

一旦表格存在，我们可以向其中添加数据。最简单的方法是使用 DataFrame 作为数据源。请记住，我们的表格‘Price_and_Origin’期望有三列：名称、价格和来源。因此，我们可以使用一些示例数据，如下所示：

![](../Images/f84705a5850c26c52191bde741cee681.png)

作者提供的图片

要插入值，我们可以使用 **DBInterface.executemany** 函数，它允许按顺序传递多个值。如下所示可以完成此操作。最终子句确保使用 **DBInterface.close!** 函数关闭数据库连接。这通常是一种良好的实践，有助于避免意外地将相同的连接用于其他用途。

![](../Images/bf068002c3482b0f0b3698eefddd53c5.png)

作者提供的图片

让我们验证数据库是否按预期填充。我们首先建立一个连接‘conn_fruit’，以连接到 SQL Server 上的‘FruitsDB’。然后，我们可以从表‘Price_and_Origin’中选择所有条目，并将其传递给 DataFrame 接收器。

![](../Images/1e532b908cf421dfecc5680b4da0eb55.png)

作者提供的图片

# 更新表格

按照前一节中显示的相同顺序，数据库现在可以使用新数据进行更新。

![](../Images/c1b92cfc1f67c25e134bb1f04d2d7e03.png)

添加新水果（图像来自作者）

让我们验证新数据是否确实存在于数据库中。

![](../Images/0317a5287971204b0cdb3200a9d86b93.png)

请注意，现在行数为 7（图像来自作者）

# 移除重复项

再次执行上面的 **add_to_fruit_table** 函数会向表中添加重复的行。

![](../Images/583d0dd94f71ffe004afbd3bd9dadc79.png)

“荔枝”和“梨”出现了两次（图像来自作者）

使用 [公共表表达式](https://learn.microsoft.com/en-us/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver16) (CTE)，我们可以从给定的表中删除重复的行。以下函数帮助我们实现这一点：

![](../Images/cdc9edc978c6035ba5d1ad5e67d6e498.png)

删除重复条目（图片来自作者）

检查行是否唯一。

![](../Images/ab3fa7b3fc090cdf11b356889be4eb90.png)

重复的条目已被删除（图片来自作者）

# 删除记录

在数据库中，通常需要从表中删除条目（符合某些条件）。例如，我们可以删除所有价格 > 95 的水果，如下所示：

![](../Images/26b3a444264a35295e03a871ca471e44.png)

价格超过 95 的水果已被删除（图片来自作者）

# 删除表

使用 **DBInterface.execute** 函数中的 DROP 语句，可以删除一个表。其余功能将保持与 **delete_rows** 相同。

```py
DBInterface.execute(conn_db, 
                   "DROP TABLE $table_name")
```

# 结论

**DBInterface.execute** 函数接受有效的 SQL 语句作为输入。因此，除了已展示的内容外，还可以执行所有种类的查询，详见 [此处](https://www.w3schools.com/sql/sql_select.asp)。如前所示，查询的结果可以轻松传递给 Julia DataFrame 接收器，然后可以用来执行其他操作。

[ODBC.jl](https://github.com/JuliaDatabases/ODBC.jl/tree/main) 和 [DBInterface.jl](https://github.com/JuliaDatabases/DBInterface.jl) 这两个包正在积极维护，并且与现有工作流（尤其是涉及 DataFrames 的工作流）集成良好。这为使用 Julia 进行数据分析和可视化开辟了令人兴奋的新可能性。希望你觉得这次练习有用。感谢你的时间！通过 [LinkedIn](https://www.linkedin.com/in/negivikas/) 与我联系或访问我的 [Web 3.0 网站](https://vikasnegi.eth.limo/)。

# 参考文献

1.  [https://odbc.juliadatabases.org/stable/#Getting-Started](https://odbc.juliadatabases.org/stable/#Getting-Started)

1.  [https://juliadatabases.org/DBInterface.jl/dev/](https://juliadatabases.org/DBInterface.jl/dev/)

1.  [https://www.w3schools.com/sql/default.asp](https://www.w3schools.com/sql/default.asp)

1.  [https://learn.microsoft.com/en-us/sql/?view=sql-server-linux-ver16](https://learn.microsoft.com/en-us/sql/?view=sql-server-linux-ver16)
