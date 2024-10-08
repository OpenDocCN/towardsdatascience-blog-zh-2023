# 思考 SQL —— 避免从上到下编写 SQL

> 原文：[`towardsdatascience.com/think-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59`](https://towardsdatascience.com/think-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59)

## 通过理解逻辑查询处理顺序来编写清晰的 SQL

[](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------) ·7 分钟阅读·2023 年 2 月 2 日

--

![](img/4b40ea0f0eb8d5415ec9881ec750cd1a.png)

图片由 [Jeffrey Brandjes](https://unsplash.com/es/@jeffreyfotografie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/7cLqEYJws8E?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

由于 SQL 的声明式特性，你可能会觉得编写 SQL 有挑战。尤其对于那些熟悉 Python、Java 或 C 等命令式语言的工程师来说，SQL 切换起来是许多人需要的思维转换。思考 SQL 与任何命令式语言都不同，因此学习和开发的方式也不应相同。

当使用 SQL 时，你是按照从上到下的方法编写的吗？你是否从 SQL 中的“SELECT”子句开始开发？在这篇文章中，让我们探讨 SQL 的逻辑查询处理顺序，帮助你理解为何可能需要改变从上到下编写 SQL 的方式。这也有助于你更清晰地思考 SQL，并更有效地开发查询。

# SQL 是一种声明式语言

声明式语言的核心概念非常棒：人类传达的是高级结构和逻辑指令，而不需要精确的流程。作为开发者，这意味着我们不需要编写**如何实现目标**的逐步指南。我们只需编写**要实现的目标**，这样我们可以更多地关注结果，让底层查询解析器和引擎来决定如何执行指令。

编写 SQL 时，你更关注输入和输出，而执行是幕后的神奇黑匣子。对于详细的执行，用户提供计划和优化的偏好。但执行仍然由查询引擎在运行时处理。

然而，这并不意味着我们可以挥舞魔法棒，一切都会如愿以偿。仍然需要高级指令来提供以下信息。

+   数据源在哪里（**FROM**）

+   应用什么条件（**WHERE/HAVING**）

+   是行级别还是聚合（**GROUP BY**）

+   如何显示结果（**SELECT**）

+   如何排序数据（**ORDER BY**）

+   返回多少行（**LIMIT**）

与获取数据的命令式语言不同，我们不需要编写复杂的逻辑或逐行循环。SQL 查询就像一个蓝图，SQL 引擎是构建者。作为用户，我们只需等待输出，而不必担心它如何获取结果。

# SQL 逻辑查询处理顺序

如果你曾使用过其他数据处理框架的命令式语言，甚至对于 Spark 或 Pandas 用户，我们总是需要先有数据集进行处理。获取数据源的地方通常是任何数据处理框架的入口点，SQL 也是如此。

要理解 SQL 逻辑查询处理顺序，让我们用 SQL 查询在语法时间来比较它与逻辑处理顺序。

![](img/ac7af64a6eaa33b5832dd1303f64084a.png)

SQL 语法与逻辑查询处理顺序标记 | 图片由作者提供

1.  **FROM:** SQL 的入口点是 **FROM** 子句。它通常在“SELECT”语句之后定义，并作为准备阶段来指导查询引擎从哪些表中提取数据

1.  **ON**: 在每个表中评估的条件，用于检查要连接的键

1.  **JOIN**: 一旦知道要连接哪些键，SQL 引擎会检查需要应用哪些类型的连接（内连接、外连接、交叉连接等）

1.  **WHERE**: 应用行级过滤器

1.  **GROUP BY:** 指定要聚合的键，并改变原始行级表的视图。在这个阶段之后，所有处理的内容都会是聚合级别而不是原始行级别。如果使用了 cube 或 roll-up 聚合，也会在这个阶段发生。

1.  **HAVING:** 应用聚合级过滤器。我们还可以编写嵌套查询或 CTE（公共表表达式）在聚合级别进行过滤。不过，紧挨着 GROUP BY 子句使用更为方便，并且可能有利于 SQL 引擎优化。

1.  **SELECT:** 选择要显示给用户的字段。对于需要复杂逻辑的派生字段，如窗口函数（rank()、row_number() 等）、或 case 语句，或聚合函数，这些操作都会在此时进行。如你所见，SELECT 在 SQL 逻辑查询处理顺序中较晚。如果你先从 SELECT 子句开始，可能很难预见你会如何编写其他 6 个语句，从而可能导致 SELECT 中出现意外结果或错误。让我们在下一节中详细讨论。

1.  **ORDER BY**: 最终结果的排序顺序。在这个阶段，我们可以解析你在 SELECT 子句中定义的别名。

1.  **LIMIT：** 返回结果的数量，或者如果你想跳过几行顶部数据，可以结合 ORDER BY 使用。

让我们把 SQL 语句从语法时间重新排列成 SQL 逻辑查询处理顺序，这样会更清晰。

![](img/307298e53525e69c92c39223314b54ed.png)

SQL 逻辑查询处理顺序 | 作者图像

微软在 [SELECT 语句的逻辑处理顺序](https://learn.microsoft.com/en-us/sql/t-sql/queries/select-transact-sql?redirectedfrom=MSDN&view=sql-server-ver16#logical-processing-order-of-the-select-statement) 上有很好的文档。

# “SELECT” 不应该是编写 SQL 时的第一个单词

“SELECT” 通常是阅读 SQL 语句时的第一个子句。我们的脑袋在阅读或写作时，通常采用自上而下的方法。首先使用 “SELECT” 定义了我们希望显示的结果。将 “SELECT” 作为第一个单词符合这种模式。此外，将 “SELECT” 放在首位是语法正确的，可以编译并运行整个 SQL 脚本。

当你为数据分析编写 SQL 查询时，有多少次你首先写下了以下内容：

> SELECT col1, col2, CASE (…), ROW_NUMER(…)

有效！一个编译并正确提取结果的 SQL。

但是，正如你在上面看到的，**SELECT 在 SQL 逻辑查询处理顺序中被评估得较晚。** 以下是避免首先在 SELECT 中编写所有内容的一些原因：

+   **很难预测你会在 SELECT 之前为语句写些什么。** 你有所有的表名吗？你有定义别名吗？你是在处理行级数据还是汇总级数据？我知道有人可以很好地预测所有 SQL 以及所有表和别名。不幸的是，我不擅长预见和记忆。按逻辑查询处理顺序编写 SQL 给了我下一步要写的参考。如果在编码时被打断，它也给我一个提示，使我更容易从中断处恢复。

+   **逻辑查询处理顺序更适合我们的脑袋。** 如果你编写 SQL，可能熟悉 ETL 概念（提取、转换、加载）数据。ETL 概念遵循逻辑顺序：你获取数据集，对数据集进行操作，将重构后的数据集放到其他地方。在 SQL 中编写应该遵循相同的逻辑顺序，“SELECT” 更适合作为转换或加载阶段，我们应该先准备数据集，以便为 “SELECT” 提供数据。

+   **它有助于调试和推理错误。** 通过遵循逻辑查询处理顺序，一些错误变得明显。例如这个 StackOverflow 问题：“[SQL 在 WHERE 子句中未识别列别名](https://stackoverflow.com/questions/28802134/sql-not-recognizing-column-alias-in-where-clause)”，通过这篇文章，我们可以快速回答：SELECT 别名尚未被评估，因此 SQL 引擎没有你给定的别名的上下文，因此失败了。*（一些现代 SQL 供应商进行了一些优化以避免这个问题，但并非所有 SQL 提供商都适用，我们仍应注意这一点）*。

# **编写 SQL 的技巧：逻辑查询处理顺序**

+   我们仍然可以先写“SELECT”，但只写这 6 个字母；将其作为占位符，以提醒我们在内容准备好时填入剩余的内容。

+   从 ETL 角度思考，数据准备对于为其他工作奠定基础至关重要。因此，首先需要关注的是正确编写**FROM、ON 和 JOIN** 语句。

+   如果遇到错误，请按照逻辑查询处理顺序进行调试。`SELECT *` 在生产环境中很糟糕，但我们仍然可以在调试中使用它。我们还可以通过 LIMIT 子句进一步防止拉取过多的数据。

# 最终思考

如果你从上到下编写 SQL 并且这种方法让你在思考 SQL 时感到困扰，你应该考虑理解 SQL 逻辑查询处理顺序并进行这种练习。

我在准备 Microsoft Exam 70–461 时很早就学到了这种方法，[Itzik Ben-Gan](https://www.amazon.com/stores/Itzik-Ben-Gan/author/B001IGQENW?ref=ap_rdr&store_ref=ap_rdr&isDramIntegrated=true&shoppingPortalEnabled=true) 有一本很棒的书 [Querying Microsoft SQL Server 2012 (MCSA)](https://www.amazon.com/Training-70-461-Querying-Microsoft-Server/dp/0735666059)。这本书的优点在于它对 SQL 基础的解释远胜于其他书籍中那些花哨的语法。

思考 SQL 是至关重要的。你可以推理事情为何如此发生，并迅速解决错误。此外，它帮助你更好地组织代码，以更有条理的方式解决复杂查询。

我希望这个故事对你有帮助。本文是我工程与数据科学故事的**系列之一**，目前包括以下内容：

![Chengzhi Zhao](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----476a67f53a59--------------------------------)53 个故事![](img/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你也可以[**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe)或成为[**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，享受对 Medium 上所有故事的无限访问权限。

如有问题/评论，**请随时在本文评论区留言**，或通过[Linkedin](https://www.linkedin.com/in/chengzhizhao/)或[Twitter](https://twitter.com/ChengzhiZhao) **直接联系我**。
