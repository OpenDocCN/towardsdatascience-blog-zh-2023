# Neo4j 的电影推荐

> 原文：[https://towardsdatascience.com/movie-recommendations-with-neo4j-adaad7c9bf2b?source=collection_archive---------2-----------------------#2023-02-19](https://towardsdatascience.com/movie-recommendations-with-neo4j-adaad7c9bf2b?source=collection_archive---------2-----------------------#2023-02-19)

## 使用 Python 和 Neo4j 构建一个简单的电影推荐系统

[](https://dpanagop-53386.medium.com/?source=post_page-----adaad7c9bf2b--------------------------------)[![Dimitris Panagopoulos](../Images/437f218b1a27ed01a98270817c76729f.png)](https://dpanagop-53386.medium.com/?source=post_page-----adaad7c9bf2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----adaad7c9bf2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----adaad7c9bf2b--------------------------------) [Dimitris Panagopoulos](https://dpanagop-53386.medium.com/?source=post_page-----adaad7c9bf2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92599bd5527c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmovie-recommendations-with-neo4j-adaad7c9bf2b&user=Dimitris+Panagopoulos&userId=92599bd5527c&source=post_page-92599bd5527c----adaad7c9bf2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----adaad7c9bf2b--------------------------------) ·7分钟阅读·2023年2月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fadaad7c9bf2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmovie-recommendations-with-neo4j-adaad7c9bf2b&user=Dimitris+Panagopoulos&userId=92599bd5527c&source=-----adaad7c9bf2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fadaad7c9bf2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmovie-recommendations-with-neo4j-adaad7c9bf2b&source=-----adaad7c9bf2b---------------------bookmark_footer-----------)![](../Images/924eb1da869fd77b718b1eaa3ffc9e78.png)

图像由作者使用稳定扩散技术和[https://bytexd.com/get-started-with-stable-diffusion-google-colab-for-ai-generated-art/](https://bytexd.com/get-started-with-stable-diffusion-google-colab-for-ai-generated-art/)中的代码创建

# **介绍**

创建推荐系统是机器学习的常见应用场景。在这篇文章中，我们将演示如何使用图数据库创建一个简单的电影推荐系统。虽然所提方法并不是最先进的，但使用图数据库既易于实现又易于解释。它们可以作为一个简单推荐系统的起点，用于快速提供结果和/或作为评估更复杂系统的基线。

如果读者希望进行实验，可以使用 [Neo4j 的沙箱环境](https://neo4j.com/sandbox/) 和 [Google 的 Colab](https://colab.research.google.com/) 在短短一两分钟内准备好系统。本文将使用来自 [GroupLens.org](https://grouplens.org/datasets/movielens/) 的数据（即“[1M 数据集](https://files.grouplens.org/datasets/movielens/ml-1m.zip)”）。我们还将使用一个小数据集来创建一个仅包含少数节点的最小图，以便能够轻松检查计算。所有用于最小图的代码和数据可以在作者的 [GitHub](https://github.com/dpanagop/data_analytics_examples/tree/master/neo4j_recommender) 中找到。

请注意：

1.  Neo4j 图数据科学插件应安装在 Neo4j 中（它已在 Neo4j 的沙箱环境中预安装）。

1.  在 Python 中，应安装“neo4j-driver”和“graphdatascience”库。
