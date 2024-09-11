# 监控生产中机器学习模型的难题

> 原文：[`towardsdatascience.com/the-difficulties-of-monitoring-machine-learning-models-in-production-ed3f87e88253?source=collection_archive---------14-----------------------#2023-03-14`](https://towardsdatascience.com/the-difficulties-of-monitoring-machine-learning-models-in-production-ed3f87e88253?source=collection_archive---------14-----------------------#2023-03-14)

[](https://maciejbalawejder.medium.com/?source=post_page-----ed3f87e88253--------------------------------)![Maciej Balawejder](https://maciejbalawejder.medium.com/?source=post_page-----ed3f87e88253--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ed3f87e88253--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed3f87e88253--------------------------------) [Maciej Balawejder](https://maciejbalawejder.medium.com/?source=post_page-----ed3f87e88253--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F20c0457a86b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-difficulties-of-monitoring-machine-learning-models-in-production-ed3f87e88253&user=Maciej+Balawejder&userId=20c0457a86b0&source=post_page-20c0457a86b0----ed3f87e88253---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed3f87e88253--------------------------------) ·7 分钟阅读·2023 年 3 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fed3f87e88253&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-difficulties-of-monitoring-machine-learning-models-in-production-ed3f87e88253&user=Maciej+Balawejder&userId=20c0457a86b0&source=-----ed3f87e88253---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fed3f87e88253&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-difficulties-of-monitoring-machine-learning-models-in-production-ed3f87e88253&source=-----ed3f87e88253---------------------bookmark_footer-----------)![](img/0c292c56c7b178d571dd34d1cc4ec2e1.png)

[照片由 [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)

成为数据科学家听起来可能是一个简单的工作——准备数据，训练模型，并在生产环境中部署。然而，现实远非如此简单。这项工作更像是照顾一个婴儿——一个永无止境的监控循环，确保一切正常。

挑战在于需要关注三个关键组件：代码、数据和模型本身。每个元素都呈现出自己的一系列难题，使得在生产中监控变得困难。

在这篇文章中，我们将深入探讨这些挑战，并提及一些应对方法。

# 模型可能失败的两种方式

## ***模型无法进行预测***

![](img/67de4312e15f610cd78c109b660fb386.png)

代码中的错误示例。图片由作者提供。图片由作者提供。

当我们谈论模型无法进行预测时，意味着它不能生成输出。由于模型总是一个更大软件系统的一部分，因此它也面临更多技术挑战。以下是一些可能的例子：

+   **语言障碍**——将用一种编程语言构建的模型集成到用另一种编程语言编写的系统中。它…
