# 使用 Spotify 的 Pedalboard 进行自然音频数据增强

> 原文：[https://towardsdatascience.com/natural-audio-data-augmentation-using-spotifys-pedalboard-212ea59d39ce?source=collection_archive---------12-----------------------#2023-01-09](https://towardsdatascience.com/natural-audio-data-augmentation-using-spotifys-pedalboard-212ea59d39ce?source=collection_archive---------12-----------------------#2023-01-09)

## 带有现成的 Python 代码和预设

[](https://medium.com/@maxhilsdorf?source=post_page-----212ea59d39ce--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----212ea59d39ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----212ea59d39ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----212ea59d39ce--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----212ea59d39ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-audio-data-augmentation-using-spotifys-pedalboard-212ea59d39ce&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----212ea59d39ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----212ea59d39ce--------------------------------) ·10分钟阅读·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F212ea59d39ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-audio-data-augmentation-using-spotifys-pedalboard-212ea59d39ce&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----212ea59d39ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F212ea59d39ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-audio-data-augmentation-using-spotifys-pedalboard-212ea59d39ce&source=-----212ea59d39ce---------------------bookmark_footer-----------)![](../Images/66165831ed554a6df7e20476f30856b8.png)

图像改编自[**戸山 神奈**](https://unsplash.com/de/fotos/Qi32ATTSwp4)**.**

作为数据科学家，处理音频数据往往是一项挑战。一个常见的问题是**数据不足**，难以建立有效的模型来完成语音识别、音乐推荐和语音转文本等任务。音频数据**通常是敏感的**，可能包含人声或受版权保护的音乐。最后，音频数据的维度很高，通常需要**巨大的计算资源**，与其他数据类型相比。因此，充分利用现有数据至关重要。音频数据增强是一种很好的方法来实现这一点！

在本文中，我们将涵盖以下主题：

1.  **（音频）数据增强基础**

1.  **Spotify Pedalboard 介绍**

1.  **音频数据增强的最佳实践**

1.  **高级使用方法与现成代码及预设**

# 数据增强

数据增强是通过对数据集中某些或所有示例进行轻微修改，以增加训练数据总量的过程。如果操作得当，增强数据集可以有效地**防止过拟合**。这意味着即使在数据量相对较少的情况下，你的机器学习模型也可能产生更多**可推广的结果**。
