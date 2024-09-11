# 使用变换器进行爵士和弦解析

> 原文：[`towardsdatascience.com/jazz-chords-parsing-with-transformers-d75031a976f2?source=collection_archive---------7-----------------------#2023-08-01`](https://towardsdatascience.com/jazz-chords-parsing-with-transformers-d75031a976f2?source=collection_archive---------7-----------------------#2023-08-01)

## 一种基于数据的方法进行树状音乐分析

[](https://medium.com/@foscarin.francesco?source=post_page-----d75031a976f2--------------------------------)![弗朗切斯科·福斯卡林](https://medium.com/@foscarin.francesco?source=post_page-----d75031a976f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d75031a976f2--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----d75031a976f2--------------------------------) [弗朗切斯科·福斯卡林](https://medium.com/@foscarin.francesco?source=post_page-----d75031a976f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb4fb56d8a44&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjazz-chords-parsing-with-transformers-d75031a976f2&user=Francesco+Foscarin&userId=b4fb56d8a44&source=post_page-b4fb56d8a44----d75031a976f2---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----d75031a976f2--------------------------------) ·11 min read·2023 年 8 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd75031a976f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjazz-chords-parsing-with-transformers-d75031a976f2&user=Francesco+Foscarin&userId=b4fb56d8a44&source=-----d75031a976f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd75031a976f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjazz-chords-parsing-with-transformers-d75031a976f2&source=-----d75031a976f2---------------------bookmark_footer-----------)![](img/12225dc1bc607140dfc63b87d3353462.png)

在本文中，我总结了我的研究论文[“使用基于图的神经解码器预测音乐层次结构”](https://arxiv.org/abs/2306.16955)，该论文介绍了一种能够解析爵士和弦序列的数据驱动系统。

本研究**源于我对基于语法的解析系统的挫败感**（这些系统曾是处理音乐数据的唯一选择）：

+   语法构建阶段需要大量的领域知识

+   解析器在遇到一些未知的配置或嘈杂数据时会失败

+   在单一语法规则中考虑多个音乐维度是具有挑战性的

+   目前没有得到充分支持的主动 Python 框架来帮助开发

**我的方法**（受到自然语言处理领域类似工作的启发），**不依赖于任何语法**，**对嘈杂输入产生部分结果**，**轻松处理多个音乐维度，并在 PyTorch 中实现**。

如果你对解析和语法不熟悉，或者需要刷新你的知识，我现在将退一步。

*什么是“解析”？*

术语*解析*指的是预测/推断一棵树（数学结构）……
