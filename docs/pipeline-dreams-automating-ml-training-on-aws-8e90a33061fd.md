# 管道梦想：在 AWS 上自动化 ML 训练

> 原文：[https://towardsdatascience.com/pipeline-dreams-automating-ml-training-on-aws-8e90a33061fd?source=collection_archive---------10-----------------------#2023-10-25](https://towardsdatascience.com/pipeline-dreams-automating-ml-training-on-aws-8e90a33061fd?source=collection_archive---------10-----------------------#2023-10-25)

[](https://medium.com/@raicik.zach?source=post_page-----8e90a33061fd--------------------------------)[![Zachary Raicik](../Images/860760b53fcc75013007067190e8ca65.png)](https://medium.com/@raicik.zach?source=post_page-----8e90a33061fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e90a33061fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e90a33061fd--------------------------------) [Zachary Raicik](https://medium.com/@raicik.zach?source=post_page-----8e90a33061fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28b350f36c59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpipeline-dreams-automating-ml-training-on-aws-8e90a33061fd&user=Zachary+Raicik&userId=28b350f36c59&source=post_page-28b350f36c59----8e90a33061fd---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8e90a33061fd--------------------------------) ·11分钟阅读·2023年10月25日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e90a33061fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpipeline-dreams-automating-ml-training-on-aws-8e90a33061fd&source=-----8e90a33061fd---------------------bookmark_footer-----------)![](../Images/c696ca663854fee564e07b9749427f82.png)

图片由 [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在机器学习的世界中，自动化训练管道简化了从数据到洞察的过程。它们自动化了机器学习生命周期的各个部分，如数据摄取、预处理、模型训练、评估和部署。亚马逊网络服务（“AWS”）提供了多种工具来开发自动化训练管道。本文将引导你通过使用经典的 iris 数据集来设置一个基本的自动化训练管道。

# 设置舞台：需求和 AWS 工具包

在本节中，我们将涵盖一些高级要求以及我们将使用的 AWS 工具的简要概述。

## 要求

如果你选择通过建立自己的训练管道进行跟随，你将需要以下内容。

+   一个活跃的 AWS 账户（你可以在[这里](https://aws.amazon.com/free/?trk=78b916d7-7c94-4cab-98d9-0ce5e648dd5f&sc_channel=ps&ef_id=CjwKCAjws9ipBhB1EiwAccEi1K7ugfy2KiyndD6pN_A9_pVTNfC-K3Xp41WNOcLEEDoPj1Lyu_7WHRoCtjEQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21432339156165%21e%21%21g%21%21aws+account%219572385111%21102212379047) 注册）并拥有管理员权限

+   基本的**AWS CLI**知识（我们将在未来的帖子中探讨 AWS CLI 的替代方案）

设置你的 AWS 账户并通过 CLI 连接到 AWS 超出了本文的范围，不过如果你需要帮助，请随时直接联系我。

## 工具包
