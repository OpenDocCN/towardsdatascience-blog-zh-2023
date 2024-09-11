# 《逐步解决四个现实生活中的问题：使用 Transformers 和 Hugging Face 的完整指南》

> 原文：[https://towardsdatascience.com/4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d?source=collection_archive---------17-----------------------#2023-01-31](https://towardsdatascience.com/4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d?source=collection_archive---------17-----------------------#2023-01-31)

## 理解 Transformers 并利用它们的强大功能在 Python 中解决现实生活中的问题

[](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----e45fe698cc4d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------) ·11分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe45fe698cc4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d&user=Zoumana+Keita&userId=e6ae785a30d&source=-----e45fe698cc4d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe45fe698cc4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d&source=-----e45fe698cc4d---------------------bookmark_footer-----------)![](../Images/429c05a1b2db762b98d5755b81d78326.png)

图片由 [Aditya Vyas](https://unsplash.com/@aditya1702) 提供，来源于 [Unsplash](https://unsplash.com/photos/B9MULm2UZIk)

# 介绍

在自然语言处理（NLP）领域，研究人员在过去几十年中做出了重要贡献，带来了各个领域的创新进展。以下是 NLP 实践中的一些例子：

+   **Siri**，苹果公司开发的个人助理，可以帮助用户完成设置闹钟、发送短信和回答问题等任务。

+   在**医疗领域**，NLP 正被用来加速药物发现。

+   此外，NLP 还通过**翻译**来弥合语言障碍。

本文的目的是讨论变换器，这是一种在自然语言处理中的极其强大的模型。文章将首先强调变换器相对于循环神经网络的优势，以加深你对该模型的理解。接着，将提供在实际场景中使用 Huggingface 变换器的实际例子。

# 循环网络——变换器之前的辉煌时代
