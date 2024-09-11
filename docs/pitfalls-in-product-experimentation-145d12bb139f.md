# 产品实验中的陷阱

> 原文：[https://towardsdatascience.com/pitfalls-in-product-experimentation-145d12bb139f?source=collection_archive---------18-----------------------#2023-01-17](https://towardsdatascience.com/pitfalls-in-product-experimentation-145d12bb139f?source=collection_archive---------18-----------------------#2023-01-17)

![](../Images/69c395fd20f7e52c33a86110cf56307b.png)

图片由 pikisuperstar 提供，来源于 Freepik (www.freepik.com)

## 在产品实验中常被忽视的待办事项清单，导致结果不佳且不可靠

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----145d12bb139f--------------------------------)[![Olivia Tanuwidjaja](../Images/52a56de28da9b782b57f1c3928655cfb.png)](https://tanuwidjajaolivia.medium.com/?source=post_page-----145d12bb139f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----145d12bb139f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----145d12bb139f--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----145d12bb139f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff43d6dd597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpitfalls-in-product-experimentation-145d12bb139f&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=post_page-f43d6dd597----145d12bb139f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----145d12bb139f--------------------------------) ·8 min read·2023年1月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F145d12bb139f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpitfalls-in-product-experimentation-145d12bb139f&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=-----145d12bb139f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F145d12bb139f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpitfalls-in-product-experimentation-145d12bb139f&source=-----145d12bb139f---------------------bookmark_footer-----------)

我们都知道产品实验的重要性，**其好处已经被组织大大证明**，能够基于数据做出关于产品、特性和流程的决策。[谷歌在搜索结果中测试了40种蓝色](https://bambrick.com.au/blog/google-increased-revenue-200-million-just-finding-perfect-shade-blue/)，而正确的蓝色使收入增加了2亿。[Booking.com 已确认](https://hbr.org/2020/03/building-a-culture-of-experimentation) 组织的扩展和转型得益于在那里进行的大量测试和实验。

然而，产品实验和其他统计测试或实验一样，**容易出现陷阱**。这些陷阱可能是设计和/或执行上的缺陷，可能在整个过程中隐蔽或未被察觉。数据团队——产品数据分析师/数据科学家——有责任对实验的执行和分析进行监管，以确保结果的可靠性。因此，了解常见的陷阱及其处理方法非常重要，因为这些陷阱可能会误导分析结果和结论。

> 如果实验没有被正确配置和分析，可能会导致结果不佳且不可靠，破坏实验的初衷——即测试处理方法并评估其影响。
