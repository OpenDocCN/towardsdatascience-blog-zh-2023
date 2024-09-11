# 如何在数据工程团队中建立值班文化

> 原文：[`towardsdatascience.com/how-to-build-an-on-call-culture-in-a-data-engineering-team-7856fac0c99?source=collection_archive---------4-----------------------#2023-03-15`](https://towardsdatascience.com/how-to-build-an-on-call-culture-in-a-data-engineering-team-7856fac0c99?source=collection_archive---------4-----------------------#2023-03-15)

## 系统性地解决生产中的数据问题

[](https://medium.com/@xiaoxugao?source=post_page-----7856fac0c99--------------------------------)![Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----7856fac0c99--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7856fac0c99--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7856fac0c99--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----7856fac0c99--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2adc5a07e772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-on-call-culture-in-a-data-engineering-team-7856fac0c99&user=Xiaoxu+Gao&userId=2adc5a07e772&source=post_page-2adc5a07e772----7856fac0c99---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7856fac0c99--------------------------------) · 9 分钟阅读 · 2023 年 3 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7856fac0c99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-on-call-culture-in-a-data-engineering-team-7856fac0c99&user=Xiaoxu+Gao&userId=2adc5a07e772&source=-----7856fac0c99---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7856fac0c99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-on-call-culture-in-a-data-engineering-team-7856fac0c99&source=-----7856fac0c99---------------------bookmark_footer-----------)![](img/a16b492e8b21d711105088072c9758a6.png)

图片由 [Pavan Trikutam](https://unsplash.com/@ptrikutam) 提供，来源于 [Unsplash](https://unsplash.com/)

在任何公司中，获得和留住客户的最佳方式之一是提供优质服务，即服务在客户访问时应保持健康和功能正常。为了实现这一点，技术行业引入了值班制度，这一制度过去通常与医生相关联。

不同公司对值班的定义有所不同。但这里是一般的描述：值班意味着在某一段时间内保持可用状态，能够以适当的紧急程度响应生产事故。值班通常与软件工程师和站点可靠性工程师（SRE）相关，以支持如 API、网站、移动应用、物联网服务等软件。

数据工程师似乎不像他们的同行那样频繁参与值班。我将在下一部分解释原因。但这种情况将会改变。

作为一个对多学科感兴趣的人，我想分享一下我们如何在数据工程团队中建立值班文化。大部分经验来自于我目前的团队，但我也希望听听你们团队的做法以及任何有趣的见解。

## 为什么值班在…不是一件事
