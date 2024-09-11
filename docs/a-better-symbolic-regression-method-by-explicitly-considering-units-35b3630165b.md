# 更好的符号回归方法，通过明确考虑单位

> 原文：[`towardsdatascience.com/a-better-symbolic-regression-method-by-explicitly-considering-units-35b3630165b?source=collection_archive---------12-----------------------#2023-03-16`](https://towardsdatascience.com/a-better-symbolic-regression-method-by-explicitly-considering-units-35b3630165b?source=collection_archive---------12-----------------------#2023-03-16)

## 可能有助于将机器学习与传统科学和工程方法融合，通过提供更具解释性的模型，根植于基本概念。

[](https://lucianosphere.medium.com/?source=post_page-----35b3630165b--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----35b3630165b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----35b3630165b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----35b3630165b--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----35b3630165b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-symbolic-regression-method-by-explicitly-considering-units-35b3630165b&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----35b3630165b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----35b3630165b--------------------------------) · 7 分钟阅读 · 2023 年 3 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F35b3630165b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-symbolic-regression-method-by-explicitly-considering-units-35b3630165b&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----35b3630165b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F35b3630165b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-symbolic-regression-method-by-explicitly-considering-units-35b3630165b&source=-----35b3630165b---------------------bookmark_footer-----------)![](img/948d15e8e2e9706e5b347d1ff8c707b0.png)

由作者通过结合不同的 Dall-E-2 生成图像创建的图示。

**符号回归是一种技术，它通过寻找描述数据之间关系的数学方程来帮助我们理解不同数据片段之间的关系。我对符号回归方法非常期待并提倡它们，因为通过提供明确的方程，它们原则上是高度可解释的，以直观的方式展示——与大多数现代 AI 模型不同，后者像黑箱一样我们无法理解，使得了解它们的工作原理和原因变得困难。**

**Tenachi 等人提出了一项新工作，该工作已作为预印本发布在 arXiv 上，展示了一种新方法，该方法利用深度强化学习来寻找数据集中变量的方程，同时考虑到数据相关的单位。这种方法有助于排除物理上不可能的解，并通过限制方程生成器的自由度来提高性能。**

## 索引

**-** **介绍** **-** **关于符号回归及引入新方法** **-** **如何**…
