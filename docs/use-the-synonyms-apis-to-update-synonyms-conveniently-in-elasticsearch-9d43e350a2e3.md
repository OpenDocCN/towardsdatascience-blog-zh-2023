# 使用同义词 API 方便地更新 Elasticsearch 中的同义词

> 原文：[https://towardsdatascience.com/use-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3?source=collection_archive---------8-----------------------#2023-11-25](https://towardsdatascience.com/use-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3?source=collection_archive---------8-----------------------#2023-11-25)

## 了解一种新的方法，无需重新索引或重新加载即可更新 Elasticsearch 的同义词

[](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)[![Lynn G. Kwong](../Images/b9a05b6587db5ca41c1d8264adda5b06.png)](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----9d43e350a2e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------) ·5分钟阅读·2023年11月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9d43e350a2e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----9d43e350a2e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9d43e350a2e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3&source=-----9d43e350a2e3---------------------bookmark_footer-----------)![](../Images/b3193155f751500d3dee5b98319411e9.png)

图片由 Tumisu 提供于 Pixabay

Elasticsearch 的同义词功能非常强大，正确使用时可以显著提升搜索引擎的效率。使用同义词功能时的一个常见问题是更新同义词集合。

在索引设置中定义的内联同义词不能直接更新，我们需要关闭索引、更新设置并重新打开索引才能使更改生效。另一种方法是使用可以通过重新加载索引进行更新的同义词文件。然而，当 Elasticsearch 服务器分布式部署或托管在云端时，使用索引文件是难以管理的。这是因为我们需要将文件放在所有集群节点上。

好消息是，现在有第三种方法，这种方法比之前的两种方法更为方便。我们现在可以使用同义词 API 来管理同义词。尽管在撰写时这仍然是 Elasticsearch 的一个测试功能，我认为它很快会被采用，因为这个功能受到开发者的高度需求，并且能够非常方便地解决更新同义词集的问题。在本文中，我们将探讨同义词 API 的常见用法。
