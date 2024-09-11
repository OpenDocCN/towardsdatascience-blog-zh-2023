# 《长短期之道：基于比例的相关性捕捉文档语义从头到尾》

> 原文：[https://towardsdatascience.com/the-long-and-short-of-it-proportion-based-relevance-to-capture-document-semantics-end-to-end-f5a755e5a82f?source=collection_archive---------7-----------------------#2023-11-25](https://towardsdatascience.com/the-long-and-short-of-it-proportion-based-relevance-to-capture-document-semantics-end-to-end-f5a755e5a82f?source=collection_archive---------7-----------------------#2023-11-25)

[](https://medium.com/@alcarazanthony1?source=post_page-----f5a755e5a82f--------------------------------)[![Anthony Alcaraz](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----f5a755e5a82f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5a755e5a82f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f5a755e5a82f--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----f5a755e5a82f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-long-and-short-of-it-proportion-based-relevance-to-capture-document-semantics-end-to-end-f5a755e5a82f&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----f5a755e5a82f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5a755e5a82f--------------------------------) ·5分钟阅读·2023年11月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff5a755e5a82f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-long-and-short-of-it-proportion-based-relevance-to-capture-document-semantics-end-to-end-f5a755e5a82f&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----f5a755e5a82f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff5a755e5a82f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-long-and-short-of-it-proportion-based-relevance-to-capture-document-semantics-end-to-end-f5a755e5a82f&source=-----f5a755e5a82f---------------------bookmark_footer-----------)

*使用了人工智能软件来增强本文的语法、流畅性和可读性。*

当前主流的搜索方法通常依赖于关键词匹配或向量空间相似性来估计查询与文档之间的相关性。然而，这些技术在使用整个文件、论文甚至书籍作为搜索查询时存在困难。

![](../Images/88c8318af5222375bec03ff04d3b5364.png)

与 Dall-E 3 的趣味互动

**基于关键词的检索**

虽然关键词搜索在短时间查找方面表现优秀，但它们未能捕捉对长篇内容至关重要的语义。一份正确讨论“云平台”的文档可能会被完全遗漏，而查询却寻求“AWS”方面的专业知识。确切的术语匹配在较长文本中经常面临词汇不匹配的问题。

**向量相似性搜索**

现代的向量嵌入模型如BERT将意义浓缩到数百个数值维度中，准确估计语义相似性。然而，具有自注意力的变换器架构在超过512–1024个令牌时因计算量爆炸而无法扩展。

在无法完全摄取文档的情况下，结果中的“词袋”部分嵌入会丢失分布在各个部分的含义细微差别。上下文会被丢失……
