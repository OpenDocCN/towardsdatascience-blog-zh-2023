# RLHF：来自人类反馈的强化学习

> 原文：[`towardsdatascience.com/rlhf-reinforcement-learning-from-human-feedback-faa5ff4761d1?source=collection_archive---------0-----------------------#2023-10-11`](https://towardsdatascience.com/rlhf-reinforcement-learning-from-human-feedback-faa5ff4761d1?source=collection_archive---------0-----------------------#2023-10-11)

## ChatGPT 的成功秘诀：指令数据。

[](https://automata88.medium.com/?source=post_page-----faa5ff4761d1--------------------------------)![Ms Aerin](https://automata88.medium.com/?source=post_page-----faa5ff4761d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----faa5ff4761d1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----faa5ff4761d1--------------------------------) [Ms Aerin](https://automata88.medium.com/?source=post_page-----faa5ff4761d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d8994ad0efc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frlhf-reinforcement-learning-from-human-feedback-faa5ff4761d1&user=Ms+Aerin&userId=1d8994ad0efc&source=post_page-1d8994ad0efc----faa5ff4761d1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----faa5ff4761d1--------------------------------) ·24 min read·2023 年 10 月 11 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffaa5ff4761d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frlhf-reinforcement-learning-from-human-feedback-faa5ff4761d1&user=Ms+Aerin&userId=1d8994ad0efc&source=-----faa5ff4761d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffaa5ff4761d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frlhf-reinforcement-learning-from-human-feedback-faa5ff4761d1&source=-----faa5ff4761d1---------------------bookmark_footer-----------)

ChatGPT 凭借其令人印象深刻的能力吸引了全世界的关注。但它是如何变得如此聪明的呢？

我最近和一位我非常尊敬的前同事——一位软件工程师——交谈，我注意到他认为 ChatGPT 是 AGI 的体现，并指出其将复杂话题简化到六岁儿童理解水平的能力作为证据。虽然我并不完全不同意他对其不合理智能的看法，但我觉得有必要表达一下我的想法。在这篇文章中，我想强调 ChatGPT 的魔力在于它的训练数据。

精心策划的指令数据是 ChatGPT 类人能力的关键。像是向 6 岁小孩解释概念、将简历转换为 LinkedIn 个人资料、与您头脑风暴等，这些能力并非偶然出现——它们被刻意地以训练数据的形式编码进模型中。

和其他人一样，这是我第一次体验封闭式研究。自大学以来，所有前沿研究都是公开的，并经过同行评审，直到最近。我相信开放性最终比…更能推动科学进步。
