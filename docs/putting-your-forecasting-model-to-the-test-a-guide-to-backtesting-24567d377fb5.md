# 将你的预测模型进行测试：回测指南

> 原文：[`towardsdatascience.com/putting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5?source=collection_archive---------5-----------------------#2023-11-08`](https://towardsdatascience.com/putting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5?source=collection_archive---------5-----------------------#2023-11-08)

![](img/d92886eef96d6f3b904a3be033fdc241.png)

使用 Midjourney 生成的图像

## 学习如何通过回测正确评估时间序列模型的性能

[](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)![Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----24567d377fb5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------) ·9 分钟阅读·2023 年 11 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F24567d377fb5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----24567d377fb5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F24567d377fb5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5&source=-----24567d377fb5---------------------bookmark_footer-----------)

评估时间序列模型不是一项简单的任务。实际上，在评估预测模型时很容易犯下严重的错误。尽管这些错误可能不会破坏代码或阻止我们获取一些输出数字，但它们可能会显著影响这些性能估计的准确性。

在本文中，我们将展示如何正确评估时间序列模型。

# 为什么标准的机器学习方法不适用于时间序列？

评估机器学习模型性能最简单的方法是将数据集分成两个子集：训练集和测试集。为了进一步提高我们性能估计的稳健性，我们可能需要多次拆分数据集。这个过程叫做交叉验证。

下图表示了最流行的交叉验证类型之一——k 折验证方法。在 5 折验证的情况下，我们首先将数据集分成 5 个部分。然后，我们使用这 5 个部分中的 4 个来训练模型，并在第 5 个部分上评估其性能。这个过程会重复…
