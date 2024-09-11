# 如何使用 dbt Seeds

> 原文：[`towardsdatascience.com/how-to-use-dbt-seeds-f9239c347711?source=collection_archive---------11-----------------------#2023-04-25`](https://towardsdatascience.com/how-to-use-dbt-seeds-f9239c347711?source=collection_archive---------11-----------------------#2023-04-25)

## 它们是什么以及何时使用

[](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)![Madison Schott](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ed0ce2dcf93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-dbt-seeds-f9239c347711&user=Madison+Schott&userId=3ed0ce2dcf93&source=post_page-3ed0ce2dcf93----f9239c347711---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------) ·4 min read·2023 年 4 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff9239c347711&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-dbt-seeds-f9239c347711&user=Madison+Schott&userId=3ed0ce2dcf93&source=-----f9239c347711---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9239c347711&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-dbt-seeds-f9239c347711&source=-----f9239c347711---------------------bookmark_footer-----------)![](img/c67417df60fabed2bd778b5ff088e330.png)

由 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/s/photos/seeds?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

老实说，我已经很久没使用过 [dbt seeds](https://docs.getdbt.com/docs/build/seeds) 了。实际上，我在几天前刚刚重新发现它们，当时我在尝试验证对我们一个数据模型的更改。

我的经理给我发了一份包含重复值的 CSV 文件，这些值出现在我们的一个数据模型中。我正在处理删除这些重复值的工作。修改完成后，我需要验证这些重复值是否不再出现在模型中。

我使用 dbt seeds 将 CSV 文件导入到我们的数据仓库中，而不是手动上传到几乎不可能使用 Redshift 的数据仓库中。然后我可以快速运行验证查询，确认重复项已被移除。

太简单了！作为一名分析工程师，我*喜欢*事情简单。因为老实说，事情很少如此简单。这让我思考如何进一步使用 dbt seeds 让我们的分析工程师工作变得更轻松。

更不用说，dbt seeds 还可以被数据科学家和机器学习工程师用于测试他们的模型。你可以用它来摄取测试数据和训练数据，让它们在数据仓库中作为两个不同的参考点存在。

# 什么是 dbt seeds？
