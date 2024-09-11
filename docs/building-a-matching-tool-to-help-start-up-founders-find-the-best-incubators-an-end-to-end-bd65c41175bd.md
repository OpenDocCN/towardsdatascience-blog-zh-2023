# 帮助初创公司创始人找到最佳孵化器：一个端到端的项目。

> 原文：[`towardsdatascience.com/building-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd?source=collection_archive---------5-----------------------#2023-11-26`](https://towardsdatascience.com/building-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd?source=collection_archive---------5-----------------------#2023-11-26)

## 一个自由职业项目的逐步讲解，旨在为初创公司创始人推荐最佳孵化器，使用了 Python、Pinecone、FastAPI、Pydantic 和 Docker

[](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)![Jeremy Arancio](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------) [Jeremy Arancio](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7a4c4019f28e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd&user=Jeremy+Arancio&userId=7a4c4019f28e&source=post_page-7a4c4019f28e----bd65c41175bd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------) ·15 分钟阅读·2023 年 11 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd65c41175bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd&user=Jeremy+Arancio&userId=7a4c4019f28e&source=-----bd65c41175bd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd65c41175bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd&source=-----bd65c41175bd---------------------bookmark_footer-----------)

[Harness](https://www.joinharness.com/)，一家致力于帮助创始人创业的初创公司，找到了我，希望我开发一个工具，帮助他们的社区找到最合适的孵化器：**匹配工具**。

在本文中，我们将逐步讲解该项目的不同阶段，从解决方案设计到交付。

![](img/2488d4d84449aeb3c7adad531d8346bf.png)

图片由 [Rames Quinerie](https://unsplash.com/@ramesquinerie?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

公司及其联合创始人希望创建一个工具，使他们的初创企业创始人社区能够找到全球最佳的孵化器和加速器。

为此，他们手动收集了来自孵化器网站的数据，包括位置、各种要求、资金机会等详细信息。此外，他们还利用了一个积极参与的创始人社区。

通过孵化器和他们的社区提供的数据，他们需要找到一种方法来检索基于初创公司信息的**顶级孵化器**。

挑战接受。

# 解决方案设计
