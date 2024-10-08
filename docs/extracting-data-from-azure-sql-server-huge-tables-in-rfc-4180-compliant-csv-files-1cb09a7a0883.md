# 从（Azure）SQL Server 大型表中提取数据到 RFC 4180 兼容 CSV 文件

> 原文：[`towardsdatascience.com/extracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883`](https://towardsdatascience.com/extracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883)

## 从（Azure）SQL Server 大型表中提取包含特殊字符的字符串到 CSV 文件中的噩梦

[](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)![Luca Zavarella](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------) [Luca Zavarella](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------) ·阅读时间 21 分钟·2023 年 1 月 5 日

--

![](img/623f3bda214162d325d7158441329fe3.png)

（图片来自 Unsplash）

当一组来自公司外部的数据科学家被聘用来实现机器学习模型时，你必须以某种方式与他们分享用于模型训练的数据。如果上述团队无法直接访问数据库中持久化的数据，第一种选择是将数据从数据库中提取到 CSV 格式的文件中。考虑到这些数据通常是大量的（超过 5 GB），而且某些字段可能包含特殊字符（逗号，与字段分隔符重合；回车和/或换行符），非开发用户使用的通常导出工具可能不够合适，甚至可能导致内存问题。

在本文中，你将看到如何使用 PowerShell 函数解决从（Azure）SQL Server 数据库中提取包含特殊字符的大量数据到 RFC 4180 兼容 CSV 文件中的问题。

当你需要从（Azure）SQL Server 数据库中提取数据时，用户首先想到的工具是[*SQL Server Management Studio*](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?WT.mc_id=AI-MVP-5003688)（SSMS）和*Azure Data Studio*（ADS）。这是因为这两者都包含简单的功能，允许你通过几次点击从数据库中提取数据。

与（Azure）SQL Server 完美配合的工具是 SSMS。最近，微软在向 ADS 中添加功能方面进行了大量投资，使其成为 Azure 及其他平台上微软数据平台的首选工具。因此，当你安装最新版本的 SSMS 时，安装程序也会在后台安装 ADS。

任何涉及导入 CSV 文件以加载数据集的第三方系统必须基于一个定义 CSV 格式的标准。因此，在进行实际测试之前，让我们看看是否有 CSV 格式的标准定义。

# 什么是 RFC 4180

RFC 4180 是一个标准，它规范了用于*逗号分隔值*（CSV）文件的格式和与 CSV 格式相关的特定*多用途互联网邮件扩展*（MIME）类型（“text/csv”）。该标准的内容可以在这里找到：

[## RFC 4180：逗号分隔值（CSV）文件的通用格式和 MIME 类型

### INFORMATIONAL 7111 Errata Exist 网络工作组 Y. Shafranovich 请求评论：4180 SolidMatrix…

www.rfc-editor.org](https://www.rfc-editor.org/rfc/rfc4180?source=post_page-----1cb09a7a0883--------------------------------)

如前面链接中格式的定义所示，虽然前四点比较明显，但其余三点需要仔细阅读：

+   每个字段*可能被括在双引号内，也可能不被括在双引号内*（但

    一些程序，如 Microsoft Excel，不使用双引号

    如果字段没有被双引号括起，那么

    双引号可能不会出现在字段内。

+   包含换行符（CR/LF）、双引号和逗号的字段

    *应被括在双引号内*。

+   如果使用双引号来括起字段，那么一个双引号

    出现在字段内的*必须通过在其前面加上转义字符*来进行转义

    另一个双引号*。

同时考虑链接中给出的示例，可以看出字段的值只有在必要时才会用双引号括起。对于那些只需部分值使用双引号的字段，将所有值都用双引号括起来是没有意义的。

当你需要使用 CSV 格式文件与第三方系统共享信息时，适用以下内容：

> 重要的是，你从导出中生成的 CSV 文件必须符合 RFC 4180，以确保文件可以被任何提供 CSV 文件导入功能的外部系统读取。

为了测试上述工具如何以 CSV 格式提取数据，让我们创建一个包含 RFC 4180 标准中提到的特殊字符和 Unicode 字符的简单表，以确保文本字段内容的通用性。

# 创建一个包含特殊字符的虚拟表

首先，你需要使用以下脚本在 SQL Server 实例中创建`extract_test`表：

```py
CREATE TABLE [dbo].extract_test NOT NULL,
 [name] nvarchar NULL,
 [notes] nvarchar NULL
)
```

然后，你可以使用以下脚本向该表中添加数据：

```py
SET IDENTITY_INSERT [dbo].[extract_test] ON 
GO
INSERT [dbo].[extract_test] ([id], [name], [notes]) VALUES (1, N'Luca', N'let''s add a carriage return
here')
GO
INSERT [dbo].[extract_test] ([id], [name], [notes]) VALUES (2, N'Zavarella, Luca', N'the name contains a comma')
GO
INSERT [dbo].[extract_test] ([id], [name], [notes]) VALUES (3, N'Luca Zavarella', N'here we have a comma and a double quotation mark: ,"')
GO
INSERT [dbo].[extract_test] ([id], [name], [notes]) VALUES (4, N'秋彦', N'this japanese name means "bright prince"')
GO
SET IDENTITY_INSERT [dbo].[extract_test] OFF
GO
```

从 INSERT 语句的内容中可以看出，我们提供了标准中提到的所有特殊字符。我们还使用了日文字符，以便验证 CSV 文件是否正确使用 Unicode 字符表进行编写。

显然，此情况下创建的表不会是 5 GB 的表，而是包含特殊字符以测试 CSV 格式导出的。这里是 ADS 中的 SELECT 输出：

![](img/eaf2a39c72049f69be92efdf94cd812c.png)

图 1 — 在 ADS 中输出的虚拟表内容（作者提供）

不必担心 ADS 或 SSMS 的输出网格中未显示回车符。由于 INSERT 该行的方式，回车符确实存在。

所以，让我们尝试使用 SSMS 和 ADS 从这个表中提取数据。

# 使用微软用户友好的工具来提取数据

我们首先尝试使用传统工具，即 SQL Server Management Studio 来进行操作。

## 使用 SSMS 提取数据

打开 SSMS 并连接到数据库实例后，右键点击刚创建表所在的数据库名称，选择*任务*，然后选择*导出数据*：

![](img/0e5a5cd2336ac931a44980e7caa97902.png)

图 2 — 使用 SSMS 从数据库导出数据（作者提供）

您将看到一个描述提取数据活动的初始屏幕。如果继续操作，将会看到这个窗口：

![](img/6771f58266f92bbcff0a8ae2331beae9.png)

图 3 — 从导出向导中选择数据源（作者提供）

选择 SQL Server 客户端数据源，输入您的服务器实例名称，然后选择登录数据库所使用的身份验证方式。在我的例子中，因为测试表存储在 Azure SQL 数据库上，我使用了 SQL Server 身份验证来访问我的*test-sql-bug*数据库，如*图 3*所示。

在向导的下一个屏幕上，您可以选择导出目标。在我们的例子中，选择*平面文件目标*，通过*浏览*按钮在您首选的文件夹中创建一个 CSV 目标文件（记得在点击浏览后打开的窗口中选择 CSV 扩展名）。记得勾选*Unicode*标志，以确保还处理了我们示例中的日文字符。之后，选择*分隔符*作为格式，将*文本限定符*保持为“*<none>*”。还要确保勾选了“*首行数据中的列名*”标志。然后点击*下一步*：

![](img/c0449416dd54667408166e7b20be2d06.png)

图 4 — 选择数据输出的目标（作者提供）

在下一个窗口中选择*从一个或多个表或视图中复制数据*，然后再次点击*下一步*。

在出现的配置窗口中，您可以选择表` `[dbo].[extract_text]` 作为*源表或视图*。对于其他选项，您可以保持默认设置，因为行分隔符（*CR\LF*）和列分隔符（*逗号*）均按照 RFC 4180 标准定义。然后点击*下一步*：

![](img/cd514fc41e93dd51c7ee8655eb5a34d3.png)

图 5 — 配置平面文件目标选项（作者提供）

在下一个窗口中保持 *立即运行* 选项被选中，然后点击 *完成*。将出现选项摘要窗口。再次点击 *完成* 开始提取。完成后，点击 *关闭*。

如果你现在尝试用文本编辑器（而不是 Excel）打开输出的 CSV 文件，你会注意到以下内容：

![](img/8ef38686fd3879fea02e5673ccb17c5a.png)

图 6 — SSMS 导出向导输出（没有文本限定符）（作者提供）

基本上，在这种情况下，导出向导会提取每个文本字段的内容，而不管它是否可能包含特殊字符（逗号和回车）。这意味着文本字段中包含的任何回车都会被系统解释为行分隔符，就像文本字段中包含的任何逗号会被解释为字段分隔符一样。另一方面，Unicode 字符已被正确处理。因此，生成的 CSV 文件将无法被任何需要导入这些信息的第三方系统识别为正确。

如果你尝试重复导出，这次将双引号作为文本限定符输入，你将得到以下结果：

![](img/0a30ade9cdc7cbcb38a845d1c32daf9d.png)

图 7 — 使用双引号作为文本限定符的 SSMS 导出向导输出（作者提供）

在这种情况下，所有提取的值都被双引号包围，包括标题。然而，这会强迫必须读取数据的外部系统将所有数字值视为字符串。此外，如果文本字段中的值包含双引号字符，则不会转义，从而给外部系统带来解析问题。因此，再次生成的 CSV 文件将无法被任何需要导入这些信息的第三方系统识别为正确。

关于在非常大数据量上进行提取操作的可扩展性，毫无问题，因为导出向导使用 *SQL Server 集成服务*（SSIS）作为其引擎，SSIS 开发用于处理巨大的数据量。

此外，有时你可能需要对数据源的数据类型采取措施，以避免在使用导出向导时出现一些错误，如本博客文章所强调：

[## 如何将数据从 SQL Server 导出到平面文件](https://www.sqlshack.com/export-data-sql-server-flat-file/?source=post_page-----1cb09a7a0883--------------------------------)

### 2017 年 12 月 7 日 在这篇文章中，我们将展示如何使用…将 SQL Server 数据导出到平面文件中。

[www.sqlshack.com](https://www.sqlshack.com/export-data-sql-server-flat-file/?source=post_page-----1cb09a7a0883--------------------------------)

我们可以通过以下陈述来总结这一部分：

> 使用 SSMS 导出向导作为从（Azure）SQL Server 数据库提取 CSV 格式数据的工具，并不能保证具有符合 RFC 4180 定义的标准格式，因此提取的信息可能无法被外部系统正确读取。

相反，让我们看看使用 Azure Data Studio 提取 CSV 格式信息时会发生什么。

## 使用 ADS 提取数据

一旦打开 Azure Data Studio，首先要做的是[添加一个新的服务器实例连接](https://learn.microsoft.com/en-us/sql/azure-data-studio/quickstart-sql-server?WT.mc_id=AI-MVP-5003688)。注意，从较新的版本开始，*加密*选项默认设置为*True*。如果你连接到 Azure SQL 数据库，这不会导致连接错误，但如果你的数据源是本地 SQL Server，则可能会产生错误。在这种情况下，你可以将选项设置为*False*。

也就是说，为了在 ADS 中提取表（或视图、查询）的内容，你必须首先执行一个 SELECT 查询，并在运行时将其内容显示在输出网格中。之后，只需按下网格右上角的“保存为 CSV”按钮：

![](img/761cd9020cf9946afb9c7c68b290f0c0.png)

图 8 — 在 ADS 中以 CSV 格式保存查询输出（作者提供）

一个输出文件选择窗口将打开，允许你命名要提取的文件（在我们的例子中是*ExtractTestADS.csv*）。一旦按下*保存*按钮，CSV 文件的内容将直接显示在 ADS 中：

![](img/0f3abdf699119f41e38bb107d72ac01c.png)

图 9 — ADS 以 CSV 格式输出的结果（作者提供）

哇！ADS 生成的输出在所有方面都符合 RFC 4180 标准！因此，ADS 似乎是从（Azure）SQL 数据库中提取 CSV 格式信息的完美工具。

然而，这里存在一个扩展性问题。由于 ADS 要求查询输出首先在输出网格中暴露，这限制了处理大量数据时的功能。在这些情况下，将所有数据包含在一个网格中会占用系统大量内存，导致应用程序崩溃。

因此，我们可以得出以下结论：

> ADS 的 CSV 格式数据导出过程保证了符合 RFC 4180 标准的输出。然而，当待导出的数据集大小相对有限时，使用 ADS 进行提取任务是合适的。当需要提取超过 2–3 GB 的数据时，ADS 可能会占用整个系统内存并导致崩溃。

总的来说，我们可以得出以下结论：

> 不幸的是，微软的数据平台工具提供的*用户友好功能*无法按照 RFC 4180 标准提取大量数据为 CSV 格式。

让我们尝试通过专家用户知道的更具体的工具来实现我们的目标。

# 使用 BCP 工具提取数据

*批量复制程序*（BCP）命令行工具用于将大量新行导入 SQL Server 表中或将数据从表中导出到用户指定格式的数据文件。这是即使在非常大量的数据中也能*尽可能快*地导入或导出数据的解决方案。因此，它在可扩展性方面没有问题。

除了在标准本地 SQL Server 安装时默认安装外，并且除了可以在 Windows 操作系统上[单独安装](https://learn.microsoft.com/en-us/sql/tools/bcp-utility?WT.mc_id=AI-MVP-5003688)之外，BCP 工具还可以通过 Azure 云终端与 Azure SQL 数据库交互，正如这篇博客文章所示：

[导出 CSV 文件从 Azure SQL 数据库](https://kontext.tech/article/763/export-csv-file-from-azure-sql-databases?source=post_page-----1cb09a7a0883--------------------------------) [## Export CSV File from Azure SQL Databases

### 有很多方法可以从 Azure SQL 数据库导出 CSV 文件。本文展示了两种方法：bcp 和 sqlcmd…

[kontext.tech](https://kontext.tech/article/763/export-csv-file-from-azure-sql-databases?source=post_page-----1cb09a7a0883--------------------------------)

不深入细节，BCP 的主要问题是它不能提取表头，也不能以简单的方式处理双引号。这一点通过[*Erland Sommarskog*](https://sessionize.com/ErlandSommarskog/)的参考指南可以证明，该指南报告了获得表头和双引号的多个变通方法，如你所见：

[## 在 SQL Server 中使用批量加载工具

### 由 Erland Sommarskog 编写的 SQL 文本，SQL Server MVP。最近更新时间 2021-01-26。版权适用于此文本。请参见…

[www.sommarskog.se](https://www.sommarskog.se/bulkload.html?source=post_page-----1cb09a7a0883--------------------------------#exportexamples)

这种方法的一个缺点是你必须提前知道哪些字段需要双引号（除非你为所有文本字段提供双引号）。一般来说，我无法提前知道哪些字段可能需要双引号。我只想无忧地提取数据。然而，如果你能够通过 Erland 的建议获取标题和双引号，那么这些引号将应用于所选字段中的所有值。正如 Erland 本人指出的：

> … 假设数据应始终被引号括起来。如果你只想在需要时才加引号，你将需要在查询中处理这个问题，这超出了本文的范围。我能说的就是：祝好运。或者更直接一点：如果可能的话，避免这样做。

此外，如果包含双引号的字段有一个字符串同时包含逗号和双引号，则 BCP 不处理通过双引号转义双引号的特性。

因此，我们可以声明：

> 使用 BCP 以 CSV 格式导出数据，包括头部和双引号，对于非专家用户来说非常复杂。一个缺点是你必须提前知道哪些字段需要提供双引号。此外，它仍然不会产生与 RFC 4180 标准一致的格式。

我不会详细介绍微软的另一个命令行工具[SQLCMD](https://learn.microsoft.com/en-us/sql/tools/sqlcmd-utility?WT.mc_id=AI-MVP-5003688)的使用，因为问题类似于本节中提到的问题。

那么，怎么办呢？如何继续？由于我在互联网上找不到能够以 RFC 4180 兼容的 CSV 格式提取数据并同时处理非常大数据量的应用程序，因此唯一可能的解决方案是开发一个*自定义解决方案*，即使是非专家用户也可以轻松使用。让我们看看这个解决方案是如何工作的。

# 在 PowerShell 中开发自定义解决方案

当我决定为这个问题开发一个具体解决方案时，我首先问自己使用什么编程语言。第一个想到的语言肯定是 Python。然而，我接着想到，标准用户在 Windows 机器上接触自动化的世界时可能不懂 Python，而且操作系统上也不会预装 Python。这就是为什么选择了*PowerShell*，它提供了一个专门用于 SQL Server 的模块。

## SQL Server PowerShell 模块的问题

我第一次尝试使用的是[SQL Server PowerShell 模块](https://learn.microsoft.com/en-us/sql/powershell/download-sql-server-ps-module?WT.mc_id=AI-MVP-5003688)，它允许 SQL Server 开发人员、管理员和商业智能专业人员自动化数据库开发和服务器管理。

具体来说，我尝试使用的命令是[Invoke-Sqlcmd](https://learn.microsoft.com/en-us/powershell/module/sqlserver/invoke-sqlcmd?WT.mc_id=AI-MVP-5003688)，它用于将查询发送到 Azure SQL 数据库以检索数据。这个命令除了调用[sqlcmd.exe 命令行工具](https://learn.microsoft.com/en-us/sql/tools/sqlcmd-utility?WT.mc_id=AI-MVP-5003688)外别无他用，后者常被自动化过程用来从 SQL Server 数据库中检索信息。到目前为止，一切都很好。问题在于 Invoke-Sqlcmd 会将所有查询输出直接存入 PowerShell 数据结构中。正如你可以猜到的那样，当查询输出超过 3-4 GB 时，你会遇到与在 ADS 中提取数据时相同的问题，即由于过度消耗 RAM 系统变得不稳定。

因此，我认为直接在 PowerShell 中使用[ADO.NET 对象](https://learn.microsoft.com/en-us/dotnet/framework/data/adonet/retrieving-and-modifying-data?WT.mc_id=AI-MVP-5003688)是合适的，以尝试绕过这个问题。让我们看看我在这个解决方案中是如何使用它们的。

## 批量导出数据到输出文件

我的解决方案的主要思想是始终使用一个中间数据结构（一个 [*DataTable*](https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable?WT.mc_id=AI-MVP-5003688)），该结构会收集查询数据，但一次只收集一定数量的行。一旦达到中间数据结构的最大容量，它的内容会被写入目标文件，结构会被清空，并立即加载数据源中的下一批数据：

![](img/c3198964bb710faa2e714c6b1f447642.png)

图 10 — 解决方案的主要过程（图片由作者提供）

该过程持续进行，直到数据源中没有新行可读。

你可能会想知道为什么我使用了一个中间的 DataTable，而没有通过 [*StreamWriter*](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter?WT.mc_id=AI-MVP-5003688) 实现直接写入输出文件。答案在于直接使用 PowerShell 的 *Export-Csv* [cmdlet](https://learn.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?WT.mc_id=AI-MVP-5003688) 的能力。

## 使用 Export-Csv 写入数据

我在解决问题时设定的目标之一是，尽量避免重新发明轮子，如果已经有方便的解决方案可以完全或部分解决问题。在这种情况下，我认为可以省略重新编写处理 RFC 4180 标准所提及特殊字符的所有逻辑，直接使用 [*Export-Csv* cmdlet](https://learn.microsoft.com/en-us/previous-versions/powershell/module/microsoft.powershell.utility/export-csv?WT.mc_id=AI-MVP-5003688)。

查阅 PowerShell cmdlet 指南时，我意识到 *Export-Csv* 仅在版本 7 中提供控制双引号使用的参数：

![](img/17eccee714413ecca5c7cd538b60c771.png)

图 11 — Export-Csv 版本 6 和 7 的区别（图片由作者提供）

具体而言，[*UseQuotes 参数*](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-7.3&WT.mc_id=AI-MVP-5003688#-usequotes) 提供了值 *AsNeeded*，其功能定义如下：

> 仅为包含分隔符字符、双引号或换行符的字段加引号

基本上，这正是我们为了满足 RFC 4180 标准要求所需要的。

如果你希望仅为某些字段提供双引号，你可以通过 *QuoteFields* 参数明确指定它们。

目前，PowerShell 版本存在一个小问题。请注意，Windows 10、Windows 11 和 Windows Server 2022 预装了版本 5.1 的 Windows PowerShell（也称为桌面版）。为了使用较新的 *Export-Csv* cmdlet，你必须安装[较新版本的 PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?WT.mc_id=AI-MVP-5003688)（至少 PowerShell 7.0），这实际上是基于 .NET Core 的 Windows PowerShell 的独立软件（如果你有兴趣了解它的演变，你可以在[此链接](https://www.itechtics.com/windows-powershell-vs-powershell-core/)中了解更多）。

强调以下几点很重要：

> 由于该模块是为 PowerShell Core 版本开发的，因此也可以在 Linux 和 macOS 系统上使用。

话虽如此，我们来看看如何使用这个新模块。

# 如何使用 SqlBulkExport 模块

新的 *SqlBulkExport* 模块可以在 GitHub 上找到，链接如下：

[## GitHub - lucazav/sql-bulk-export: 这个 PowerShell 模块包含两个对导出大量数据很有用的函数…](https://github.com/lucazav/sql-bulk-export?source=post_page-----1cb09a7a0883--------------------------------)

### 这个 PowerShell 模块包含两个对导出大量数据非常有用的函数。

[github.com](https://github.com/lucazav/sql-bulk-export?source=post_page-----1cb09a7a0883--------------------------------)

它提供了两个功能：

+   **Export-SqlBulkCsv**：将 SQL Server 数据库表、视图或查询的内容导出到一个符合 RFC 4180 的 CSV 文件中。此功能支持导出大量结果集，将 CSV 文件内容分多次写入。

+   **Export-SqlBulkCsvByPeriod**：将 SQL Server 数据库表、视图或查询的内容导出到多个符合 RFC 4180 的 CSV 文件中，按时间段（按年、按月或按日）进行拆分，基于所选日期字段的内容。此功能支持导出大量结果集，将每个 CSV 文件的内容分多次写入。

两个函数都需要以下参数：

+   *ServerName*：要连接的 SQL Server 实例名称。

+   *Port*：SQL Server 实例端口号。默认值为 1433。

+   *DatabaseName*：要连接的 SQL Server 数据库名称。

+   *SchemaName*：提取数据的表或视图的数据库架构。默认值为“*dbo*”。

+   *TableViewName*：提取数据的数据库表或视图的名称。

+   *Query*：提取数据的 T-SQL 查询。

+   *User*：用于连接数据库的用户名。

+   *Password*：用于连接数据库的用户名的密码。

+   *ConnectionTimeout*：连接超时时间（以秒为单位）。默认值为 30 秒。

+   *DatabaseCulture*：数据库文化代码（例如 it-IT）。它用于正确提取小数分隔符。默认情况下为“*en-US*”。

+   *BatchSize*：写入到输出文件中的批次大小（行数），直到提取的数据结束。

+   *OutputFileFullPath*：输出文件的完整路径（包括文件名和 csv 扩展名）。

+   *SeparatorChar*：用于构建在控制台中显示的字符串分隔符的字符。

*Export-SqlBulkCsvByPeriod* 函数提供了三个更多的必填参数，以便根据时间段对结果集进行分区：

+   *DateColumnName*：按时间段拆分数据的日期/时间类型列。

+   *StartPeriod*：时间段字符串（允许的格式：“*yyyy*”、“*yyyy-MM*”、“*yyyy-MM-dd*”），表示从哪个时间段开始提取数据（包括该时间段）。

+   *EndPeriod*：时间段字符串（允许的格式：“*yyyy*”、“*yyyy-MM*”、“*yyyy-MM-dd*”），表示提取数据的结束时间段（包括该时间段）。

显然，两个输入时间段所使用的格式必须一致。

重要的是要注意，使用 *Export-SqlBulkCsvByPeriod* 函数提取按时间段划分的多个 CSV 文件只能通过表/视图实现，而不能通过查询实现。如果需要选择字段和应用过滤器，必须首先暴露一个具有这些逻辑的视图，然后才能按时间段提取多个 CSV 文件。

此外，*Export-SqlBulkCsvByPeriod* 函数在输出 CSV 文件名中涉及使用字符串标记 `{}`（大括号），该标记将被与 CSV 文件中交易时间段相关的字符串替换。

两个函数会根据是否传递了 User 和 Password 参数自动识别是使用 Windows 身份验证还是 SQL Server 身份验证。

在继续示例之前，请确保您已安装最新版本的 PowerShell。

## 安装最新的 PowerShell 和 SqlBulkExport 版本

为了在 Windows 机器上安装最新版本的 PowerShell，请从 [此链接](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?WT.mc_id=AI-MVP-5003688#installing-the-msi-package) 下载并运行 64 位安装程序（在我们的例子中为 7.3.0 版本）。

点击 *Next* 继续所有的设置向导窗口。然后点击 *Finish*。你会看到 PowerShell 7 (x64) 提示符已安装到你的应用程序中：

![](img/5360576ace50843f971a799953bfa3d0.png)

图 12 — PowerShell 7 刚刚安装完成（图像由作者提供）

运行它，你会看到 PowerShell 提示符准备好接收你的命令：

![](img/dbc29ca3bc3b72081470f642617b7c9b.png)

图 13 — PowerShell 7 提示符准备就绪（图像由作者提供）

你可以输入 `$PSVersionTable` 命令并按 Enter 键检查是否一切正常：

![](img/c4ecfb1c7ad431c5bbc2917190935fb7.png)

图 14 — PSVersionTable 输出（图像由作者提供）

很好！如有必要，您还可以在 [Linux](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-linux?WT.mc_id=AI-MVP-5003688) 或 [macOS](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-macos?WT.mc_id=AI-MVP-5003688) 上安装 PowerShell。

现在，您需要下载 SqlBulkExport 模块文件：

1.  前往 SqlBulkExport GitHub 仓库的 [发布页面](https://github.com/lucazav/sql-bulk-export/releases) 下载最新版本的 *Source code.zip* 文件。

1.  一旦文件保存在您的机器上，解压缩它并将其内容复制到 `C:\Temp` 文件夹中（或您可以选择您喜欢的文件夹）。这样，您的模块文件将被保存在 `C:\Temp\sql-bulk-export-<version>` 文件夹中。

好的！现在您可以尝试一些示例了。

## 将我们的虚拟表内容导出到一个 CSV 文件中

让我们尝试提取本文开始时创建的 *extract_test* 表的内容，以检查其是否符合 RFC 4180 标准。在我们的案例中，相关表被保存在 Azure SQL 数据库中：

1.  打开 PowerShell 7 提示符，输入 `cd C:\Temp\sql-bulk-export-<version>` 命令并按 *Enter* 键，将工作目录更改为模块目录。

1.  输入 `Import-Module -Name ".\SqlBulkExport.psd1"` 命令来导入 *SqlBulkExport* 模块。

1.  输入 `Export-SqlBulkCsv -ServerName "<your-server-name>" -DatabaseName "<your-database-name>" -User "<username>" -Password "<password>" -TableViewName "export_test" -BatchSize 30000 -OutputFileFullPath "C:\Temp\ExtractedTestPS.csv"` 命令，将数据库表（或视图）的内容以每批 30K 行的方式导出到 *ExtractedTestPS.csv* 文件中。以下是输出：

![](img/f9c971ff04c77ac0b4730993bbf3e986.png)

图 15 — 提取虚拟表内容到 CSV 文件的命令控制台输出（图片来源于作者）

这是输出 CSV 文件的内容：

![](img/eeb8565fad9adc7fd92f210ae911f75b.png)

图 16 — 使用 SqlBulkExport 模块提取的 CSV 文件中的虚拟表（图片来源于作者）

如您所见，输出的 CSV 文件内容符合 RFC 4180 标准。由于使用的虚拟表行数较少，因此只使用了一个批次进行提取。现在我们来尝试提取一个拥有几万行的表的内容。

## 将表/视图的内容导出到一个 CSV 文件中

与之前一样，我们要用于提取数据的表也保存在 Azure SQL 数据库中：

1.  打开 PowerShell 7 提示符，输入 `cd C:\Temp\sql-bulk-export-<version>` 命令并按 *Enter* 键，将工作目录更改为模块目录。

1.  输入 `Import-Module -Name ".\SqlBulkExport.psd1"` 命令来导入 *SqlBulkExport* 模块。

1.  输入`Export-SqlBulkCsv -ServerName "<your-server-name>" -DatabaseName "<your-database-name>" -User "<username>" -Password "<password>" -TableViewName "<your-table-or-view-name>" -BatchSize 30000 -OutputFileFullPath "C:\Temp\output.csv"`命令将数据库表（或视图）的内容按 30K 行的批次导出到*output.csv*文件中。以下是输出：

![](img/a5cec7c861005446c9f0d03d8c9c04ad.png)

图 17 — 提取表/视图内容到 CSV 文件的命令的控制台输出（图片由作者提供）

如你所见，提取一个约 74K 行的表内容需要 3 批次的 30K 行，总共花费了 1 秒钟和 88 毫秒。不错！

让我们尝试使用查询来导出数据。

## 将查询输出导出到一个 CSV 文件中

在这种情况下，我们将从与前一个案例相同的表中提取数据，但使用类似`SELECT * FROM <table> WHERE <condition>`的查询。

1.  打开 PowerShell 7 提示符，输入`cd C:\Temp\sql-bulk-export-<version>`命令并按*Enter*键，将工作目录更改为模块目录。

1.  输入`Import-Module -Name ".\SqlBulkExport.psd1"`命令以导入*SqlBulkExport*模块。

1.  输入`Export-SqlBulkCsv -ServerName "<your-server-name>" -DatabaseName "<your-database-name>" -User "<username>" -Password "<password>" -Query "SELECT * FROM <your-table-or-view-name> WHERE <condition>" -BatchSize 30000 -OutputFileFullPath "C:\Temp\output.csv"`命令，将查询结果集的内容按 30K 行的批次导出到*output.csv*文件中。以下是输出：

![](img/d49bc259f0d72c2e9eb8eef2e6d11347.png)

图 18 — 提取查询输出到 CSV 文件的命令的控制台输出（图片由作者提供）

一切运行得非常顺利！现在让我们尝试将一个视图的内容导出到多个每月的 CSV 文件中。

## 将表/视图的内容导出到多个每月的 CSV 文件中

想象一下你有一个每月包含数十万行的交易表。有一组来自公司外部的数据科学家被分配来对交易历史进行高级分析。为了方便，他们要求你提取一个数据集，其中包含表中字段的子集，涵盖几个月的交易记录。他们要求你提供多个按月份划分的 CSV 文件，而不是生成一个单一的 CSV 文件。

让我们看看如何通过*Export-SqlBulkCsvByPeriod*函数做到这一点：

1.  打开 PowerShell 7 提示符，输入`cd C:\Temp\sql-bulk-export-<version>`命令并按*Enter*键，将工作目录更改为模块目录。

1.  输入`Import-Module -Name ".\SqlBulkExport.psd1"`命令以导入*SqlBulkExport*模块。

1.  输入 `Export-SqlBulkCsvByPeriod -ServerName "<your-server-name>" -DatabaseName "<your-database-name>" -User "<username>" -Password "<password>" -TableViewName "<your-table-name>" -DateColumnName "<your-date-column-name>" -StartPeriod "2022-01" -EndPeriod "2022-03" -BatchSize 100000 -OutputFileFullPath "C:\Temp\output_{}.csv"` 命令将数据库表（或视图）的内容导出为多个按月的 CSV 文件，每批 10 万行，从 2022 年 1 月到 2022 年 3 月。以下是输出：

![](img/3a3b9613b382db4191d13ffd0b515f40.png)

图 19 — 提取多个按月 CSV 文件的命令控制台输出（图片由作者提供）

太棒了！你刚刚在 1 分钟 19 秒内提取了大约 150 万行数据，并将其分解为三个按月的 CSV 文件！

# 结论

促使我撰写这篇文章的需求是将大量数据（3–4+ GB）提取到一个或多个符合 RFC 4180 标准的 CSV 格式文件中。

你已经看到微软提供的工具（无论是 IDE，例如 SSMS 和 ADS；还是命令行工具，例如 BCP）无法满足上述需求。唯一看起来稍微合适的工具是 ADS，但它无法在不崩溃的情况下提取大量数据。不客气地说，到目前为止微软还没有提供符合上述要求的工具，这实在有些令人尴尬。

由于未能在互联网上找到满足上述需求的软件，我编写了*SqlBulkExport* PowerShell 模块来解决这个问题，并将其以 MIT 许可证开源[在 GitHub](https://github.com/lucazav/sql-bulk-export)上。我强调我并不是 PowerShell 开发人员，所以任何能改进此解决方案的建议都非常欢迎！
