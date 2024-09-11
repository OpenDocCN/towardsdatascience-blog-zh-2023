# 使用 Scikit-Learn 的 SGDRegressor：你需要知道的未教会的课程

> 原文：[`towardsdatascience.com/sgdregressor-with-scikit-learn-untaught-lessons-you-need-to-know-cf2430439689?source=collection_archive---------6-----------------------#2023-03-08`](https://towardsdatascience.com/sgdregressor-with-scikit-learn-untaught-lessons-you-need-to-know-cf2430439689?source=collection_archive---------6-----------------------#2023-03-08)

## 揭示隐藏的算法关系通过一个令人困惑的名称

[](https://medium.com/@angela.shi?source=post_page-----cf2430439689--------------------------------)![安吉拉和柯展·史](https://medium.com/@angela.shi?source=post_page-----cf2430439689--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cf2430439689--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cf2430439689--------------------------------) [安吉拉和柯展·史](https://medium.com/@angela.shi?source=post_page-----cf2430439689--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsgdregressor-with-scikit-learn-untaught-lessons-you-need-to-know-cf2430439689&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----cf2430439689---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cf2430439689--------------------------------) ·13 min 阅读·2023 年 3 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcf2430439689&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsgdregressor-with-scikit-learn-untaught-lessons-you-need-to-know-cf2430439689&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----cf2430439689---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcf2430439689&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsgdregressor-with-scikit-learn-untaught-lessons-you-need-to-know-cf2430439689&source=-----cf2430439689---------------------bookmark_footer-----------)

在机器学习领域，线性模型是一种基础技术，广泛用于根据输入数据预测数值。来自 scikit-learn 的 SGDRegressor 估算器是一个强大的工具，可以让机器学习从业者快速高效地执行线性回归。

然而，SGDRegressor 这个名字对初学者来说可能有点混淆。本文将解释它的工作原理，并探讨为何这个名字可能会对刚刚开始学习机器学习的初学者产生误导。

此外，我们还将探讨 SGDRegressor 中实际上存在多个模型的观点，每个模型都有自己特定的参数和超参数。这将引发关于机器学习中模型定义的有趣问题。例如，岭回归或支持向量回归（SVR）是一个独立的模型，还是只是一个调整后的线性模型？

当你读完这篇文章时，你将更好地理解 SGDRegressor 的内部工作原理，并对线性模型在机器学习中的复杂性和其实简单性有新的认识。

# 1\. 通常教授的 SGDRegressor 课程
