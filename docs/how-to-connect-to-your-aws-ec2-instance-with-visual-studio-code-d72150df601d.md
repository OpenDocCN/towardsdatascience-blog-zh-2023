# 如何使用 Visual Studio Code 连接到你的 AWS EC2 实例

> 原文：[`towardsdatascience.com/how-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d?source=collection_archive---------1-----------------------#2023-04-27`](https://towardsdatascience.com/how-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d?source=collection_archive---------1-----------------------#2023-04-27)

## 使用 VS Code 访问远程服务器的快速简便指南

[](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----d72150df601d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----d72150df601d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d72150df601d--------------------------------) ·7 分钟阅读·2023 年 4 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd72150df601d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d&user=Aashish+Nair&userId=3087ba81e065&source=-----d72150df601d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd72150df601d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-your-aws-ec2-instance-with-visual-studio-code-d72150df601d&source=-----d72150df601d---------------------bookmark_footer-----------)![](img/31f029a0a26bb457b3ee2006cf821363.png)

图片来源 [John Barkiple](https://unsplash.com/@barkiple?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

Visual Studio Code 毫无疑问是我最喜欢的 IDE。它简洁、直观的用户界面使其成为我进行数据科学项目的首选编辑器。

实际上，它非常出色，以至于即使在远程服务器上工作时，我也不愿意放弃它。

这有问题吗？不，一点也没有！

毕竟，Visual Studio Code 可以配置为允许用户在远程服务器上工作。最棒的是，这只需不到 10 分钟的时间！

这对于需要计算或存储资源超出本地机器提供能力的项目非常有用。当我需要处理大型数据集时，我常常依赖这个功能。

如果你处于类似的情况，并希望在享受 Visual Studio Code 的舒适环境的同时处理大数据或训练深度学习模型，本教程将向你展示如何使用这个 IDE 连接到你的 AWS EC2。

## 案例研究

作为教程的一部分，我们将把一个包含两个文件的文件夹移动到 EC2 实例：一个名为 `random_number.py` 的 Python 文件和一个包含所有依赖的 `requirements.txt` 文件。
