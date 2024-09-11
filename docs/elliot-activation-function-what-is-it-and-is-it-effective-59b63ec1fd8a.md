# Elliot 激活函数：它是什么，效果如何？

> 原文：[https://towardsdatascience.com/elliot-activation-function-what-is-it-and-is-it-effective-59b63ec1fd8a?source=collection_archive---------5-----------------------#2023-02-04](https://towardsdatascience.com/elliot-activation-function-what-is-it-and-is-it-effective-59b63ec1fd8a?source=collection_archive---------5-----------------------#2023-02-04)

## 什么是Elliot激活函数，它是否是神经网络中其他激活函数的良好替代？

[](https://ben-mccloskey20.medium.com/?source=post_page-----59b63ec1fd8a--------------------------------)[![Benjamin McCloskey](../Images/7118f5933f2affe2a7a4d3375452fa4c.png)](https://ben-mccloskey20.medium.com/?source=post_page-----59b63ec1fd8a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----59b63ec1fd8a--------------------------------)[![数据科学进展](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----59b63ec1fd8a--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----59b63ec1fd8a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F503796fc1483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felliot-activation-function-what-is-it-and-is-it-effective-59b63ec1fd8a&user=Benjamin+McCloskey&userId=503796fc1483&source=post_page-503796fc1483----59b63ec1fd8a---------------------post_header-----------) 发表在 [数据科学进展](https://towardsdatascience.com/?source=post_page-----59b63ec1fd8a--------------------------------) ·7分钟阅读·2023年2月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F59b63ec1fd8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felliot-activation-function-what-is-it-and-is-it-effective-59b63ec1fd8a&user=Benjamin+McCloskey&userId=503796fc1483&source=-----59b63ec1fd8a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F59b63ec1fd8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felliot-activation-function-what-is-it-and-is-it-effective-59b63ec1fd8a&source=-----59b63ec1fd8a---------------------bookmark_footer-----------)![](../Images/39a7ac7751ff21380e8ecc69c64edfb1.png)

Elliot 激活函数（图片来自作者）

# 介绍

你是否在创建新的机器学习模型时，对应该使用什么激活函数感到不确定？

**但等一下，*什么是激活函数？***

激活函数使机器学习模型能够理解和解决*非线性*问题。在神经网络中使用激活函数，尤其是有助于将每个神经元的重要信息传递到下一个神经元。如今，ReLU激活函数通常用于神经网络的架构中，但这并不意味着它总是最佳选择。（查看我下面关于ReLU和LReLU激活的文章）。

[](/leaky-relu-vs-relu-activation-functions-which-is-better-1a1533d0a89f?source=post_page-----59b63ec1fd8a--------------------------------) [## Leaky ReLU与ReLU激活函数：哪个更好？

### 一个实验用来调查使用ReLU激活函数时模型性能是否有明显差异…

[towardsdatascience.com](/leaky-relu-vs-relu-activation-functions-which-is-better-1a1533d0a89f?source=post_page-----59b63ec1fd8a--------------------------------)

我最近遇到了**艾略特激活函数**，它被赞誉为可能替代各种激活函数的选择…
