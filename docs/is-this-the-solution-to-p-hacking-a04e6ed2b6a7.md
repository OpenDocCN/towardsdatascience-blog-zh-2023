# 这是 P-黑客行为的解决方案吗？

> 原文：[`towardsdatascience.com/is-this-the-solution-to-p-hacking-a04e6ed2b6a7?source=collection_archive---------7-----------------------#2023-11-16`](https://towardsdatascience.com/is-this-the-solution-to-p-hacking-a04e6ed2b6a7?source=collection_archive---------7-----------------------#2023-11-16)

![](img/268cfbc4852390989c44eab83f520315.png)

E 是新的 P 吗？图像由作者使用 Dall·E 创建。

## E-值，p-值的更好替代方案

[](https://hennie-de-harder.medium.com/?source=post_page-----a04e6ed2b6a7--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----a04e6ed2b6a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a04e6ed2b6a7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a04e6ed2b6a7--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----a04e6ed2b6a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-this-the-solution-to-p-hacking-a04e6ed2b6a7&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----a04e6ed2b6a7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a04e6ed2b6a7--------------------------------) ·11 min read·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa04e6ed2b6a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-this-the-solution-to-p-hacking-a04e6ed2b6a7&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----a04e6ed2b6a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa04e6ed2b6a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-this-the-solution-to-p-hacking-a04e6ed2b6a7&source=-----a04e6ed2b6a7---------------------bookmark_footer-----------)

**在科学研究中，数据操控和结果窥探一直是存在的问题。研究人员通常追求显著的 p-值以便发表，这可能导致提前停止数据收集或操控数据的诱惑。这种行为称为 p-黑客行为，是** **我之前的文章****的重点。如果研究人员决定故意更改数据值或伪造完整的数据集，我们对此无能为力。然而，对于某些 p-黑客行为，可能有可用的解决方案！**

在这篇文章中，我们深入探讨了安全测试的主题。安全测试相对于传统的假设检验有一些明显的优势。例如，这种测试方法允许将多个研究的结果进行结合。另一个优势是，你可以随时选择停止实验。为了说明安全测试，我们将使用由提出该理论的研究人员开发的 R 包[safestats](https://cran.r-project.org/package=safestats)。首先，我们将介绍 E 值并解释它们可以解决的问题。E 值已经被像 Netflix 和 Amazon 这样的公司使用，因为它们的好处。

> 我不会深入探讨理论的证明；相反，本文采用更为实际的方法，展示如何…
