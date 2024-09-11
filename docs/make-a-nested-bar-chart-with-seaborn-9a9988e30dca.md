# 使用 Seaborn 制作嵌套条形图

> 原文：[https://towardsdatascience.com/make-a-nested-bar-chart-with-seaborn-9a9988e30dca?source=collection_archive---------10-----------------------#2023-10-19](https://towardsdatascience.com/make-a-nested-bar-chart-with-seaborn-9a9988e30dca?source=collection_archive---------10-----------------------#2023-10-19)

## 我快速成功数据科学

## 基准测试大学橄榄球投票的准确性

[](https://medium.com/@lee_vaughan?source=post_page-----9a9988e30dca--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----9a9988e30dca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a9988e30dca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9a9988e30dca--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----9a9988e30dca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-nested-bar-chart-with-seaborn-9a9988e30dca&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----9a9988e30dca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a9988e30dca--------------------------------) ·9分钟阅读·2023年10月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a9988e30dca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-nested-bar-chart-with-seaborn-9a9988e30dca&user=Lee+Vaughan&userId=5d604015c08b&source=-----9a9988e30dca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a9988e30dca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-nested-bar-chart-with-seaborn-9a9988e30dca&source=-----9a9988e30dca---------------------bookmark_footer-----------)![](../Images/374d6c5dd302c2d2c0047599a38579a3.png)

嵌套条形图的示例（作者）

*嵌套条形图* 是一种在类别中比较多个度量的方法。其中一个度量表示次要或*背景*度量，如*目标*或*以前*的值。主要度量表示*实际*或*当前*的值。

次要度量通常以*缩减的能力*呈现，从而为主要度量提供背景。在较窄且较浅的次要条形上放置较宽且较深的主要条形，可以产生一个吸引人且紧凑的图表。这也解释了为什么这种图形有时被称为*口红条形图*。

![](../Images/5556ea6fec96d5bba13e831b46480f10.png)

口红图片由 Onder Ortel 在 Unsplash 上提供

当然，为了使其正常工作，主条形图的长度*绝不能*超过次条形图。因此，你需要使用嵌套条形图来绘制每个类别的*减少*值的示例，例如房价下降或由于新疫苗导致的疾病率下降。

在这个*快速成功数据科学*项目中，我们将探讨著名的*美联社大学橄榄球*…
