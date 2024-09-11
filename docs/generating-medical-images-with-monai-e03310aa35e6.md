# 使用 MONAI 生成医学图像

> 原文：[`towardsdatascience.com/generating-medical-images-with-monai-e03310aa35e6?source=collection_archive---------5-----------------------#2023-04-14`](https://towardsdatascience.com/generating-medical-images-with-monai-e03310aa35e6?source=collection_archive---------5-----------------------#2023-04-14)

## *一个端到端的开源项目，利用最新的 MONAI 生成模型从放射学报告文本中生成胸部 X 光图像*

[](https://medium.com/@walhugolp?source=post_page-----e03310aa35e6--------------------------------)![Walter Hugo Lopez Pinaya](https://medium.com/@walhugolp?source=post_page-----e03310aa35e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e03310aa35e6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e03310aa35e6--------------------------------) [Walter Hugo Lopez Pinaya](https://medium.com/@walhugolp?source=post_page-----e03310aa35e6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa1dadbc02295&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-medical-images-with-monai-e03310aa35e6&user=Walter+Hugo+Lopez+Pinaya&userId=a1dadbc02295&source=post_page-a1dadbc02295----e03310aa35e6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e03310aa35e6--------------------------------) · 14 分钟阅读 · 2023 年 4 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe03310aa35e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-medical-images-with-monai-e03310aa35e6&user=Walter+Hugo+Lopez+Pinaya&userId=a1dadbc02295&source=-----e03310aa35e6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe03310aa35e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-medical-images-with-monai-e03310aa35e6&source=-----e03310aa35e6---------------------bookmark_footer-----------)

大家好！在这篇文章中，我们将创建一个潜在扩散模型，以使用 MONAI 的新开源扩展 [**MONAI Generative Models**](https://github.com/Project-MONAI/GenerativeModels) 生成胸部 X 光图像！

![](img/51dca716335eeca38fff769dc85c3648.png)

# **简介**

生成性人工智能在医疗保健领域具有巨大的潜力，因为它允许我们创建能够学习训练数据集的潜在模式和结构的模型。这样，我们可以利用这些生成模型创建无限量的合成数据，具有真实数据的相同细节和特征，但没有它们的限制。鉴于其重要性，我们创建了***MONAI 生成模型*，这是一个** [**MONAI 平台**](https://monai.io/)的开源扩展，包含最新的模型（如扩散模型、自回归变换器和生成对抗网络）以及帮助训练和评估生成模型的组件。

![](img/c5d4f6f58dff9be2230f424112fd52a4.png)

在这篇文章中，我们将详细讲解一个完整的项目，以创建一个潜在扩散模型（与…相同）。
