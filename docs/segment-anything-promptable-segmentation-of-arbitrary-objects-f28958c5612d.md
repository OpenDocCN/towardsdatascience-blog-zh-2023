# Segment Anything: Promptable Segmentation of Arbitrary Objects

> 原文：[`towardsdatascience.com/segment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d?source=collection_archive---------1-----------------------#2023-09-14`](https://towardsdatascience.com/segment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d?source=collection_archive---------1-----------------------#2023-09-14)

## [🚀Sascha’s Paper Club](https://towardsdatascience.com/tagged/saschas-paper-club)

## **Segment Anything** 由 A. Krillov 等人撰写。

[](https://medium.com/@SaschaKirch?source=post_page-----f28958c5612d--------------------------------)![Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----f28958c5612d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f28958c5612d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f28958c5612d--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----f28958c5612d--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5c38dace9d5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d&user=Sascha+Kirch&userId=5c38dace9d5e&source=post_page-5c38dace9d5e----f28958c5612d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f28958c5612d--------------------------------) ·12 min read·2023 年 9 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff28958c5612d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d&user=Sascha+Kirch&userId=5c38dace9d5e&source=-----f28958c5612d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff28958c5612d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d&source=-----f28958c5612d---------------------bookmark_footer-----------)

今天的论文讲解将会非常直观！我们将深入分析**Segment Anything**，这是 Meta AI 研究团队的一篇论文，该论文不仅在研究界引起了轰动，也引起了各类深度学习从业者和支持者的关注。

Segment Anything 引入了可提示分割的任务，介绍了 Segment Anything 模型（SAM），并详细描述了生成一个包含超过 10 亿个掩膜的 1100 万张图像的新公开数据集。SAM 已被社区广泛采用，并产生了一些新的最先进的基础模型，如 [Grounded-SAM](https://github.com/IDEA-Research/Grounded-Segment-Anything)，它将 [Grounding DINO](https://arxiv.org/abs/2303.05499) 与 SAM 结合。

![](img/3655fd5fedea80f78ac89299c8bd7b30.png)

图片来自 [出版物](https://arxiv.org/abs/2304.02643) 由 [Sascha Kirch](https://medium.com/@SaschaKirch) 创作

> **论文:** [Segment Anything](https://arxiv.org/abs/2304.02643) 由 [Alexander Kirillov](https://arxiv.org/search/cs?searchtype=author&query=Kirillov%2C+A) 等人，2023 年 4 月 5 日
> 
> **资源:** [GitHub](https://github.com/facebookresearch/segment-anything) — [演示](https://segment-anything.com/demo) — [项目页面](https://segment-anything.com/) — [数据集](https://segment-anything.com/dataset/index.html) — [HuggingFace](https://huggingface.co/docs/transformers/main/model_doc/sam)
> 
> **类别:** 分割，零样本预测，计算机视觉，提示，大规模
> 
> [**其他讲解**](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2)**:**
> 
> [BYOL] — [CLIP] — [GLIP] — [[Depth Anything](https://medium.com/towards-data-science/depth-anything-a-foundation-model-for-monocular-depth-estimation-8a7920b5c9cc?sk=fc6197edd68e6137c3396c83e50f65cb)] — [DINO] — [DDPM]

# 大纲

1.  背景与上下文

1.  SAM — Segment Anything 模型
