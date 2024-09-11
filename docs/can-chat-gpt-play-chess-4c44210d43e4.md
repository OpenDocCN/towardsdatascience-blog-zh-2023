# Chat GPT 能下棋吗？

> 原文：[`towardsdatascience.com/can-chat-gpt-play-chess-4c44210d43e4?source=collection_archive---------8-----------------------#2023-12-18`](https://towardsdatascience.com/can-chat-gpt-play-chess-4c44210d43e4?source=collection_archive---------8-----------------------#2023-12-18)

## 在棋局中击败 GPT3.5，使用多策略 AI 与深度强化学习

[](https://octaviobomfim.medium.com/?source=post_page-----4c44210d43e4--------------------------------)![Octavio Santiago](https://octaviobomfim.medium.com/?source=post_page-----4c44210d43e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c44210d43e4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c44210d43e4--------------------------------) [Octavio Santiago](https://octaviobomfim.medium.com/?source=post_page-----4c44210d43e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f595b85e4c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chat-gpt-play-chess-4c44210d43e4&user=Octavio+Santiago&userId=6f595b85e4c9&source=post_page-6f595b85e4c9----4c44210d43e4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c44210d43e4--------------------------------) · 5 分钟阅读 · 2023 年 12 月 18 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c44210d43e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chat-gpt-play-chess-4c44210d43e4&user=Octavio+Santiago&userId=6f595b85e4c9&source=-----4c44210d43e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c44210d43e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chat-gpt-play-chess-4c44210d43e4&source=-----4c44210d43e4---------------------bookmark_footer-----------)![](img/f33a7eb2f5450e8975ca20364bceaaab.png)

作者提供的图像：使用 DALLE-3 生成的机器人下棋

2021 年，我开发了一个人工智能模型，采用多种策略和深度强化学习，成为一个现实且专业的棋手，并且我将这个 AI 机器人与许多不同的机器人进行了测试。您可以在下面的文章中获得有关这次经历的更多细节：

[](/hacking-chess-with-decision-making-deep-reinforcement-learning-173ed32cf503?source=post_page-----4c44210d43e4--------------------------------) ## 破解棋局：利用决策制定的深度强化学习

### 使用机器学习创建棋类冠军

[towardsdatascience.com

2023 年我正在使用相同的 AI 来测试 ChatGPT 是否能下棋，以及在实际对局中表现如何。

在开始之前，首先要问 ChatGPT 是否能下棋。ChatGPT 说它能下棋，但不是一个完美的棋手（这算不算是输棋的借口呢？）。

![](img/2d87bbaccdc586b2691d7ef5abb82d0c.png)

ChatGPT 回答 — 作者提供的图像

好的，我的 AI 正在用白棋下棋，我们开始一局经典的 e4 开局吧！
