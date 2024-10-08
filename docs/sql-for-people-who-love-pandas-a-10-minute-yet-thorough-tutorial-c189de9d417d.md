# 30 个 SQL 查询通过它们的 Pandas 等效体进行解释

> 原文：[`towardsdatascience.com/sql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d`](https://towardsdatascience.com/sql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d)

## SQL 对喜欢 Pandas 的人来说变得更简单了

[](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------) ·阅读时间 10 分钟·2023 年 6 月 9 日

--

![](img/4804e3d33ecae544cceeee4a0f1df5f4.png)

图片由我使用 Midjourney 制作

## 动机

自 1974 年起 SQL 主导了数据世界，而 Pandas 在 2008 年出现，提供了内置可视化和灵活的数据处理等吸引人的功能。它迅速成为数据探索的首选工具，掩盖了 SQL 的光芒。

但不要被迷惑，SQL 仍然保持其地位。它是数据科学领域第二受欢迎且第三增长最快的语言（见这里）。所以，虽然 Pandas 抢占了风头，SQL 仍然是任何数据科学家必备的技能。

让我们看看当你已经了解 Pandas 时学习 SQL 有多么简单。

## 连接到数据库

设置 SQL 工作区并连接到示例数据库可能非常麻烦。首先，你需要安装你喜欢的 SQL 类型（PostgreSQL、MySQL 等）并下载一个 SQL IDE。在这里进行这些操作会偏离文章的目的，因此我们将使用一个快捷方式。

具体来说，我们将直接在 Jupyter Notebook 中运行 SQL 查询，而无需额外的步骤。我们需要做的只是使用 pip 安装`ipython-sql`包：

```py
pip install ipython-sql
```

安装完成后，启动一个新的 Jupyter 会话，并在笔记本中运行以下命令：

```py
%load_ext sql
```

一切准备就绪！

为了说明基本的 SQL 语句如何工作，我们将使用[Chinook SQLite 数据库](https://github.com/lerocha/chinook-database)，它包含 11 个表。

要将数据集及其 11 个表作为单独的变量加载到我们的环境中，我们可以运行：

```py
%sql sqlite:///data/chinook.db
```

这个语句以`%sql`内联魔法命令开头，这告诉笔记本解释器我们将执行 SQL 命令。接下来是下载的 Chinook 数据库所在的路径。

有效的路径应该始终以 `sqlite:///` 前缀开头，用于 SQLite 数据库。上面，我们连接到当前目录的 'data' 文件夹中存储的数据库。如果你想传递绝对路径，前缀应该以四个斜杠开头 - `sqlite:////`

如果你希望连接到不同的数据库风格，你可以参考这篇优秀的文章。

## 初步查看表格

我们在 Pandas 中总是首先使用 `.head()` 函数来初步查看数据。让我们学习如何在 SQL 中做到这一点：

![](img/dc7f4d416bfea6b0cacc14a983ddf7b5.png)

数据集也允许用于商业用途。

上述查询中的第一个关键字是 `SELECT`。它相当于 Pandas 中的括号运算符，用于选择特定列。但 SELECT 关键字后面跟着一个 *（星号）。***** 是一个 SQL 操作符，用于从 `FROM` 关键字之后指定的表中选择所有内容（所有行和列）。LIMIT 用于最小化返回的输出。因此，上述查询等同于 `df.head()` 函数。

如果你不想选择所有列，你可以在 SELECT 关键字后指定一个或多个列名：

![](img/e3c518e6fc2cdd19719295689634550e.png)

等效的 Pandas 操作是：

```py
tracks[['Name', 'Composer', 'UnitPrice']].head(10)
```

SQL 中另一个有用的关键字是 `DISTINCT`。在任何列名之前添加此关键字将返回其唯一值：

![](img/125978d65bce95c1364fb4730bcdee41.png)

SQL 中的注释是用双短横线写的。

## 计算行数

就像 Pandas 的 DataFrames 上有 `.shape` 属性一样，SQL 有 `COUNT` 函数来显示表中的行数：

```py
%%sql

SELECT COUNT(*) FROM tracks
```

![](img/8a30d20e157f0b8ef126e0ec0aa776d2.png)

更有用的信息是计算特定列中唯一值的数量。我们可以通过将 DISTINCT 关键字添加到 COUNT 中来做到这一点：

![](img/a8db55163ef326a09e4d2ef03b920ca6.png)

## 使用 WHERE 子句过滤结果

仅仅查看和计算行数有点无聊。让我们看看如何基于条件过滤行。

首先，让我们查看价格超过一美元的歌曲：

![](img/b01521ec956e0db7dd095502ba029040.png)

价格超过一美元的歌曲。

条件语句写在 WHERE 子句中，WHERE 子句总是位于 FROM 和 LIMIT 关键字之间。使用条件与在 Pandas 中类似，但我敢说 SQL 版本更易读。

你也可以在使用条件时使用 COUNT 函数。例如，让我们查看价格在 1 到 10 美元之间的歌曲数量：

正如你所看到的，SQL 版本要易读得多。

![](img/a6c7eee67f788d3de10c5e57e6788542.png)

价格在 1 到 10 美元之间的歌曲数量。

上面我们使用布尔运算符 AND 链接了两个条件。其他布尔运算符（OR、NOT）也类似使用。

现在，让我们查看所有开票城市为巴黎 *或* 柏林的发票：

![](img/4824e7acf91c1958acbf49ab817b4c3b.png)

从柏林或巴黎开具的发票

SQL 中的**等于**运算符只需要一个‘=’（等号）。**不等于**运算符用‘!=’或‘<>’表示：

![](img/ffad28285f3fea6e1f373cbf9e4132f1.png)

账单城市不是柏林或巴黎的发票

## 使用 BETWEEN 和 IN 进行更简单的过滤

类似的条件非常常用，用简单的布尔运算符编写起来会很麻烦。例如，Pandas 有 `.isin()` 函数检查一个值是否属于值列表。

如果我们想选择五个城市的发票，我们将不得不编写五个链式条件。幸运的是，SQL 支持类似 `.isin()` 的 IN 运算符，所以我们不需要：

![](img/327884228f87e946f7439adaf40a86c3.png)

IN 之后的值列表应该给出为元组，而不是列表。你也可以用 NOT 关键字来否定条件：

![](img/82549dfe24a4c8c67c1aa10a9b19c437.png)

对数字列的另一个常见过滤操作是选择范围内的值。为此，可以使用 BETWEEN 关键字，这等同于 `pd.Series.between()`：

![](img/4847efd137d14dc78ff08b57d62e06b4.png)

选择账单金额在 5 到 15 之间的发票。

## 检查空值

每个数据源都有缺失值，数据库也不例外。就像有几种方法可以在 Pandas 中探索缺失值一样，SQL 中有特定的关键字用于检查空值的存在。下面的查询计算 BillingState 中缺失值的行数：

![](img/83423d4f16fa55426964a7a000f18c11.png)

你可以在 IS 和 NULL 之间添加 NOT 关键字，以丢弃某一列的缺失值：

![](img/012687577373b387bf9dbdf4286dd5c7.png)

## 使用 LIKE 进行更好的字符串匹配

在 WHERE 子句中，我们根据精确的文本值过滤列。但通常，我们可能希望根据模式过滤文本列。在 Pandas 和纯 Python 中，我们会使用正则表达式进行模式匹配，这非常强大，但正则表达式需要时间来掌握。

作为替代，SQL 提供了‘%’通配符作为占位符，可以匹配任意字符 0 次或多次。例如，‘gr%’ 字符串匹配‘great’，‘groom’，‘greed’，‘%ex%’ 匹配任何中间有‘ex’的文本等。让我们看看如何在 SQL 中使用它：

![](img/3027ca269d06ef9e1a87879479a72d61.png)

上述查询找到所有以‘B’开头的歌曲。包含通配符的字符串应出现在 LIKE 关键字之后。

现在，让我们找到所有标题中包含‘beautiful’一词的歌曲：

![](img/54dd60788a78237eaa41b85e9cff6ede.png)

你还可以在 LIKE 旁边使用其他布尔运算符：

![](img/1720b5fe6e18b784bda4f6db6122e87c.png)

SQL 中还有许多其他通配符具有不同的功能。你可以在[这里](https://www.w3schools.com/sql/sql_wildcards.asp)查看完整列表及其用法。

## SQL 中的聚合函数

对列进行基本的算术运算也是可能的。这些运算在 SQL 中称为聚合函数，最常见的有`AVG、SUM、MIN、MAX`。它们的功能从名称中应当可以清楚地了解到：

![](img/f10d9d963c9631af2a374f9c040bf26c.png)

聚合函数只会对你使用它们的列给出一个结果。这意味着你不能对一个列进行聚合并选择其他未聚合的列：

![](img/be6816913bd9ab3e0f3a9288c47e37cc.png)

你还可以使用`WHERE`子句将聚合函数与条件结合使用：

![](img/83b9f728d822c748e59f368e21c23d24.png)

也可以对列和简单数字使用算术运算符，如 +、-、*、/。当作用于列时，操作是逐元素进行的：

![](img/0e944272637a2ea34be7c5ce7406e5bf.png)

关于算术运算，有一点需要注意：如果你仅对整数执行操作，SQL 会认为你期望整数作为答案：

```py
%%sql
```

```py
SELECT 10 / 3
```

![](img/2cc93888d6c83c6e87e5b7d0368febd6.png)

结果是 3 而不是 3.33…。为了获得浮点结果，你应在查询中使用至少一个浮点数或使用全部浮点数以确保安全：

```py
%%sql
```

```py
SELECT 10.0 / 3.0
```

![](img/2432fdca12e14d0a42b848f8fc0b83bd.png)

利用这些知识，让我们计算歌曲的平均时长（分钟）：

![](img/2c5b0339aedae5044c05d0283519ae82.png)

如果你注意到上面的列，它的名字写作“*生成该列的查询*。”由于这种行为，使用长计算，如计算列的标准差或方差，可能会成为问题，因为列名将和查询本身一样长。

为了避免这种情况，SQL 允许使用别名，类似于 Python 中的导入语句别名。例如：

![](img/533ccfaee872774e4c06b69c86b72455.png)

在`SELECT`语句中的单个项后使用`as`关键字告诉 SQL 我们正在使用别名。这里是更多的示例：

![](img/dd8bb65d466cc86d6be7896e4e50504e.png)

对于长名称的列，你也可以同样轻松地使用别名。

## SQL 中的结果排序

就像 Pandas 有`sort_values`方法一样，SQL 通过`ORDER BY`子句支持对列进行排序。在子句后传递列名会将结果按升序排列：

![](img/5e1d89c8ce00f2d1ef9a301f7bc33924.png)

我们按作曲家的名字升序排列`tracks`表。请注意，`ORDER BY`语句应始终在`WHERE`子句之后。也可以将两个或多个列传递给`ORDER BY`：

![](img/404087b630ec2fbf87892ebf4042f8bc.png)

你还可以通过在每个列名后传递`DESC`关键字来反转排序：

![](img/39e48cc6cf81385ea9c6a0934a32fbea.png)

上述查询在按`UnitPrice`和`Compose`降序排列以及`name`升序排列后返回三列（`ASC`是默认关键字）。

## SQL 中的分组

Pandas 最强大的功能之一是`groupby`。你可以用它将表格转变成几乎任何你想要的形状。在 SQL 中，与之非常接近的是`GROUP BY`子句，也可以用来实现相同的功能。例如，下面的查询计算了每个流派的歌曲数量：

![](img/3c12aa5902654883336982ec27625a74.png)

SQL 中的 GROUP BY 和 Pandas 中的`groupby`之间的区别在于 SQL 不允许选择在 GROUP BY 子句中没有给出的列。例如，在上面的查询中添加一个额外的自由列会产生错误：

然而，你可以在 SELECT 语句中选择任意多的列，只要你在这些列上使用了某种聚合函数：

![](img/00c7826c53ce7056b61a5391803e32a6.png)

上面的查询包括了我们迄今为止学习的几乎所有主题。我们按专辑 ID 和流派 ID 进行分组，并为每个组计算了歌曲的平均时长和价格。我们也有效地利用了别名。

我们可以通过按平均时长和流派数量排序来使查询更强大：

![](img/45426a4b24bb076f7fd4fde15730e1a5.png)

注意我们在 ORDER BY 子句中如何使用聚合函数的别名。一旦你对列或聚合函数的结果进行了别名，你可以在查询的其余部分只通过别名来引用它们。

## 使用 HAVING 条件

默认情况下，SQL 不允许在 WHERE 子句中使用聚合函数进行条件过滤。例如，我们想选择歌曲数量大于 100 的流派。让我们用 WHERE 子句尝试一下：

基于聚合函数结果过滤行的正确方法是使用 HAVING 子句：

![](img/10f729ee3f781fa996eff4bd3655ba0e.png)

HAVING 子句通常与 GROUP BY 一起使用。每当你想使用聚合函数过滤行时，HAVING 子句是最合适的选择！

> 所有图片均由我自己生成。

## 摘要

到现在为止，你应该已经意识到 SQL 有多强大，以及与 Pandas 相比，它变得多么可读。尽管我们学到了很多，但我们仅仅是触及了表面。

对于练习题，我推荐[Data Lemur](https://datalemur.com/questions?category=SQL)或者[LeetCode](https://leetcode.com/)，如果你有冒险的心情。

喜欢这篇文章及其奇特的写作风格？想象一下能访问到更多类似的文章，全部由一个才华横溢、迷人、机智的作者（顺便说一下就是我 :)）撰写。

只需支付 4.99$的会员费用，你将不仅可以访问我的故事，还能获得来自 Medium 上最杰出的头脑的知识宝库。如果你使用[我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我的超级感激和一个虚拟的击掌以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----c189de9d417d--------------------------------) [## 使用我的推荐链接加入 Medium — Bex T.

### 获取对我所有⚡优质⚡内容的独家访问权限，以及在 Medium 上无限制地访问所有内容。通过为我购买一份来支持我的工作…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----c189de9d417d--------------------------------)
