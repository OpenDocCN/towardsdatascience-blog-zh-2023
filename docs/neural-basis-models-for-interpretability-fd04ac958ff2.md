# 神经基础模型的可解释性

> 原文：[https://towardsdatascience.com/neural-basis-models-for-interpretability-fd04ac958ff2?source=collection_archive---------8-----------------------#2023-10-11](https://towardsdatascience.com/neural-basis-models-for-interpretability-fd04ac958ff2?source=collection_archive---------8-----------------------#2023-10-11)

## 解读Meta AI提出的新型可解释模型

[](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)[![Nakul Upadhya](../Images/336cb21272e9b1f098177adbde50e92e.png)](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-basis-models-for-interpretability-fd04ac958ff2&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----fd04ac958ff2---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------) · 6 min read · 2023年10月11日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd04ac958ff2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-basis-models-for-interpretability-fd04ac958ff2&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----fd04ac958ff2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd04ac958ff2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-basis-models-for-interpretability-fd04ac958ff2&source=-----fd04ac958ff2---------------------bookmark_footer-----------)

机器学习和人工智能在各个领域的广泛应用带来了更高的风险和伦理评估挑战。如在[ProPublica报道的犯罪再犯模型](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing)案例中所见，机器学习算法可能存在极大的偏见，因此需要强有力的解释机制，以确保在这些模型被应用于高风险领域时的信任和安全。

那么，我们如何在可解释性与准确性和模型表达能力之间取得平衡呢？好吧，Meta AI 研究人员提出了一种新方法，他们称之为[神经基础模型 (NBMs)[1]](https://proceedings.neurips.cc/paper_files/paper/2022/hash/37da88965c016dca016514df0e420c72-Abstract-Conference.html)，这是广义加性模型的一个子系列，在基准数据集上实现了最先进的 (SOTA) 性能，同时保留了玻璃箱的可解释性。

在这篇文章中，我旨在解释 NBM 及其成为有益模型的原因。像往常一样，我鼓励大家阅读原始论文。

如果你对可解释的机器学习和其他伦理 AI 方面感兴趣，可以查看我的其他文章并关注我！

![纳库尔·乌帕德亚](../Images/e62aa67aa11cd0f9bcd1132257fc3773.png)

[纳库尔·乌帕德亚](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)

## 可解释和伦理 AI

[查看列表](https://medium.com/@upadhyan/list/interpretable-and-ethical-ai-f6ee1f0b476d?source=post_page-----fd04ac958ff2--------------------------------)5个故事![](../Images/3718151c0f72303f3d1c71f54229bc98.png)![](../Images/eddb4279ebae7fc1ba79cf6dcc6ebd5a.png)![](../Images/7def8e23dad656929857f2a488b1f547.png)

# 背景：GAMs
