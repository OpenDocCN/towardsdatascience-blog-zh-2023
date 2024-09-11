# FinOps：减少BigQuery存储成本的四种方法

> 原文：[https://towardsdatascience.com/finops-four-ways-to-reduce-your-bigquery-storage-cost-82d99c47f139?source=collection_archive---------2-----------------------#2023-01-30](https://towardsdatascience.com/finops-four-ways-to-reduce-your-bigquery-storage-cost-82d99c47f139?source=collection_archive---------2-----------------------#2023-01-30)

## 不要忽视云存储成本

[](https://medium.com/@xiaoxugao?source=post_page-----82d99c47f139--------------------------------)[![Xiaoxu Gao](../Images/8712a7e5f3bad0d2abd7e04792fad66f.png)](https://medium.com/@xiaoxugao?source=post_page-----82d99c47f139--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82d99c47f139--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82d99c47f139--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----82d99c47f139--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2adc5a07e772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinops-four-ways-to-reduce-your-bigquery-storage-cost-82d99c47f139&user=Xiaoxu+Gao&userId=2adc5a07e772&source=post_page-2adc5a07e772----82d99c47f139---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----82d99c47f139--------------------------------) · 9分钟阅读 · 2023年1月30日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82d99c47f139&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinops-four-ways-to-reduce-your-bigquery-storage-cost-82d99c47f139&user=Xiaoxu+Gao&userId=2adc5a07e772&source=-----82d99c47f139---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82d99c47f139&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinops-four-ways-to-reduce-your-bigquery-storage-cost-82d99c47f139&source=-----82d99c47f139---------------------bookmark_footer-----------)![](../Images/4ae5fa9251fd278ef8b86e5381d1c56e.png)

照片由[Nathan Dumlao](https://unsplash.com/@nate_dumlao)提供，来源于[Unsplash](https://unsplash.com/)

在当前经济形势下，最大限度地利用现有现金并制定一系列成本优化策略比以往任何时候都更为重要。云服务的广泛使用不仅为业务带来了许多机会，还可能引发管理挑战，从而导致成本超支和其他问题。

FinOps，这一新引入的概念，是一个不断发展的运营框架和文化转变，它通过云端转型将技术、财务和业务整合在一起，从而使组织获得最大的商业价值。其核心支柱之一是支出控制。这不是削减业务，而是更好地了解云端的可能性，并优化资源以实现相同目标的同时减少开支。

我们今天关注的领域是 BigQuery 存储成本。许多人认为存储很便宜，这并不完全错误。根据云存储和数据备份公司 Backblaze 的数据，自 2009 年以来，每 Gigabyte 的成本已经下降了 90%。

![](../Images/b8adc4aa83841b1e18f0b7b1f0d22044.png)

来源: [Backblaze](https://www.backblaze.com/blog/hard-drive-cost-per-gigabyte/)
