# RAG 的未解之谜：应对领域特定搜索中的挑战

> 原文：[https://towardsdatascience.com/the-untold-side-of-rag-addressing-its-challenges-in-domain-specific-searches-808956e3ecc8?source=collection_archive---------0-----------------------#2023-10-18](https://towardsdatascience.com/the-untold-side-of-rag-addressing-its-challenges-in-domain-specific-searches-808956e3ecc8?source=collection_archive---------0-----------------------#2023-10-18)

## 使用混合搜索、层级排名和讲师嵌入技术来解决我们 RAG 设置中领域特定文档的相似性问题。

[](https://agustinus-nalwan.medium.com/?source=post_page-----808956e3ecc8--------------------------------)[![Agustinus Nalwan](../Images/7c5ade9ab8bca1d27a317b5c09d1b734.png)](https://agustinus-nalwan.medium.com/?source=post_page-----808956e3ecc8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----808956e3ecc8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----808956e3ecc8--------------------------------) [Agustinus Nalwan](https://agustinus-nalwan.medium.com/?source=post_page-----808956e3ecc8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8b7ab157b0a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-untold-side-of-rag-addressing-its-challenges-in-domain-specific-searches-808956e3ecc8&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=post_page-8b7ab157b0a4----808956e3ecc8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----808956e3ecc8--------------------------------) ·29 分钟阅读·2023年10月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F808956e3ecc8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-untold-side-of-rag-addressing-its-challenges-in-domain-specific-searches-808956e3ecc8&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=-----808956e3ecc8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F808956e3ecc8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-untold-side-of-rag-addressing-its-challenges-in-domain-specific-searches-808956e3ecc8&source=-----808956e3ecc8---------------------bookmark_footer-----------)![](../Images/4e2dd6573d0b19b4a98e8fb5dbae636d.png)

生成式 AI 增强的搜索技术（图像由 MidJourney 生成）

[Carsales](https://www.carsales.com.au/) 作为领先的汽车平台，服务于澳大利亚、智利、韩国和美国的汽车和生活方式车辆市场。我们的目标是重新定义汽车购买和销售体验，设立无与伦比的标准。为此，我们的一项关键功能是一个全面的搜索工具，可以扫描成千上万篇与汽车相关的编辑文章。我们目前整合了 Google 搜索——专门为我们的编辑内容量身定制，通过 iframe 展示——虽然结果尚可，但主要依赖于词汇（关键字）关联，有时会错过搜索查询背后的真正本质或语义。

![](../Images/95dce076f1586ea6916a057ff1db4e15.png)

使用现有的 Google 搜索进行的搜索结果

例如，搜索“2020 年丰田卡罗拉的气囊数量是多少？”会返回包含“丰田卡罗拉和气囊”等词汇的文章。然而，这些文章大多讨论的是气囊召回，而不是……
