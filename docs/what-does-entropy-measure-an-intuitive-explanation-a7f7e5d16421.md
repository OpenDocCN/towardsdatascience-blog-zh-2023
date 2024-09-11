# 熵度量什么？一个直观的解释

> 原文：[https://towardsdatascience.com/what-does-entropy-measure-an-intuitive-explanation-a7f7e5d16421?source=collection_archive---------1-----------------------#2023-01-04](https://towardsdatascience.com/what-does-entropy-measure-an-intuitive-explanation-a7f7e5d16421?source=collection_archive---------1-----------------------#2023-01-04)

[](https://tim-lou.medium.com/?source=post_page-----a7f7e5d16421--------------------------------)[![Tim Lou, PhD](../Images/e4931bb6d59e27730529ceaf00a23822.png)](https://tim-lou.medium.com/?source=post_page-----a7f7e5d16421--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7f7e5d16421--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a7f7e5d16421--------------------------------) [Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----a7f7e5d16421--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d41b438feef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-does-entropy-measure-an-intuitive-explanation-a7f7e5d16421&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=post_page-8d41b438feef----a7f7e5d16421---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7f7e5d16421--------------------------------) · 11 分钟阅读 · 2023年1月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa7f7e5d16421&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-does-entropy-measure-an-intuitive-explanation-a7f7e5d16421&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=-----a7f7e5d16421---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7f7e5d16421&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-does-entropy-measure-an-intuitive-explanation-a7f7e5d16421&source=-----a7f7e5d16421---------------------bookmark_footer-----------)

熵可能看起来抽象，但它有一个直观的方面：即数据中出现特定模式的概率。这是它的工作原理。

![](../Images/4daae7bdad8d3ebc360bb1ef70760dd8.png)

背景图片来源：Joe Maldonado [@unsplash](https://unsplash.com/@joesracingteam)

在数据科学中，有许多概念与熵的概念相关。最基本的是香农的信息熵，它为任何分布 *P*(*x*) 定义，通过以下公式：

其中总和是对所有可能的类别 *C* 进行的。

还有其他相关概念，其公式看起来类似：

+   [Kullback–Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)：用于比较两个分布

+   [互信息](https://en.wikipedia.org/wiki/Mutual_information)：用于捕捉两个变量之间的一般关系

+   [交叉熵](https://en.wikipedia.org/wiki/Cross_entropy)：用于训练分类模型

尽管类似熵的公式非常普遍，但很少有讨论这些公式背后的直觉：为什么涉及对数？为什么要将 *P*(*x*) 和 log *P*(*x*) 相乘？虽然许多文章提到“信息”、“预期惊讶”等术语，但缺乏其背后的直觉。

事实证明，和概率一样，熵可以通过计数练习来理解，并且可以与分布的某种对数似然联系起来。此外，这种计数可以与计算机中的字节数字面上联系起来。这些…
