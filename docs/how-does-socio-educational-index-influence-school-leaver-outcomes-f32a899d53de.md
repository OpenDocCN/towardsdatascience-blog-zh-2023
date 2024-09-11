# 社会教育指数如何影响学校离开者的结果？— 使用 R 和 brms 进行的贝叶斯分析

> 原文：[`towardsdatascience.com/how-does-socio-educational-index-influence-school-leaver-outcomes-f32a899d53de?source=collection_archive---------8-----------------------#2023-09-12`](https://towardsdatascience.com/how-does-socio-educational-index-influence-school-leaver-outcomes-f32a899d53de?source=collection_archive---------8-----------------------#2023-09-12)

## ANCOVA — 贝叶斯风格

[](https://mmgillin.medium.com/?source=post_page-----f32a899d53de--------------------------------)![Murray Gillin](https://mmgillin.medium.com/?source=post_page-----f32a899d53de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f32a899d53de--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f32a899d53de--------------------------------) [Murray Gillin](https://mmgillin.medium.com/?source=post_page-----f32a899d53de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa168322fa6bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-socio-educational-index-influence-school-leaver-outcomes-f32a899d53de&user=Murray+Gillin&userId=a168322fa6bf&source=post_page-a168322fa6bf----f32a899d53de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f32a899d53de--------------------------------) · 11 分钟阅读·2023 年 9 月 12 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff32a899d53de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-socio-educational-index-influence-school-leaver-outcomes-f32a899d53de&source=-----f32a899d53de---------------------bookmark_footer-----------)

在**上一篇文章**获得非常积极的反响后，我们继续探讨是否存在学校部门和高等教育结果之间的比例差异。我们指出，这种建模并非旨在进行因果分析，仅仅是描述性的。我们还提到，因果模型中可能涉及许多因素。实际上，存在一个代理测量指标，即 ICSEA 或社区社会教育优势指数。

[](/a-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895?source=post_page-----f32a899d53de--------------------------------) ## 使用 R 和 brms 的贝叶斯学校离校生结果比较

### ANOVA — 贝叶斯风格

towardsdatascience.com

延续上一篇文章，我们将探讨 ICSEA 在各个部门比例结果之间是否存在因果关系，这将涉及贝叶斯 ANCOVA 工作流。

![](img/f7395b0c3632dc63d9fdacc99f2322cd.png)

图片由[Vasily Koloda](https://unsplash.com/@napr0tiv?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 背景

在开始建模之前，让我们首先开发我们的因果模型和理解。[ICSEA](https://www.myschool.edu.au/faqs#13)是一个衡量学校社会教育优势的指标。这个指标是一个计算出来的测量值…
