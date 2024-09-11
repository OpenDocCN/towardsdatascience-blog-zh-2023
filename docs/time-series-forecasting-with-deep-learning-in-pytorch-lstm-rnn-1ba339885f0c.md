# 使用 PyTorch (LSTM-RNN) 进行时间序列预测

> 原文：[`towardsdatascience.com/time-series-forecasting-with-deep-learning-in-pytorch-lstm-rnn-1ba339885f0c?source=collection_archive---------0-----------------------#2023-02-09`](https://towardsdatascience.com/time-series-forecasting-with-deep-learning-in-pytorch-lstm-rnn-1ba339885f0c?source=collection_archive---------0-----------------------#2023-02-09)

## 一篇关于使用 PyTorch 深度学习进行单变量时间序列预测的深入教程

[](https://zainbaq.medium.com/?source=post_page-----1ba339885f0c--------------------------------)![Zain Baquar](https://zainbaq.medium.com/?source=post_page-----1ba339885f0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ba339885f0c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ba339885f0c--------------------------------) [Zain Baquar](https://zainbaq.medium.com/?source=post_page-----1ba339885f0c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd16fc4a70186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-deep-learning-in-pytorch-lstm-rnn-1ba339885f0c&user=Zain+Baquar&userId=d16fc4a70186&source=post_page-d16fc4a70186----1ba339885f0c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ba339885f0c--------------------------------) ·12 min read·2023 年 2 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ba339885f0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-deep-learning-in-pytorch-lstm-rnn-1ba339885f0c&user=Zain+Baquar&userId=d16fc4a70186&source=-----1ba339885f0c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ba339885f0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-deep-learning-in-pytorch-lstm-rnn-1ba339885f0c&source=-----1ba339885f0c---------------------bookmark_footer-----------)![](img/d2d7047e533c03e443b61973f43ce21a.png)

[Unsplash: Maxim Hopman](https://unsplash.com/@nampoh)

# **介绍**

信不信由你，人类不断在被动地预测事物——即使是最微小或看似琐碎的事情。当过马路时，我们预测汽车的位置以安全过马路，或者我们试图预测当我们试图抓住球时，球会落在哪里。我们不需要知道汽车的确切速度或影响球的精确风向来完成这些任务——这些能力对我们来说或多或少是自然和显而易见的。这些能力通过一些事件进行调整，这些事件经过多年的经验和实践使我们能够应对我们所生活的不可预测的现实。在这方面我们失败的地方是，当我们在主动预测一个大规模现象，如天气或一年后经济表现时，考虑的因素实在太多。

这就是计算的力量所在——弥补我们无法将即使是最看似随机的事件与未来事件联系起来的不足。众所周知，计算机在进行特定任务的多个迭代中表现得非常出色……
