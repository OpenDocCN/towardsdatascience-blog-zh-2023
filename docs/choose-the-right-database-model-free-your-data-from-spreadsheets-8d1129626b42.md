# 选择正确的数据库模型 & 从电子表格中解放你的数据

> 原文：[https://towardsdatascience.com/choose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42?source=collection_archive---------13-----------------------#2023-05-08](https://towardsdatascience.com/choose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42?source=collection_archive---------13-----------------------#2023-05-08)

## 你已超越了 Excel：如何在关系型数据库、文档数据库或图形数据库之间进行选择，并为下一步做好准备

[](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)[![Kaleb Nyquist](../Images/33005821cae7a73f536871e0b3c7545c.png)](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------) [Kaleb Nyquist](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F495932917d8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchoose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42&user=Kaleb+Nyquist&userId=495932917d8c&source=post_page-495932917d8c----8d1129626b42---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------) ·16 分钟阅读·2023年5月8日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8d1129626b42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchoose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42&user=Kaleb+Nyquist&userId=495932917d8c&source=-----8d1129626b42---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8d1129626b42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchoose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42&source=-----8d1129626b42---------------------bookmark_footer-----------)

打开一个空白电子表格，似乎无尽的行列可以看作是一个无限可能的画布。然而，许多数据工程师和其他数字知识工作者越来越觉得电子表格交错的灰线是有限制的——具有讽刺意味的是，这不不同于监狱牢房中的水平和垂直金属栏杆！

![](../Images/4f4481988d97ecf7f9711a1cd7d4de3a.png)

**诚然，为了让“电子表格监狱”的视觉隐喻生效，电子表格需要旋转90°。但一旦你看到这种诡异的相似性，就会变得难以忘记。** *照片由作者插图。* [*照片*](https://unsplash.com/photos/JC7bE-eQXIk) *来自* [*WWW PROD*](https://unsplash.com/@wwwprod) *在* [*Unsplash*](https://unsplash.com/photos/JC7bE-eQXIk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*。*

“电子表格监狱”发生在当将数据存储在电子表格中的决策阻碍了组织高效实现其目标时。这主要是因为电子表格的单个单元格（无意冒犯）在查询和管理能力上受到限制：例如，今天单元格`K18`可能指的是某个物品的库存量，但如果明天添加了行和列，`K18`可能完全指代其他内容。

对于更大的项目，当存储的数据量超出电子表格的最大尺寸（Google Sheets为1000万单元格；Microsoft Excel为1,048,576行和16,384列）时，也会出现“电子表格监狱”。在一个极端的例子中，发现一个财务数据列表被...
