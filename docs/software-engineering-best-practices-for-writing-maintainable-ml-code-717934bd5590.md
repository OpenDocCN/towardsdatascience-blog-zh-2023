# 编写可维护 ML 代码的最佳软件工程实践

> 原文：[`towardsdatascience.com/software-engineering-best-practices-for-writing-maintainable-ml-code-717934bd5590?source=collection_archive---------2-----------------------#2023-08-06`](https://towardsdatascience.com/software-engineering-best-practices-for-writing-maintainable-ml-code-717934bd5590?source=collection_archive---------2-----------------------#2023-08-06)

![](img/1d59ed9c7fdba6ad95f054d4357d2f46.png)

一位迷失在满是代码的森林中的数据科学家。与第二个和最后一个提示相关。图像由 [Midjourney](https://www.midjourney.com) 创作。

## 数据科学家的高级编码技巧

[](https://hennie-de-harder.medium.com/?source=post_page-----717934bd5590--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----717934bd5590--------------------------------)[](https://towardsdatascience.com/?source=post_page-----717934bd5590--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----717934bd5590--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----717934bd5590--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-engineering-best-practices-for-writing-maintainable-ml-code-717934bd5590&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----717934bd5590---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----717934bd5590--------------------------------) · 11 分钟阅读 · 2023 年 8 月 6 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F717934bd5590&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-engineering-best-practices-for-writing-maintainable-ml-code-717934bd5590&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----717934bd5590---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F717934bd5590&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-engineering-best-practices-for-writing-maintainable-ml-code-717934bd5590&source=-----717934bd5590---------------------bookmark_footer-----------)

**与传统的软件工程项目不同，由于其复杂和不断变化的特性，ML 代码库往往在代码质量上滞后，导致技术债务增加和协作困难。优先考虑可维护性对于创建能够适应、扩展并随时间提供价值的稳健 ML 解决方案至关重要。**

近年来，机器学习引起了全球的轰动，改变了从医疗保健到金融等多个行业。随着越来越多的组织加入机器学习的浪潮以探索新的可能性和洞察力，编写可维护和稳健的机器学习代码的意义变得至关重要。通过编写易于使用且经得起时间考验的机器学习代码，团队可以更好地协作，并确保随着模型和项目的发展和适应而获得成功。接下来的部分将展示机器学习代码库中的常见示例，并解释如何正确处理这些问题。

# 不要创建庞大的单块脚本

这个建议可能与你无关，但它是为那些（直到现在）还没有意识到这一点的个人而写的！

单块脚本，即整个项目的单一脚本，可能在你重用实验代码时出现…
