# 从（Azure）SQL Server 巨大表中提取数据到符合 RFC 4180 标准的 CSV 文件

> 原文：[`towardsdatascience.com/extracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883?source=collection_archive---------8-----------------------#2023-01-05`](https://towardsdatascience.com/extracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883?source=collection_archive---------8-----------------------#2023-01-05)

## 从（Azure）SQL Server 巨大表中提取包含特殊字符的字符串数据到 CSV 文件中的噩梦

[](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)![Luca Zavarella](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------) [Luca Zavarella](https://lucazavarella.medium.com/?source=post_page-----1cb09a7a0883--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc00872c87df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883&user=Luca+Zavarella&userId=c00872c87df&source=post_page-c00872c87df----1cb09a7a0883---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb09a7a0883--------------------------------) · 21 分钟阅读 · 2023 年 1 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cb09a7a0883&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883&user=Luca+Zavarella&userId=c00872c87df&source=-----1cb09a7a0883---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cb09a7a0883&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-data-from-azure-sql-server-huge-tables-in-rfc-4180-compliant-csv-files-1cb09a7a0883&source=-----1cb09a7a0883---------------------bookmark_footer-----------)![](img/623f3bda214162d325d7158441329fe3.png)

（图片来自 Unsplash）

当来自公司外部的数据科学团队被聘请来实施机器学习模型时，你需要与他们分享用于模型训练的数据。如果上述团队无法直接访问数据库中的数据，第一种选择是将数据从数据库提取到 CSV 格式的文件中。考虑到这些数据通常量大（5+ GB）并且某些字段可能包含特殊字符（逗号，这与字段分隔符相同；回车和/或换行符），非开发用户使用的导出工具可能不够合适，甚至可能导致内存问题。

在这篇文章中，你将学习如何使用 PowerShell 函数从（Azure）SQL Server 数据库中提取包含特殊字符的大量数据，并将其保存为符合 RFC 4180 的 CSV 文件。

当你需要从（Azure）SQL Server 数据库中提取数据时，用户首先想到的工具是 [*SQL Server Management Studio*](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?WT.mc_id=AI-MVP-5003688)（SSMS）和 *Azure Data*…。
