# SynthDiD 101: 初学者的合成差分法指南

> 原文：[`towardsdatascience.com/synthdid-101-a-beginners-guide-to-synthetic-difference-in-differences-84fed9b730ae?source=collection_archive---------6-----------------------#2023-04-26`](https://towardsdatascience.com/synthdid-101-a-beginners-guide-to-synthetic-difference-in-differences-84fed9b730ae?source=collection_archive---------6-----------------------#2023-04-26)

## 关于该方法的优缺点，通过 R 语言中的 synthdid 包进行演示

[](https://medium.com/@nalagoz13?source=post_page-----84fed9b730ae--------------------------------)![Nazlı Alagöz](https://medium.com/@nalagoz13?source=post_page-----84fed9b730ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84fed9b730ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----84fed9b730ae--------------------------------) [Nazlı Alagöz](https://medium.com/@nalagoz13?source=post_page-----84fed9b730ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ba02da50bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsynthdid-101-a-beginners-guide-to-synthetic-difference-in-differences-84fed9b730ae&user=Nazl%C4%B1+Alag%C3%B6z&userId=4ba02da50bf&source=post_page-4ba02da50bf----84fed9b730ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----84fed9b730ae--------------------------------) ·8 分钟阅读·2023 年 4 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F84fed9b730ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsynthdid-101-a-beginners-guide-to-synthetic-difference-in-differences-84fed9b730ae&user=Nazl%C4%B1+Alag%C3%B6z&userId=4ba02da50bf&source=-----84fed9b730ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F84fed9b730ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsynthdid-101-a-beginners-guide-to-synthetic-difference-in-differences-84fed9b730ae&source=-----84fed9b730ae---------------------bookmark_footer-----------)![](img/65c032916a89960487dbf785de9e47b3.png)

标题图像由作者使用 [Nightcafe](https://creator.nightcafe.studio/) 生成

在这篇博客文章中，我简要介绍了合成差分中的差分（SynthDiD）方法，并讨论了它与传统差分中的差分（DiD）方法和合成控制方法（SCM）的关系。SynthDiD 是 SCM 和 DiD 的一种泛化版本，它结合了两种方法的优点。它允许在大面板数据中进行因果推断，即使在治疗前期很短的情况下也是如此。我讨论了这种方法的优缺点，同时使用[R 中的 synthdi](https://synth-inference.github.io/synthdid/index.html)包演示了这种方法。我提供了简要的要点以便快速了解。

## 合成控制方法与合成差分中的差分方法

合成控制方法和合成差分中的差分方法密切相关，但在估计因果效应的方式上有所不同。合成控制方法是一种统计技术，通过将多个与处理单元在所有相关特征上相似的控制单元结合起来，创建一个“合成”控制组。这个合成控制组的构建旨在尽可能地匹配处理单元的治疗前结果。
