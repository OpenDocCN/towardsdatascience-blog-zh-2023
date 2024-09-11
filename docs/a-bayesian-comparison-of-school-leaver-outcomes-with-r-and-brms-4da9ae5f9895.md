# 使用 R 和 brms 对学校毕业生结果的贝叶斯比较

> 原文：[`towardsdatascience.com/a-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895?source=collection_archive---------12-----------------------#2023-09-02`](https://towardsdatascience.com/a-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895?source=collection_archive---------12-----------------------#2023-09-02)

## ANOVA — 贝叶斯风格

[](https://mmgillin.medium.com/?source=post_page-----4da9ae5f9895--------------------------------)![Murray Gillin](https://mmgillin.medium.com/?source=post_page-----4da9ae5f9895--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4da9ae5f9895--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4da9ae5f9895--------------------------------) [Murray Gillin](https://mmgillin.medium.com/?source=post_page-----4da9ae5f9895--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa168322fa6bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895&user=Murray+Gillin&userId=a168322fa6bf&source=post_page-a168322fa6bf----4da9ae5f9895---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4da9ae5f9895--------------------------------) ·9 min read·2023 年 9 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4da9ae5f9895&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895&user=Murray+Gillin&userId=a168322fa6bf&source=-----4da9ae5f9895---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4da9ae5f9895&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-comparison-of-school-leaver-outcomes-with-r-and-brms-4da9ae5f9895&source=-----4da9ae5f9895---------------------bookmark_footer-----------)

我们在离开学校时常被要求考虑我们想做什么。从小孩时期开始，我们就被问到长大后想做什么，然后我们在接受了 13 年的教育后继续前行。在公共政策中，政府、天主教和独立学校系统之间的差异被广泛讨论，特别是涉及到非政府学校的公共资金以及资源分配的问题。

学校部门之间的学生成果是否有实际差异？

在澳大利亚维多利亚州，州政府每年进行一次调查，以评估高中后的结果（[On Track Survey](https://www.vic.gov.au/on-track-survey)）。该[数据集](https://www.education.vic.gov.au/Documents/about/research/OnTrack2022/DestinationData2022.xlsx)（根据[CC BY 4.0](https://discover.data.vic.gov.au/dataset/2021-on-track-survey-year-12-or-equivalent-completers-post-school-destinations)许可提供）涵盖了多个年份，我们将在本文编写时仅查看最近的可用年份，即 2021 年。

本文将应用贝叶斯分析方法，使用 R 包 brms 来回答这些问题。

![](img/6fc796bd31faf209865d27bb584ba25e.png)

图片来源于 [Good Free Photos](https://unsplash.com/@goodfreephoto_com?utm_source=medium&utm_medium=referral) 由 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

## 加载库和数据集

以下是我们加载所需的包，数据集以宽报告格式呈现，包含许多合并单元格。R 不喜欢这种格式，因此我们需要做一些硬编码，以重新标记向量并创建一个整洁的数据框。
