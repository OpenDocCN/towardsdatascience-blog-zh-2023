# 通过示例学习精益数据管道

> 原文：[https://towardsdatascience.com/a-lean-data-pipeline-by-example-e08bfce58133?source=collection_archive---------12-----------------------#2023-04-18](https://towardsdatascience.com/a-lean-data-pipeline-by-example-e08bfce58133?source=collection_archive---------12-----------------------#2023-04-18)

## 像Snowflake和Databricks这样的强大云数据平台改变了我们对标准形式和数据仓库的思考方式

[](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)[![Tim Burns](../Images/70e4b9a6510a60cdf343dd0f7f92e943.png)](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------) [Tim Burns](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa05443c6167e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-lean-data-pipeline-by-example-e08bfce58133&user=Tim+Burns&userId=a05443c6167e&source=post_page-a05443c6167e----e08bfce58133---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------) · 7 min read · 2023年4月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe08bfce58133&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-lean-data-pipeline-by-example-e08bfce58133&user=Tim+Burns&userId=a05443c6167e&source=-----e08bfce58133---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe08bfce58133&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-lean-data-pipeline-by-example-e08bfce58133&source=-----e08bfce58133---------------------bookmark_footer-----------)![](../Images/c75ed1033a8be71c7cc1007e08423f67.png)

图片由 [David Di Veroli](https://unsplash.com/@daviddiveroli?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/-m1duEoiJng?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

在降低云数据管道成本时，首先想到的是寻找最便宜的ETL解决方案。例如，Databricks是否比Snowflake便宜？我应该使用云服务还是在内部集群上本地运行Spark？

更好地考虑成本最小化的一种方法是使用精益原则。精益创业的原则之一是通过小步验证[[1](https://theleanstartup.com/)]。专注于价值对于云数据管道尤为重要，因为每一次规范化和维度都会造成成本。此外，50年的数据管道提供了丰富的转换可能性，但只有对客户有分析价值的转换才是我们*应该*做的。

在本文中，我描述了如何通过专注于根据需要转换数据来开发业务价值来实现成本最小化的方法。这样，您可以根据您独特的业务情况选择大数据平台，而不是进行成本最小化的练习。只要您知道在最小化数据转换时的权衡，避免执行转换是一个好主意，如果您的业务...
