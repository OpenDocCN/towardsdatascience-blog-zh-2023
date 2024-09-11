# Python 中的音频数据增强技术

> 原文：[https://towardsdatascience.com/data-augmentation-techniques-for-audio-data-in-python-15505483c63c?source=collection_archive---------4-----------------------#2023-03-28](https://towardsdatascience.com/data-augmentation-techniques-for-audio-data-in-python-15505483c63c?source=collection_archive---------4-----------------------#2023-03-28)

## 如何使用 librosa、numpy 和 PyTorch 在波形（时间域）和频谱图（频率域）中增强音频

[](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-augmentation-techniques-for-audio-data-in-python-15505483c63c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----15505483c63c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------) · 7分钟阅读·2023年3月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15505483c63c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-augmentation-techniques-for-audio-data-in-python-15505483c63c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----15505483c63c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15505483c63c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-augmentation-techniques-for-audio-data-in-python-15505483c63c&source=-----15505483c63c---------------------bookmark_footer-----------)![](../Images/09de7dc6b33b345123ccb71209ef5b2b.png)

（由作者绘制的图像）

深度学习模型对数据的需求很大。如果你没有足够的数据，从现有数据集中生成合成数据可以帮助提高你的深度学习模型的泛化能力。虽然你可能已经对图像的数据增强技术（例如，水平翻转图像）有所了解，但音频数据的数据增强技术却不那么为人所知。

[](/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=post_page-----15505483c63c--------------------------------) [## 使用 Python 进行深度学习的音频分类

### 使用 PyTorch 和 torchaudio 调整图像模型以应对领域迁移和类别不平衡问题

towardsdatascience.com](/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=post_page-----15505483c63c--------------------------------)

本文将回顾音频数据的常见数据增强技术。你可以在波形和频谱图中应用音频数据增强：

+   [波形的音频数据增强](#d071)

    ∘ [噪声注入](#bd92)

    ∘ [时间偏移](#4f84)

    ∘ [速度变化](#f531)

    ∘ [音调变化](#76d6)

    ∘ [音量变化（不推荐）](#c212)

+   [频谱图的音频数据增强](#f7f8)

    ∘ [Mixup](#6c42)

    ∘ [SpecAugment](#23ca)
