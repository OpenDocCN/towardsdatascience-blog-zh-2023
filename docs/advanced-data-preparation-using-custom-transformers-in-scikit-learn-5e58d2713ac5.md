# 使用自定义变换器进行高级数据准备

> 原文：[https://towardsdatascience.com/advanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5?source=collection_archive---------8-----------------------#2023-06-15](https://towardsdatascience.com/advanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5?source=collection_archive---------8-----------------------#2023-06-15)

## 超越“初学者模式”，充分利用 scikit-learn 的更强大功能

[](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)[![Matt Chapman](../Images/7511deb8d9ed408ece21031f6614c532.png)](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----5e58d2713ac5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----5e58d2713ac5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e58d2713ac5--------------------------------) ·10 min read·2023年6月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5e58d2713ac5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5&user=Matt+Chapman&userId=bf7d13fc53db&source=-----5e58d2713ac5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5e58d2713ac5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-data-preparation-using-custom-transformers-in-scikit-learn-5e58d2713ac5&source=-----5e58d2713ac5---------------------bookmark_footer-----------)![](../Images/2d8d8b06eb619180898fe4e29f587be9.png)

图片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung) 提供，来源于 [Unsplash](https://unsplash.com/photos/cPF2nlWcMY4)

Scikit-Learn 提供了许多用于数据准备的有用工具，但有时预构建的选项还不够。

在本文中，我将向你展示如何使用自定义 Transformers 创建高级数据准备工作流。如果你已经使用 scikit-learn 一段时间，并希望提升技能，学习 Transformers 是超越“初学者模式”并了解现代数据科学团队所需的更高级功能的绝佳方式。

如果主题听起来有些高级，不用担心——这篇文章包含了大量示例，将帮助你对代码和概念都充满信心。

我将从简要概述 scikit-learn 的 `Transformer` 类开始，然后介绍两种构建自定义 Transformers 的方法：

1.  使用 `FunctionTransformer`

1.  从零开始编写自定义 `Transformer`

# Transformers：在 scikit-learn 中预处理数据的最佳方式
