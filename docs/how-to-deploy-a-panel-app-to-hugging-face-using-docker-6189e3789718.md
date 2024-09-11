# 如何使用 Docker 将 Panel 应用程序部署到 Hugging Face

> 原文：[`towardsdatascience.com/how-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718?source=collection_archive---------10-----------------------#2023-01-06`](https://towardsdatascience.com/how-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718?source=collection_archive---------10-----------------------#2023-01-06)

## 5 个简单步骤

[](https://sophiamyang.medium.com/?source=post_page-----6189e3789718--------------------------------)![Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----6189e3789718--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6189e3789718--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6189e3789718--------------------------------) [Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----6189e3789718--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae9cae9cbcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=post_page-ae9cae9cbcd2----6189e3789718---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6189e3789718--------------------------------) ·5 分钟阅读·2023 年 1 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6189e3789718&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=-----6189e3789718---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6189e3789718&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718&source=-----6189e3789718---------------------bookmark_footer-----------)

作者：[Sophia Yang](https://twitter.com/sophiamyang) 和 [Marc Skov Madsen](https://twitter.com/MarcSkovMadsen)

你想将一个 Panel 应用程序部署到 Hugging Face 但不知道怎么做？通过五个简单的步骤，我们可以轻松地使用 Docker 将我们的 Panel 应用程序部署到 Hugging Face。本文将逐步指导你完成这个过程。文章结束时，你应该能够像下面所示那样在 Hugging Face 上部署你自己的应用程序。这非常简单，而且是免费的。你还在等什么？让我们开始吧！

![](img/37a77f74d355a73e70094a5b74a72734.png)

+   托管在 Hugging Face 的 Panel 应用: [`huggingface.co/spaces/sophiamyang/panel_example`](https://huggingface.co/spaces/sophiamyang/panel_example)

+   独立应用链接: [`sophiamyang-panel-example.hf.space/app`](https://sophiamyang-panel-example.hf.space/app)

# 将 Panel 应用部署到 Hugging Face 的五个步骤

## **步骤 1:** [`huggingface.co/spaces`](https://huggingface.co/spaces)

访问 [`huggingface.co/spaces`](https://huggingface.co/spaces) 并点击“创建新的 Space”
