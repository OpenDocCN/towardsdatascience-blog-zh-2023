# Delta Lake — 自动模式演变

> 原文：[`towardsdatascience.com/delta-lake-automatic-schema-evolution-11d32bd1aa99`](https://towardsdatascience.com/delta-lake-automatic-schema-evolution-11d32bd1aa99)

## 合并演变数据框时发生了什么，以及你可以/不能做的事情

[](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)![Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 10 日

--

![](img/1b8dfc96acde282413eda93c212cb337.png)

图片由[McDobbie Hu](https://unsplash.com/@hjx518756?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在上一篇文章中，我们讨论了事务日志和[如何保持 Delta 表快速和干净](https://medium.com/p/3c9d4f9e2f5e)。这次我们将讨论 Delta 表中的自动模式演变。

模式演变是管理数据随时间变化的关键方面。数据源不断发展以适应新的业务需求，这可能意味着需要从现有的数据模式中添加或删除字段。作为数据使用者，快速而灵活地适应数据源的新特性是至关重要的，而自动模式演变使我们能够无缝地适应这些变化。

在这篇文章中，我们将讨论在使用[Delta](https://delta.io/)时的自动模式演变，并使用[people10m 公共数据集](https://learn.microsoft.com/en-us/azure/databricks/dbfs/databricks-datasets#create-a-table-based-on-a-databricks-dataset)（这是在 Databricks Community Edition 上提供的）进行测试。我们将在几个场景中测试添加和删除字段。

# 设置

自动模式演变可以通过两种方式启用，具体取决于我们的工作负载。如果我们进行盲目追加，只需启用***mergeSchema***选项即可：

如果我们使用合并策略插入数据，则需要通过将***spark.databricks.delta.schema.autoMerge.enabled***设置为***true***来启用它。

在这篇文章中，我们将使用合并策略，因此我们将选择后者。

我们已经设置好了，可以加载我们的 Delta 表，它应该如下所示：

![](img/8839e62234f398505c236f99438d592c.png)

初始数据集

# 模式演变

为了模拟模式演变，我们将使用手动创建的模式创建自定义 DataFrame，并使用 Scala 的 [Delta API](https://docs.delta.io/latest/delta-update.html#upsert-into-a-table-using-merge) 合并它们。

免责声明：我们将对模式进行的所有更新只是示例，并不意味着有太大意义。

初始 DataFrame 模式

模拟和合并新记录

## 添加字段

假设我们公司希望友好对待昵称，员工可以用他们喜欢的昵称被称呼（真棒！）。

我们将向当前模式中添加一个名为*nickName*的新字段，并更新 Pennie 的 nickName（id 编号 1）。

带有 nickName 的模式

![](img/76766b715befdb9bb7772984303adfaa.png)

添加新字段

如我们所见，添加了一个新字段，Pennie 现在可以用她的新喜欢的昵称称呼！注意到所有其他记录的值都自动填充为 null。

## 删除字段

由于添加了昵称，大家开始思考为何没有人使用他们的中间名，因此决定删除它。

无中间名的模式

我们还将更新 Quyen 的昵称，但由于源删除了字段，她的中间名将不会存在。表格应该如何处理？

![](img/36d9e751d84a3a69ffbd630467ba2335.png)

删除中间名后的表

如果你什么也没猜到，那你是对的。每个当前目标表记录保持不变，只有新记录的*middleName*将是*null*。

为了展示这一点，我们将插入一个新的 id（0）。

![](img/c5dd1de121806ddf7bc081c97bc07586.png)

插入新记录后的表

## 重命名列

重命名列与删除一列并用新名称添加另一列相同。如果你希望在原地重命名列，请参阅 [Delta 列名映射](https://docs.databricks.com/delta/delta-column-mapping.html#rename-a-column)。

我不会进一步深入这个话题，因为尽管这是一种模式演变，但它不是自动的。请记住，这个功能是不可逆的，一旦启用，你将无法关闭它。

## 更改列类型/顺序

更改列类型或列顺序也不是自动模式演变的一部分。

## 在结构中添加/删除字段

假设我们添加了一个员工历史结构，其中包括*startDate*和*endDate*，以追踪员工何时开始和离开工作。

![](img/b758f6669537daf41bfc7ae793dd3cb8.png)

为了更完整的历史记录，我们现在希望包括*title*以追踪员工在公司的职业生涯。

更新了带有‘title’的结构

![](img/61777ff2afd04f1c474f7d9eb4b6502c.png)

如我们所见，向结构中添加字段也不是问题。如果我们尝试删除新添加的字段，它也会成功。添加和删除结构中的字段与在根中执行的方式相同。

## 在结构数组中添加/删除字段

现在我们要处理更复杂的情况。在这种情况下，我们将向数组中的结构体添加一个新字段。假设我们现在有一个属于某个员工的设备数组：

![](img/1fd9959a1f975febf5e503df3c8b980b.png)

为了展示在数组中添加新字段的情况，我们将向结构体中添加一个*serial_num*，以便更好地跟踪设备。

在数组中更新的结构体添加了‘serial_num’

![](img/71e939fa1fa139c21dbe30d28157f5da.png)

正如我们所见，这也按预期工作。表模式已更新，新记录具有相应的*serial_num*，而旧记录的*serial_num*被填充为*null*值。

如果我们再次移除新添加的字段，它会按预期工作。

## 在结构体的映射中添加/删除字段

现在是时候在映射中测试相同的操作了。我们添加了一个名为*connections*的新列，该列将负责存储每个员工的层级。

![](img/0080df5b27d737475f8c9ee7a4454e92.png)

为了模拟更新，我们将向*connections*列中的结构体添加一个名为*title*的新列。

在映射中更新的结构体添加了‘title’

![](img/5ec4b487a79f3f62ed9847b708c14d18.png)

这一次，删除返回*AnalysisException*的字段，这意味着 MapType 转换不被很好地支持。

经简要调查，我发现这是因为[castIfNeeded](https://github.com/delta-io/delta/blob/master/core/src/main/scala/org/apache/spark/sql/delta/UpdateExpressionsSupport.scala#L53)函数尚不支持 MapTypes。我已[报告了一个错误](https://github.com/delta-io/delta/issues/1641)，并将尝试解决这个问题。

**编辑：** [`github.com/delta-io/delta/pull/1645`](https://github.com/delta-io/delta/pull/1645)

# 结论

在这篇文章中，我们讨论了在几种不同情况下添加和删除字段的问题。我们得出结论，Delta 中的自动模式演变非常完善，支持大多数复杂场景。通过允许这些场景，我们可以避免在数据源演变时手动干预以更新模式。这在处理数百个数据源时特别有用。

作为额外的奖励，我们还发现了 MapTypes 中不支持的一个遗漏案例，这是一个很好的机会为这样一个出色的开源项目做出贡献。

希望你喜欢这篇文章！请确保关注更多内容！
