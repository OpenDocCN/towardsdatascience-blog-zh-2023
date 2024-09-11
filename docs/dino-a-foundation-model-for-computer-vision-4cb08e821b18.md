# DINO — 一种计算机视觉的基础模型

> 原文：[https://towardsdatascience.com/dino-a-foundation-model-for-computer-vision-4cb08e821b18?source=collection_archive---------0-----------------------#2023-09-27](https://towardsdatascience.com/dino-a-foundation-model-for-computer-vision-4cb08e821b18?source=collection_archive---------0-----------------------#2023-09-27)

## [🚀Sascha的论文俱乐部](https://towardsdatascience.com/tagged/saschas-paper-club)

## 由 M. Caron 等人撰写的《自监督视觉变换器中的新兴特性》

[](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)[![Sascha Kirch](../Images/a0d45da9dc9c602075b2810786c660c9.png)](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5c38dace9d5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdino-a-foundation-model-for-computer-vision-4cb08e821b18&user=Sascha+Kirch&userId=5c38dace9d5e&source=post_page-5c38dace9d5e----4cb08e821b18---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------) · 13分钟阅读 · 2023年9月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4cb08e821b18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdino-a-foundation-model-for-computer-vision-4cb08e821b18&user=Sascha+Kirch&userId=5c38dace9d5e&source=-----4cb08e821b18---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4cb08e821b18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdino-a-foundation-model-for-computer-vision-4cb08e821b18&source=-----4cb08e821b18---------------------bookmark_footer-----------)

计算机视觉的这一十年令人兴奋。来自自然语言领域的巨大成功被转移到视觉领域，包括 ViT（视觉变换器）的引入，以及最近的大规模自监督预训练技术在基础模型的名下成为头条新闻。

今天我们将深入了解一个名为 DINO（自 **DI**蒸馏，**N**O 标签）的框架，这是一个基于 ViT（视觉变换器）有趣特性的视觉基础模型。它也是今天表现最佳的基础模型之一的前身：[DINOv2](https://arxiv.org/abs/2304.07193)。

![](../Images/826ce8cc3feae5497593efe2c4e9631c.png)

图片来源于 [出版物](https://arxiv.org/abs/2104.14294)，作者 [Sascha Kirch](https://medium.com/@SaschaKirch)

> **论文：** [自监督视觉变换器中的新兴特性](https://arxiv.org/abs/2104.14294)，作者 [Mathilde Caron](https://arxiv.org/search/cs?searchtype=author&query=Caron%2C+M) 等，2021 年 4 月 29 日
> 
> **资源：** [GitHub](https://github.com/facebookresearch/dino) — [博客文章](https://ai.meta.com/blog/dino-paws-computer-vision-with-self-supervised-transformers-and-10x-more-efficient-training/)
> 
> **类别：** 基础模型、计算机视觉、视觉变换器、知识蒸馏、相似性学习、自监督学习
> 
> [**其他讲解**](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2)**：**
> 
> [[BYOL](/byol-the-alternative-to-contrastive-self-supervised-learning-5d0a26983d7c?sk=fc5a3b3a556088181d8726226862252c)] — [[CLIP](/the-clip-foundation-model-7770858b487d?sk=a7b10ba1d0c3a20ecd4adb8200a48500)] — [[GLIP](/glip-introducing-language-image-pre-training-to-object-detection-5ddb601873aa?sk=4f0acb404a38d342b7669f861c013a05)] — [[Segment Anything](/segment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d?sk=bd1311a6d8b1e0e6d3369d536dba0700)] — [[DINO](/segment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d?sk=bd1311a6d8b1e0e6d3369d536dba0700)] — [[Depth Anything](https://medium.com/towards-data-science/depth-anything-a-foundation-model-for-monocular-depth-estimation-8a7920b5c9cc?sk=fc6197edd68e6137c3396c83e50f65cb)] — [[DDPM](/the-rise-of-diffusion-models-a-new-era-of-generative-deep-learning-3ef4779f6e1b?sk=8c178422a977c6f49ec24b13502be4fd)]

# 大纲

1.  背景与背景

1.  方法
