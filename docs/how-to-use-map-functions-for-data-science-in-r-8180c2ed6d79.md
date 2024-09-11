# 如何在 R 中使用 Map 函数进行数据科学

> 原文：[`towardsdatascience.com/how-to-use-map-functions-for-data-science-in-r-8180c2ed6d79?source=collection_archive---------10-----------------------#2023-02-02`](https://towardsdatascience.com/how-to-use-map-functions-for-data-science-in-r-8180c2ed6d79?source=collection_archive---------10-----------------------#2023-02-02)

## 从 tidyverse 学习强大的函数式编程工具

[](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)![Rory Spanton](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39501aa8ce39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-map-functions-for-data-science-in-r-8180c2ed6d79&user=Rory+Spanton&userId=39501aa8ce39&source=post_page-39501aa8ce39----8180c2ed6d79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------) ·7 min read·2023 年 2 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8180c2ed6d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-map-functions-for-data-science-in-r-8180c2ed6d79&user=Rory+Spanton&userId=39501aa8ce39&source=-----8180c2ed6d79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8180c2ed6d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-map-functions-for-data-science-in-r-8180c2ed6d79&source=-----8180c2ed6d79---------------------bookmark_footer-----------)![](img/7814864921e31d3a070fa03387a45857.png)

图片由 [Z](https://unsplash.com/@dead____artist?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所有数据科学家都需要重复代码。无论你是将模型拟合到多个数据集上，还是同时更改许多值，反复运行相同的代码都是必不可少的。

重复代码有很多方法。但是虽然大多数程序员使用循环，但还有更简洁、可读且高效的替代方案。进入，来自 purrr 包的 `map` 函数族。

在这篇文章中，我将解释映射的含义，以及如何在 R 中使用[purrr 包](https://purrr.tidyverse.org/)中的`map`、`map2`和`pmap`函数。

# 什么是“映射”，它在 R 中是如何实现的？

purrr 包所做的映射并不是大多数人熟悉的地理类型。R 有很棒的地理空间分析工具，但这不是我们要讨论的内容。

“[映射](https://en.wikipedia.org/wiki/Map_(higher-order_function))”是编程中的一个专业术语，指的是在一组参数上重复应用一个函数。

我们可以用一个简单的例子来理解这一点。假设我们有一个列表，每个元素包含 100 个数字。如果我们想计算每组数字的均值，我们可以用`map`来直接实现。
