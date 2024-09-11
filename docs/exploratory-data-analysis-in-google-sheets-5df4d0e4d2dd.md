# Google Sheets中的探索性数据分析

> 原文：[https://towardsdatascience.com/exploratory-data-analysis-in-google-sheets-5df4d0e4d2dd?source=collection_archive---------10-----------------------#2023-07-14](https://towardsdatascience.com/exploratory-data-analysis-in-google-sheets-5df4d0e4d2dd?source=collection_archive---------10-----------------------#2023-07-14)

## Google Sheets与Pandas方法比较

[](https://dmitryelj.medium.com/?source=post_page-----5df4d0e4d2dd--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----5df4d0e4d2dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5df4d0e4d2dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5df4d0e4d2dd--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----5df4d0e4d2dd--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-in-google-sheets-5df4d0e4d2dd&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----5df4d0e4d2dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5df4d0e4d2dd--------------------------------) ·8 min read·Jul 14, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5df4d0e4d2dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-in-google-sheets-5df4d0e4d2dd&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----5df4d0e4d2dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5df4d0e4d2dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-in-google-sheets-5df4d0e4d2dd&source=-----5df4d0e4d2dd---------------------bookmark_footer-----------)![](../Images/efcc38e54fb04154d229d5a79500bfca.png)

由作者生成的图像

使用像Pandas或Jupyter这样的现代工具来处理数据总是很棒的。但是，设想一下，当一个同事或朋友要求进行数据分析，但他或她不是技术人员，不使用Python或Jupyter，并且没有Tableau、Power BI或其他任何昂贵（但遗憾的是不是免费的）服务的账户时，会发生什么。在这种情况下，使用Google Sheets处理数据可能是一个不错的解决方法，原因有以下几点：

+   Google在全球范围内使用；在撰写本文时，已有超过18亿用户拥有Google账户。如今几乎每个人都有Google账户，因此文档共享将变得非常简单。

+   Google 的生态系统是安全的和可靠的。它支持双因素认证和现代安全标准，甚至可以在有限的人群之间共享私人数据集。

+   最后但并非最不重要的是，这个解决方案是免费的，不需要任何额外费用。而且作为额外福利，Google Sheets 可以在浏览器中运行，无需安装任何软件，并且可以在像 Windows、Linux、OSX 甚至智能手机等任何平台上工作。

在本文中，我将在 Pandas 中进行基本的探索性数据分析，然后我们将在 Google...。
