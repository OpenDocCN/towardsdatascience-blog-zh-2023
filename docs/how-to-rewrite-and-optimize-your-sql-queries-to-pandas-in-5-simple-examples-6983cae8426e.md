# 如何将 SQL 查询重写和优化为 Pandas：5 个简单示例

> 原文：[`towardsdatascience.com/how-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e?source=collection_archive---------2-----------------------#2023-06-01`](https://towardsdatascience.com/how-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e?source=collection_archive---------2-----------------------#2023-06-01)

## 编程

## 从 SQL 迁移到 Pandas，以改善数据分析工作流程

[](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)![Byron Dolon](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----6983cae8426e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------) ·9 分钟阅读·2023 年 6 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6983cae8426e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e&user=Byron+Dolon&userId=6b5d063df5dd&source=-----6983cae8426e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6983cae8426e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e&source=-----6983cae8426e---------------------bookmark_footer-----------)![](img/802af72a42f8761f0a4ee55bcb3d5954.png)

图片由作者提供

数据分析师、工程师和科学家都对 SQL 熟悉。这个查询语言在处理任何类型的关系数据库时仍然被广泛使用。

然而，如今尤其是对于数据分析师，技术要求越来越高，人们预计至少要掌握一种编程语言的基础知识。在处理数据时，Python 和 Pandas 特别是常见的职位要求附加项。

尽管 Pandas 对于熟悉 SQL 的数据人员来说可能是新鲜的，但 SQL 中选择、过滤和汇总数据的概念可以很容易地转移到 Pandas 中。在这篇文章中，我们来看看一些常见的 SQL 查询以及如何在 Pandas 中编写和优化它们。

随意在你自己的笔记本或 IDE 中跟随操作。你可以从 Kaggle [这里](https://www.kaggle.com/datasets/deepanshuverma0154/sales-dataset-of-ecommerce-electronic-products?resource=download) 下载数据集，CC0 1.0 通用（CC0 1.0）公共领域奉献许可下免费使用。

只需导入并运行以下代码，我们就可以开始了！
