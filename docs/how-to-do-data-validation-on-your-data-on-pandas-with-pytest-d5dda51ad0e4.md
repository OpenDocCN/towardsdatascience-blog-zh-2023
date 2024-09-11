# 如何使用pytest对Pandas中的数据进行数据验证

> 原文：[https://towardsdatascience.com/how-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4?source=collection_archive---------2-----------------------#2023-05-26](https://towardsdatascience.com/how-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4?source=collection_archive---------2-----------------------#2023-05-26)

## 数据

## 使用Python对处理后的DataFrames进行基本数据验证

[](https://byrondolon.medium.com/?source=post_page-----d5dda51ad0e4--------------------------------)[![Byron Dolon](../Images/9ff32138c7b1913be24cc7ab971752b0.png)](https://byrondolon.medium.com/?source=post_page-----d5dda51ad0e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5dda51ad0e4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d5dda51ad0e4--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----d5dda51ad0e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----d5dda51ad0e4---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5dda51ad0e4--------------------------------) ·10 min read·2023年5月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5dda51ad0e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4&user=Byron+Dolon&userId=6b5d063df5dd&source=-----d5dda51ad0e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5dda51ad0e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-data-validation-on-your-data-on-pandas-with-pytest-d5dda51ad0e4&source=-----d5dda51ad0e4---------------------bookmark_footer-----------)![](../Images/e0ef97c2f7836a1f5e61f73ce2556434.png)

图片经我才华横溢的妹妹 [ohmintyartz](https://www.instagram.com/ohmintyartz/) 使用许可

在大规模数据处理与机器学习中是令人兴奋的，但在开始考虑训练模型之前，有一个重要步骤你不应忘记：**数据验证**。

数据验证指的是验证你收集和转换的数据是否正确且可用。你应始终确保最终用于机器学习模型或数据分析的数据符合你的预期。

这个过程可能涉及从检查你的原始数据是否符合预期（例如，数据源定义输出的方式）开始，到检查数据处理函数是否按预期工作。

我们可以探讨如何在使用**pytest**的Pandas数据处理管道中实现基本的测试和数据验证。我们将从查看一组初始的数据处理函数开始，然后探讨如何实现一些测试，以确保我们的处理函数和数据按预期运行。

你可以在自己的笔记本或IDE中跟随操作。你可以从Kaggle [这里](https://www.kaggle.com/datasets/deepanshuverma0154/sales-dataset-of-ecommerce-electronic-products?resource=download) 下载数据集。
