# 新的 DeepMind 研究揭示了语言模型的终极提示种子

> 原文：[`towardsdatascience.com/new-deepmind-work-unveils-supreme-prompt-seeds-for-language-models-e95fb7f4903c?source=collection_archive---------0-----------------------#2023-11-08`](https://towardsdatascience.com/new-deepmind-work-unveils-supreme-prompt-seeds-for-language-models-e95fb7f4903c?source=collection_archive---------0-----------------------#2023-11-08)

## 计算优化提示如何使语言模型表现出色，以及这一切如何影响提示工程

[](https://lucianosphere.medium.com/?source=post_page-----e95fb7f4903c--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----e95fb7f4903c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e95fb7f4903c--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----e95fb7f4903c--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----e95fb7f4903c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-deepmind-work-unveils-supreme-prompt-seeds-for-language-models-e95fb7f4903c&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----e95fb7f4903c---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----e95fb7f4903c--------------------------------) · 11 分钟阅读 · 2023 年 11 月 8 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe95fb7f4903c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-deepmind-work-unveils-supreme-prompt-seeds-for-language-models-e95fb7f4903c&source=-----e95fb7f4903c---------------------bookmark_footer-----------)![](img/1fb04d056a98fae3386085e529a642a9.png)

图片由 [Ali Shah Lakhani](https://unsplash.com/@alishahlakhani?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着我们见证人工智能（AI）稳定的进步，每个月都在完成越来越困难的任务，普遍存在对我们劳动力未来的担忧。如果 AI 继续自动化许多人类目前执行的任务，那么未来的职业将会是什么样的？有一种观点认为“*编程这些系统将是人类的工作多年*”，或者“*我们总是需要人类来维护和重新训练 AI 模型*”，或者“*制定高效的提示，以引导 AI 模型朝正确方向发展是一项人类技能*”。后者，也就是本文的重点，激发了“提示工程”作为一种“职业”的产生。确实，编写高效提示以让 AI 模型完全按预期工作，或让它“思考”得足够好以改善对问题的回答，确实需要一定的技巧。请看这个，作为一个例子：
