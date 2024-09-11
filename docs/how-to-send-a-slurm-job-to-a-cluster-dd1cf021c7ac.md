# 如何将 SLURM 任务发送到集群

> 原文：[`towardsdatascience.com/how-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac?source=collection_archive---------6-----------------------#2023-08-21`](https://towardsdatascience.com/how-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac?source=collection_archive---------6-----------------------#2023-08-21)

## 本教程讲解如何将 SLURM 任务发送到集群，特别是针对深度学习和数据科学

[](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)![François Porcher](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----dd1cf021c7ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e8e73046f53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=post_page-8e8e73046f53----dd1cf021c7ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd1cf021c7ac--------------------------------) ·8 分钟阅读·2023 年 8 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdd1cf021c7ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=-----dd1cf021c7ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdd1cf021c7ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-a-slurm-job-to-a-cluster-dd1cf021c7ac&source=-----dd1cf021c7ac---------------------bookmark_footer-----------)![](img/f7cbe849c11f034a957dfc2a26cf82fa.png)

图片由 [imgix](https://unsplash.com/@imgix?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所以你习惯于使用 Google Colab 的免费 GPU 训练深度学习模型，但现在你准备升级并利用集群的强大计算能力，却不知道怎么做？你来对地方了！🚀

在我剑桥大学神经科学研究实习期间，我正在训练用于计算机视觉任务的大型模型，而 Google 提供的免费 GPU 不足够，所以我决定使用本地集群。

然而，文档很少，我不得不向其他人询问脚本以尝试理解它们，并或多或少地汇总了对我有用的几项内容。现在我已经汇编了运行基本 Python 脚本所需的一切。这个指南是我希望当时能拥有的。

# 一个典型的机器学习用例

假设你想训练一个鸟类分类器，拥有 500 个不同的类别和高分辨率的图片。这种任务在 Google Colab 上几乎无法运行。

你需要做的第一件事是确保你的深度学习模型训练脚本已经准备好。这个脚本应该包含必要的代码……
