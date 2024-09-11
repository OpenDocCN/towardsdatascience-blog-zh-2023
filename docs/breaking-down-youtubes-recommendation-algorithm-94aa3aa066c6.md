# Breaking Down YouTube’s Recommendation Algorithm

> 原文：[`towardsdatascience.com/breaking-down-youtubes-recommendation-algorithm-94aa3aa066c6?source=collection_archive---------7-----------------------#2023-04-17`](https://towardsdatascience.com/breaking-down-youtubes-recommendation-algorithm-94aa3aa066c6?source=collection_archive---------7-----------------------#2023-04-17)

## 揭示现代推荐系统运作的“技巧袋”

[](https://medium.com/@samuel.flender?source=post_page-----94aa3aa066c6--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----94aa3aa066c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94aa3aa066c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94aa3aa066c6--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----94aa3aa066c6--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-youtubes-recommendation-algorithm-94aa3aa066c6&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----94aa3aa066c6---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94aa3aa066c6--------------------------------) ·7 min read·Apr 17, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94aa3aa066c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-youtubes-recommendation-algorithm-94aa3aa066c6&user=Samuel+Flender&userId=ce56d9dcd568&source=-----94aa3aa066c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94aa3aa066c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbreaking-down-youtubes-recommendation-algorithm-94aa3aa066c6&source=-----94aa3aa066c6---------------------bookmark_footer-----------)![](img/bc33a53302b6c34ec83db3c79b921ab2.png)

(Logo design [Eyestetix Studio](https://unsplash.com/photos/LskCjwwJBEQ)，background design by [Dan Cristian Pădureț](https://unsplash.com/photos/h3kuhYUCE9A))

推荐系统已成为我们这个时代最普遍的工业机器学习应用之一，但关于它们在实践中的实际运作方式几乎没有公开发表的资料。

一个显著的例外是保罗·科温顿的论文“[YouTube 推荐的深度神经网络](https://research.google/pubs/pub45530/)”，其中充满了关于 YouTube 深度学习推荐算法的众多实际见解和学习经验，提供了一个罕见的视角，不仅展示了现代工业推荐系统的内部运作，还展示了今天的 ML 工程师正尝试解决的问题。

如果你想深入理解现代推荐系统，为 ML 设计面试做准备，或只是对 YouTube 如何吸引用户感到好奇，请继续阅读。在这篇文章中，我们将拆解论文中的 8 个关键见解，以帮助解释 YouTube（以及任何现代推荐系统）的成功。

让我们开始吧。

## 1 — 推荐 = 候选生成 + 排名

YouTube 的推荐系统分为 2 个阶段：候选生成阶段，即过滤数十亿的候选项……
