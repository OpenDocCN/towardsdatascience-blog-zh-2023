# 为什么回测很重要以及如何正确进行回测

> 原文：[https://towardsdatascience.com/why-backtesting-matters-and-how-to-do-it-right-731fb9624a?source=collection_archive---------8-----------------------#2023-07-18](https://towardsdatascience.com/why-backtesting-matters-and-how-to-do-it-right-731fb9624a?source=collection_archive---------8-----------------------#2023-07-18)

## 我们如何知道我们的预测模型是否准确可靠，并评估其在未见数据上的表现？这就是回测的作用所在。

[](https://medium.com/@davide.burba?source=post_page-----731fb9624a--------------------------------)[![Davide Burba](../Images/a1ca3cf59c2b933021fa0d978e1af522.png)](https://medium.com/@davide.burba?source=post_page-----731fb9624a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----731fb9624a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----731fb9624a--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----731fb9624a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f58aaaeaed7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-backtesting-matters-and-how-to-do-it-right-731fb9624a&user=Davide+Burba&userId=9f58aaaeaed7&source=post_page-9f58aaaeaed7----731fb9624a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----731fb9624a--------------------------------) · 7 分钟阅读 · 2023年7月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F731fb9624a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-backtesting-matters-and-how-to-do-it-right-731fb9624a&user=Davide+Burba&userId=9f58aaaeaed7&source=-----731fb9624a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F731fb9624a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-backtesting-matters-and-how-to-do-it-right-731fb9624a&source=-----731fb9624a---------------------bookmark_footer-----------)![](../Images/a56e7682ea4c901ab7c808da2f123238.png)

“回测”，由 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/). 经许可使用。

+   [什么是回测？](#0326)

+   [Python 示例：航空乘客](#9788)

+   [结论](#0172)

# 什么是回测？

为了评估预测模型的性能，我们使用一种称为回测的程序（也称为**时间序列交叉验证**）。回测本质上是一种测试模型如果在过去使用会表现如何的方法。

***它是如何工作的？***

要对时间序列预测模型进行回测，我们首先将数据分成两部分：训练集和验证集（有时也称为测试集，不过我们将在接下来的章节中澄清它们之间的区别）。训练集用于训练模型，而测试集用于评估模型在未见数据上的表现。一旦模型训练完成，你可以用它对测试集进行预测。你可以将这些预测与实际值进行比较，以查看模型的表现如何。
