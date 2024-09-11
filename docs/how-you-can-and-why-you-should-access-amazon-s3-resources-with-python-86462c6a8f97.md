# 如何使用 Python 访问 Amazon S3 资源（及为什么你应该这样做）

> 原文：[https://towardsdatascience.com/how-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97?source=collection_archive---------21-----------------------#2023-01-24](https://towardsdatascience.com/how-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97?source=collection_archive---------21-----------------------#2023-01-24)

## 使用自动化将数据迁移到云端及从云端迁移

[](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----86462c6a8f97---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------) ·7 分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F86462c6a8f97&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97&user=Aashish+Nair&userId=3087ba81e065&source=-----86462c6a8f97---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F86462c6a8f97&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97&source=-----86462c6a8f97---------------------bookmark_footer-----------)![](../Images/206b4eb369a3a357dd723f699da2f412.png)

图片由 [Lia Trevarthen](https://unsplash.com/@melodi2?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Amazon 简单存储服务 (S3) 为用户提供了便宜、安全且易于管理的存储基础设施。虽然可以使用 AWS 控制台来迁移 S3 存储桶中的文件，但 AWS 还提供了使用代码来简化这些操作的选项。

对于 Python，AWS 软件开发工具包（SDK）提供了 boto3，允许用户通过代码创建、配置和使用 S3 桶和对象。在这里，我们*深入探讨*了用于 S3 资源的基本 boto3 函数，并考虑如何利用它们来自动化操作。

## 为什么选择 Boto3？

你可能会想，如果你已经可以通过 AWS 控制台访问 S3 服务，那么学习使用另一个工具是否有意义。诚然，使用 AWS 简单且用户友好的用户界面（UI），将文件从 S3 桶中移入移出是很容易的。

然而，当操作需要扩展时，使用典型的点击和拖拽方法是不可行的。处理 1-10 个文件是一回事，但你能应付 100 个或 1000 个文件吗？如果手动操作，这样的任务自然会耗时。
