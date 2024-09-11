# 本地与全球预测：你需要知道的事项

> 原文：[https://towardsdatascience.com/local-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0?source=collection_archive---------10-----------------------#2023-05-02](https://towardsdatascience.com/local-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0?source=collection_archive---------10-----------------------#2023-05-02)

## 本文比较了本地与全球时间序列预测的方法，并通过使用 LightGBM 和澳大利亚旅游数据集的 Python 演示进行了说明。

[](https://medium.com/@davide.burba?source=post_page-----1cc29e66cae0--------------------------------)[![Davide Burba](../Images/a1ca3cf59c2b933021fa0d978e1af522.png)](https://medium.com/@davide.burba?source=post_page-----1cc29e66cae0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cc29e66cae0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1cc29e66cae0--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----1cc29e66cae0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f58aaaeaed7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0&user=Davide+Burba&userId=9f58aaaeaed7&source=post_page-9f58aaaeaed7----1cc29e66cae0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cc29e66cae0--------------------------------) · 9 分钟阅读 · 2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cc29e66cae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0&user=Davide+Burba&userId=9f58aaaeaed7&source=-----1cc29e66cae0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cc29e66cae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0&source=-----1cc29e66cae0---------------------bookmark_footer-----------)![](../Images/4b226d455de07e4891301fc3a307c7a3.png)

本地与全球预测，由 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/)。经许可使用。

+   [什么是本地预测？](#9be8)

+   [什么是全球预测？](#d102)

+   [如何在本地预测与全球预测之间做出选择？](#65b1)

+   [Python 示例：澳大利亚旅游](#4f95)

+   [结论](#99a0)

## ***什么是本地预测？***

本地预测是传统方法，我们**为每个时间序列独立训练一个预测模型**。经典的统计模型（如指数平滑、ARIMA、TBATS等）通常使用这种方法，但标准机器学习模型也可以通过特征工程步骤使用它。

本地预测具有**优点**：

+   这是直观易懂且易于实施的。

+   每个模型可以单独调整。

但它也有一些**限制**：

+   它存在“冷启动”问题：对于每个时间序列，需要相对大量的历史数据来可靠地估计模型参数。这也使得它…
