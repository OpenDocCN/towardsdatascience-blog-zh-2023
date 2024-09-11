# 停止在数据科学项目中硬编码——改用配置文件

> 原文：[https://towardsdatascience.com/stop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f?source=collection_archive---------0-----------------------#2023-05-26](https://towardsdatascience.com/stop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f?source=collection_archive---------0-----------------------#2023-05-26)

## 如何高效地在Python中与配置文件交互

[](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----479ac8ffc76f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------) ·6分钟阅读·2023年5月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F479ac8ffc76f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f&user=Khuyen+Tran&userId=84a02493194a&source=-----479ac8ffc76f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F479ac8ffc76f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f&source=-----479ac8ffc76f---------------------bookmark_footer-----------)![](../Images/d8f861a64c002d8fcc6f28868a2a7e65.png)

作者提供的图片

*最初发表于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/05/25/stop-hard-coding-in-a-data-science-project-use-configuration-files-instead/) *2023年5月26日。*

# 问题

在你的数据科学项目中，某些值往往会频繁变化，例如文件名、选择的特征、训练-测试分割比例以及模型的超参数。

![](../Images/8095b45a4fe0e495526e4a29a2bea6ff.png)

作者提供的图片

在编写用于假设测试或演示目的的临时代码时，硬编码这些值是可以的。然而，随着代码库和团队的扩展，避免硬编码变得至关重要，因为这可能会引发各种问题：

+   **可维护性**：如果值在代码库中分散，保持一致性更新变得更困难。当值需要更新时，这可能导致错误或不一致。
