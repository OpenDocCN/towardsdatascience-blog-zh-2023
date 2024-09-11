# 使用 Fugue 和 Python 简化 BigQuery 上的数据科学工作流

> 原文：[https://towardsdatascience.com/simplify-data-science-workflows-on-bigquery-with-fugue-and-python-5215a1b65e43?source=collection_archive---------9-----------------------#2023-04-13](https://towardsdatascience.com/simplify-data-science-workflows-on-bigquery-with-fugue-and-python-5215a1b65e43?source=collection_archive---------9-----------------------#2023-04-13)

## 加速迭代并降低计算成本

[](https://khuyentran1476.medium.com/?source=post_page-----5215a1b65e43--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----5215a1b65e43--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5215a1b65e43--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5215a1b65e43--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----5215a1b65e43--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-science-workflows-on-bigquery-with-fugue-and-python-5215a1b65e43&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----5215a1b65e43---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5215a1b65e43--------------------------------) ·6 min read·2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5215a1b65e43&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-science-workflows-on-bigquery-with-fugue-and-python-5215a1b65e43&user=Khuyen+Tran&userId=84a02493194a&source=-----5215a1b65e43---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5215a1b65e43&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-science-workflows-on-bigquery-with-fugue-and-python-5215a1b65e43&source=-----5215a1b65e43---------------------bookmark_footer-----------)

# 动机

许多数据团队开始时会在如 BigQuery 这样的数据仓库上建立分析实践。然而，由于各种原因，仅依赖 BigQuery 进行数据科学工作负载可能不是最佳方法：

+   **超越 SQL 的高级需求**：数据验证、可视化和机器学习预测等用例可能需要超出 SQL 语法限制的更高级功能。

+   **探索成本高**：由于 BigQuery 的迭代性质（涉及大量特征工程和算法实验），它可能不是进行数据科学任务的最具成本效益的解决方案。

对于在 BigQuery 上处理数据的数据科学家来说，理想的解决方案应能使他们：

+   使用 SQL 和 Python 查询来自 BigQuery 的数据。

+   交互式地在本地测试各种 SQL 查询

+   在彻底测试后，轻松切换回 BigQuery。

![](../Images/5885ddbb9506439aefb9f64884f16214.png)

作者提供的图像

FugueSQL BigQuery 集成允许你做到这一点。

# 什么是 Fugue？
