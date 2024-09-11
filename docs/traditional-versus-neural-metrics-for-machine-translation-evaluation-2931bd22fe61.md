# 传统与神经网络指标在机器翻译评估中的对比

> 原文：[`towardsdatascience.com/traditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61?source=collection_archive---------5-----------------------#2023-03-09`](https://towardsdatascience.com/traditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61?source=collection_archive---------5-----------------------#2023-03-09)

## 自 2010 年以来新增了 100 多个指标

[](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----2931bd22fe61---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------) ·15 min read·2023 年 3 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2931bd22fe61&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61&user=Benjamin+Marie&userId=ad2a414578b3&source=-----2931bd22fe61---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2931bd22fe61&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61&source=-----2931bd22fe61---------------------bookmark_footer-----------)![](img/6177bf23f91d84b81a888a899af3c75e.png)

图片来自 [Pixabay](https://pixabay.com/photos/tunnel-light-grim-dark-art-6786462/)

使用自动化指标进行评估具有**更快、更可重复、成本更低**的优点，相较于人工评估。

这对于机器翻译的评估尤其如此。对于人工评估，我们理想情况下需要专家翻译员。

对于许多语言对来说，这些专家**极其稀少且难以雇佣**。

由于机器翻译的研究领域非常动态，需要对新系统进行大规模且快速的人工评估，这往往是不切实际的。

因此，机器翻译的自动评估在过去 20 多年里一直是**非常活跃且富有成效的研究领域**。

尽管 BLEU 仍然是最常用的评估指标，但有无数更好的替代方案。

[## BLEU: 一个被误解的指标](https://towardsdatascience.com/bleu-a-misunderstood-metric-from-another-age-d434e18f1b37?source=post_page-----2931bd22fe61--------------------------------)

### 但在今天的 AI 研究中仍然被使用

towardsdatascience.com

自 2010 年以来，已经提出了 100 多种自动化指标来改进机器翻译评估。
