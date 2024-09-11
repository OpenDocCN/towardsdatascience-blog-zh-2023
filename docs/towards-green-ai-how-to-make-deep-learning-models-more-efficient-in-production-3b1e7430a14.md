# 面向绿色人工智能：如何提高深度学习模型在生产中的效率

> 原文：[`towardsdatascience.com/towards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14?source=collection_archive---------6-----------------------#2023-08-08`](https://towardsdatascience.com/towards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14?source=collection_archive---------6-----------------------#2023-08-08)

## Kaggle 蓝图

## 从学术界到工业界：在机器学习实践中找到预测性能与推断运行时间之间的最佳权衡，以实现可持续性

[](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----3b1e7430a14---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------) ·14 分钟阅读·2023 年 8 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b1e7430a14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----3b1e7430a14---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b1e7430a14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14&source=-----3b1e7430a14---------------------bookmark_footer-----------)![](img/2c688f3626d020d10afe04975cc8d466.png)

在 GPU 篝火晚会上制作 s’mEARTHs（图片由作者手绘）

*本文最初在* [*Kaggle 上发布*](https://www.kaggle.com/code/iamleonie/towards-green-ai) *，作为* [*“2023 Kaggle AI Report”比赛*](https://www.kaggle.com/competitions/2023-kaggle-ai-report) *的参赛作品，发布于 2023 年 7 月 5 日，其中* [*赢得了“ Kaggle 竞赛”类别的第一名*](https://www.kaggle.com/competitions/2023-kaggle-ai-report/discussion/429989)*。由于它回顾了 Kaggle 竞赛的报告，因此是“**The Kaggle Blueprints**”系列的特别版。*

# 介绍

“我认为我们正处于一个时代的终结，这个时代以这些巨大无比的模型为特征。[…] 我们会以其他方式让它们变得更好。”—— [萨姆·阿尔特曼](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/)，OpenAI 的首席执行官，在他们发布 GPT-4 后不久发表了这一声明。这一声明令许多人感到惊讶，因为 GPT-4 的规模估计比其前身 GPT-3 大十倍（[1.76 万亿参数](https://wandb.ai/byyoung3/ml-news/reports/AI-Expert-Speculates-on-GPT-4-Architecture---Vmlldzo0NzA0Nzg4)），而 GPT-3 有[1750 亿参数](https://arxiv.org/abs/2005.14165)。

> “我认为我们正处于一个时代的终结，这个时代以这些巨大无比的模型为特征。[…] 我们会以其他方式让它们变得更好。”—— [萨姆·阿尔特曼](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/)
