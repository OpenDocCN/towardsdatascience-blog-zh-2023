# 使用蒙特卡罗模拟传播误差的力量和简单性

> 原文：[`towardsdatascience.com/the-power-and-simplicity-of-propagating-errors-with-monte-carlo-simulations-9c8dcca9d90d?source=collection_archive---------20-----------------------#2023-07-21`](https://towardsdatascience.com/the-power-and-simplicity-of-propagating-errors-with-monte-carlo-simulations-9c8dcca9d90d?source=collection_archive---------20-----------------------#2023-07-21)

## 精通数据分析和模型拟合中的不确定性，提供实际的代码和示例

![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----9c8dcca9d90d-----------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c8dcca9d90d-----------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----9c8dcca9d90d-----------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-and-simplicity-of-propagating-errors-with-monte-carlo-simulations-9c8dcca9d90d&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----9c8dcca9d90d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c8dcca9d90d--------------------------------) · 15 分钟阅读 · 2023 年 7 月 21 日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c8dcca9d90d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-and-simplicity-of-propagating-errors-with-monte-carlo-simulations-9c8dcca9d90d&source=-----9c8dcca9d90d---------------------bookmark_footer-----------)![](img/f9b4b497c8b238de0f45eba099ff0d5d.png)

图片由 [eskay lim](https://unsplash.com/es/@eskaylim?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在*Towards Data Science*上有[很多关于蒙特卡洛方法](https://medium.com/search?q=monte+carlo+towards+data+science)的一般信息，但关于其在误差传播中的重要且有用的应用实际上不多，除了[Shuai Guo](https://medium.com/u/7b08bf52bf9c?source=post_page-----9c8dcca9d90d--------------------------------)的优秀介绍和一些其他文章：

[## 使用蒙特卡洛方法量化模型预测误差](https://towardsdatascience.com/how-to-quantify-the-prediction-error-made-by-my-model-db4705910173?source=post_page-----9c8dcca9d90d--------------------------------)

### 蒙特卡洛模拟展示了

[关于如何量化模型预测误差](https://towardsdatascience.com/how-to-quantify-the-prediction-error-made-by-my-model-db4705910173?source=post_page-----9c8dcca9d90d--------------------------------)

在这里，我想提出一些具体的数值应用及代码，让你可以亲自尝试并感受蒙特卡洛方法如何在几乎任何计算中极其有用且易于实现的误差传播。

我将从一个非常简单的示例开始，演示在减法操作中传播误差，然后举例说明如何利用基本相同的思路来传播几乎任何类型数值过程中的误差，从简单的线性回归到非常复杂的拟合过程，这些过程用解析方法处理会非常困难。
