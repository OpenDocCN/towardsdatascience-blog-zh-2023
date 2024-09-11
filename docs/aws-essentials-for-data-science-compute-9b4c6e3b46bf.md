# AWS 数据科学基础：计算

> 原文：[`towardsdatascience.com/aws-essentials-for-data-science-compute-9b4c6e3b46bf?source=collection_archive---------14-----------------------#2023-01-03`](https://towardsdatascience.com/aws-essentials-for-data-science-compute-9b4c6e3b46bf?source=collection_archive---------14-----------------------#2023-01-03)

## 理解和部署 EC2 和 Lambda 服务

[](https://mgsosna.medium.com/?source=post_page-----9b4c6e3b46bf--------------------------------)![Matt Sosna](https://mgsosna.medium.com/?source=post_page-----9b4c6e3b46bf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b4c6e3b46bf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b4c6e3b46bf--------------------------------) [Matt Sosna](https://mgsosna.medium.com/?source=post_page-----9b4c6e3b46bf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff17fb22b897&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faws-essentials-for-data-science-compute-9b4c6e3b46bf&user=Matt+Sosna&userId=f17fb22b897&source=post_page-f17fb22b897----9b4c6e3b46bf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b4c6e3b46bf--------------------------------) ·18 分钟阅读·2023 年 1 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9b4c6e3b46bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faws-essentials-for-data-science-compute-9b4c6e3b46bf&user=Matt+Sosna&userId=f17fb22b897&source=-----9b4c6e3b46bf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b4c6e3b46bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faws-essentials-for-data-science-compute-9b4c6e3b46bf&source=-----9b4c6e3b46bf---------------------bookmark_footer-----------)![](img/7452510321621511a738d6a39304a5d4.png)

图片由 [Nick Owuor (astro.nic.visuals)](https://unsplash.com/@astro_nic25?utm_source=medium&utm_medium=referral) 提供，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

假设你已经开发了一个很酷的应用程序，并希望向全世界展示。也许这是一个可以生成 [涂鸦猫图像](https://affinelayer.com/pixsrv/) 的 AI，一个 [病毒式 LinkedIn 帖子生成器](https://viralpostgenerator.com)，或者一个 [英文到 RegEx 的翻译器](https://www.autoregex.xyz/)。你希望用户只需点击一个链接，即可立即开始与应用程序互动，而不是需要下载并在他们的计算机上运行它。

这种“即时互动”将需要一个**服务器**，它接收用户请求（例如，猫的涂鸦）并*提供*响应（例如，AI 生成的猫图像）。你*可以*使用个人笔记本电脑，但它在进入睡眠状态或关闭时会停止服务请求，且一个复杂的黑客可能会偷取你的私人数据。更糟糕的是，如果你的电脑尝试同时处理过多请求，你的硬盘可能会过热损坏！

除非你喜欢被黑客攻击、硬盘熔化的笔记本电脑，否则你可能想要从云端租用一个服务器。虽然你通过不直接访问物理机器而牺牲了一些控制权，但你可以抽象掉很多你可能不愿意处理的配置和维护工作。如果你愿意多花点钱，你可以轻松租用一个或几个比你的笔记本电脑强大得多的机器。那么我们怎么开始呢？
