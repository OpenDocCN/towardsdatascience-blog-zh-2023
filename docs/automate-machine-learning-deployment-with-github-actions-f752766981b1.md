# 使用 GitHub Actions 自动化机器学习部署

> 原文：[https://towardsdatascience.com/automate-machine-learning-deployment-with-github-actions-f752766981b1?source=collection_archive---------1-----------------------#2023-04-16](https://towardsdatascience.com/automate-machine-learning-deployment-with-github-actions-f752766981b1?source=collection_archive---------1-----------------------#2023-04-16)

## 更快的市场响应时间和提高效率

[](https://khuyentran1476.medium.com/?source=post_page-----f752766981b1--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----f752766981b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f752766981b1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f752766981b1--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----f752766981b1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomate-machine-learning-deployment-with-github-actions-f752766981b1&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----f752766981b1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f752766981b1--------------------------------) · 8分钟阅读 · 2023年4月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff752766981b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomate-machine-learning-deployment-with-github-actions-f752766981b1&user=Khuyen+Tran&userId=84a02493194a&source=-----f752766981b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff752766981b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomate-machine-learning-deployment-with-github-actions-f752766981b1&source=-----f752766981b1---------------------bookmark_footer-----------)

# 动机

考虑以下场景：每月都会开发出更准确的机器学习模型并添加到主分支中。

![](../Images/ccdff6e6a105e25ad475d5c870eebe0d.png)

图片来源：作者

要部署模型，你必须将其下载到你的机器上，进行打包，然后进行部署。

![](../Images/23f375e6391f79e8a12f898563401bb6.png)

图片来源：作者

然而，由于你可能有其他责任，完成部署可能需要几天甚至几周，这会拖慢发布进程，并占用本可以用于其他任务的宝贵时间。

![](../Images/1d7da9399f4687046772d64aeaefddd1.png)

图片来源：作者

如果模型能够在每次新版本推送到主分支时自动部署到生产环境，那岂不是太好了？这就是…
