# 普通人的数据建模，第1部分：什么是数据建模？

> 原文：[https://towardsdatascience.com/data-modeling-for-mere-mortals-part-1-what-is-data-modeling-103eb184930e?source=collection_archive---------6-----------------------#2023-08-02](https://towardsdatascience.com/data-modeling-for-mere-mortals-part-1-what-is-data-modeling-103eb184930e?source=collection_archive---------6-----------------------#2023-08-02)

## 数据建模有时可能看起来很吓人（所有那些复杂的图表！），但其实并不必如此。

[](https://datamozart.medium.com/?source=post_page-----103eb184930e--------------------------------)[![Nikola Ilic](../Images/9fab894b9696c0dfd80c5173188b720b.png)](https://datamozart.medium.com/?source=post_page-----103eb184930e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----103eb184930e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----103eb184930e--------------------------------) [Nikola Ilic](https://datamozart.medium.com/?source=post_page-----103eb184930e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64005b7daa38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-1-what-is-data-modeling-103eb184930e&user=Nikola+Ilic&userId=64005b7daa38&source=post_page-64005b7daa38----103eb184930e---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----103eb184930e--------------------------------) ·12 分钟阅读·2023 年 8 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F103eb184930e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-1-what-is-data-modeling-103eb184930e&user=Nikola+Ilic&userId=64005b7daa38&source=-----103eb184930e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F103eb184930e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-1-what-is-data-modeling-103eb184930e&source=-----103eb184930e---------------------bookmark_footer-----------)![](../Images/f68e515f2767c1fe53db6c0958c7de5f.png)

[Alina Grubnyak 在 Unsplash 的照片](https://unsplash.com/de/fotos/ZiQkhI7417A)

近年来，我已经为各种受众进行了数十次关于各种数据平台主题的培训。在教授各种数据平台概念和技术时，我发现有一个概念特别让许多业务分析师感到害怕，尤其是那些刚开始自己旅程的人。那就是***数据建模的概念***。

# 为什么数据建模有时会让人感到害怕？

也许是因为当你看到这些看起来如此复杂和繁琐的图表时，你会感到迷失……

但数据建模并不是关于图表的。它是关于建立信任，即在业务和数据专业人士之间建立共同理解，最终目标是通过数据提供更高的业务价值。

如果我们同意数据建模是关于建立信任的，我相信我们也可以同意，信任不是轻易建立的——这需要一定的时间和精力。而时间和精力不是你可以视为理所当然的——这是你需要**投入**的！
