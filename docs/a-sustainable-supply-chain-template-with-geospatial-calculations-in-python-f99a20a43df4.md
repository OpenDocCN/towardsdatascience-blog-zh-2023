# 一个包含地理空间计算的可持续供应链模板（Python 实现）

> 原文：[https://towardsdatascience.com/a-sustainable-supply-chain-template-with-geospatial-calculations-in-python-f99a20a43df4?source=collection_archive---------19-----------------------#2023-04-25](https://towardsdatascience.com/a-sustainable-supply-chain-template-with-geospatial-calculations-in-python-f99a20a43df4?source=collection_archive---------19-----------------------#2023-04-25)

## 供应链网络足迹应包括运输和配送中心的温室气体计算，而不仅仅是其中一个。

[](https://sabolch-horvat.medium.com/?source=post_page-----f99a20a43df4--------------------------------)[![Sabi Horvat](../Images/9de5f9318397230926326d4bd983f596.png)](https://sabolch-horvat.medium.com/?source=post_page-----f99a20a43df4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f99a20a43df4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f99a20a43df4--------------------------------) [Sabi Horvat](https://sabolch-horvat.medium.com/?source=post_page-----f99a20a43df4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4786f9cfa0d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-sustainable-supply-chain-template-with-geospatial-calculations-in-python-f99a20a43df4&user=Sabi+Horvat&userId=4786f9cfa0d3&source=post_page-4786f9cfa0d3----f99a20a43df4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f99a20a43df4--------------------------------) · 11 分钟阅读 · 2023年4月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff99a20a43df4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-sustainable-supply-chain-template-with-geospatial-calculations-in-python-f99a20a43df4&user=Sabi+Horvat&userId=4786f9cfa0d3&source=-----f99a20a43df4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff99a20a43df4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-sustainable-supply-chain-template-with-geospatial-calculations-in-python-f99a20a43df4&source=-----f99a20a43df4---------------------bookmark_footer-----------)![](../Images/e85bae59ebc38bfabfd4dd181d7e0943.png)

照片由 [Edelbert Macapagal](https://unsplash.com/ko/@emacapagal?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 供应链数据科学的可持续性

供应链数据科学家通常需要具备多种领域的知识，以支持各个部门。这种广度有助于实现*全球*优化供应链网络的目标，而不仅仅是优化*局部*环节；局部优化的组合很少能带来最佳的整体解决方案。

可持续性是供应链分析中的一个相对较新的领域。与*全球优于局部优化*的目标类似，我们也希望运用批判性思维，以实现最有价值的可持续性指标成果。

美国环境保护局（US EPA）计算了运输业作为美国五大经济部门中对温室气体排放贡献最大的一个[1]。

**交通运输是温室气体的重要来源，但在供应链计算中还需要考虑其他来源。**
