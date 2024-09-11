# 我的第一次探索性数据分析与 ChatGPT

> 原文：[https://towardsdatascience.com/my-first-exploratory-data-analysis-with-chatgpt-7f100005efdc?source=collection_archive---------0-----------------------#2023-05-10](https://towardsdatascience.com/my-first-exploratory-data-analysis-with-chatgpt-7f100005efdc?source=collection_archive---------0-----------------------#2023-05-10)

## 释放 ChatGPT 的力量：深入探索数据分析及未来机会

[](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)[![Jye Sawtell-Rickson](../Images/e16c2b5020d24331812a4f35e9bd7890.png)](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------) [Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d976cb1305&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-first-exploratory-data-analysis-with-chatgpt-7f100005efdc&user=Jye+Sawtell-Rickson&userId=74d976cb1305&source=post_page-74d976cb1305----7f100005efdc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------) ·15分钟阅读·2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7f100005efdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-first-exploratory-data-analysis-with-chatgpt-7f100005efdc&user=Jye+Sawtell-Rickson&userId=74d976cb1305&source=-----7f100005efdc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f100005efdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-first-exploratory-data-analysis-with-chatgpt-7f100005efdc&source=-----7f100005efdc---------------------bookmark_footer-----------)![](../Images/2d8f14196cf7b2834ea040accdadbbe9.png)

“一个 AI 探索广阔的数据世界。数字艺术。生动的色彩。”（作者通过 DALL-E 2 生成）

ChatGPT 是一个非凡的工具，可以提高工作效率，这一点在数据分析中也不例外。在本文中，我们将通过一个 ChatGPT 运行的探索性数据分析（EDA）示例进行演示。我们将覆盖 EDA 的各个阶段，查看一些令人印象深刻的输出（词云！），并指出 ChatGPT 做得好的地方（以及不那么好的地方）。最后，我们将探讨大型语言模型在分析中的未来，以及我们对此的激动之情。

用于分析的数据集来自于[Common Crawl](https://commoncrawl.org/)，这是一个[任何人都可以免费访问和分析](https://commoncrawl.org/terms-of-use/)的资源。Common Crawl 数据集是一个庞大的网络爬虫数据集合，包含了来自互联网的数十亿个网页。该数据集包括各种类型的网页内容，并且定期更新。它作为训练语言模型（如LLM）的重要资源，占据了[ChatGPT训练数据的60%](https://medium.com/@dlaytonj2/chatgpt-show-me-the-data-sources-11e9433d57e8)。你可以在Kaggle上找到作者整理的数据集样本，[点击这里](https://www.kaggle.com/datasets/jyesawtellrickson/commoncrawl)查看。

在整个帖子中，内容将会被截断，因此可以直接在[用于运行此分析的Google Colab](https://colab.research.google.com/drive/1REJ3xa37Z3milf8-4-1-2fB69AONI0Zz)上跟进。
