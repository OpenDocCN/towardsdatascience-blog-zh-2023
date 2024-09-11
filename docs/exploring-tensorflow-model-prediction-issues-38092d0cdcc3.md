# 探索 TensorFlow 模型预测问题

> 原文：[`towardsdatascience.com/exploring-tensorflow-model-prediction-issues-38092d0cdcc3?source=collection_archive---------15-----------------------#2023-02-02`](https://towardsdatascience.com/exploring-tensorflow-model-prediction-issues-38092d0cdcc3?source=collection_archive---------15-----------------------#2023-02-02)

## 调试 BERT（和其他大语言模型）在个人计算机上慢速预测时间的步骤

[](https://adam1brownell.medium.com/?source=post_page-----38092d0cdcc3--------------------------------)![Adam Brownell](https://adam1brownell.medium.com/?source=post_page-----38092d0cdcc3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38092d0cdcc3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38092d0cdcc3--------------------------------) [Adam Brownell](https://adam1brownell.medium.com/?source=post_page-----38092d0cdcc3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2479b1fc8999&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-tensorflow-model-prediction-issues-38092d0cdcc3&user=Adam+Brownell&userId=2479b1fc8999&source=post_page-2479b1fc8999----38092d0cdcc3---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38092d0cdcc3--------------------------------) ·7 分钟阅读·2023 年 2 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38092d0cdcc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-tensorflow-model-prediction-issues-38092d0cdcc3&user=Adam+Brownell&userId=2479b1fc8999&source=-----38092d0cdcc3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38092d0cdcc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-tensorflow-model-prediction-issues-38092d0cdcc3&source=-----38092d0cdcc3---------------------bookmark_footer-----------)

这一切始于我在玩 BERT 模型时，我收到了所有数据科学家都希望避免的可怕消息：

令人恐惧的“Kernel Died”消息 💀

这发生在我运行 TensorFlow BERT 模型时，在我的 Jupyter Notebook 上。训练大型语言模型（LLMs）通常需要大量数据和计算，因此我的相对弱小的笔记本电脑在这里崩溃也许可以理解……

… 除了这个崩溃发生在*预测*过程中，而不是*训练*过程中，这很奇怪，因为我原以为训练过程比预测过程使用更多内存。

“Kernel Died” 错误提供的信息不够详细，逐行调试 TensorFlow 似乎是一个令人畏惧的任务。

在 Stack Overflow 上做了一些快速搜索，也没有完全解答我的悬而未决的问题。但我仍然需要一个前进的方向。

这是我对 Kernel 死亡问题的探索以及如何找到解决方案的过程。🚀

# 深入探索

鉴于我对问题的了解仅限于内核崩溃，我必须收集更多背景信息。从[一些其他线程](https://stackoverflow.com/questions/39328658/how-to-debug-dying-jupyter-python3-kernel)来看，内核崩溃的原因似乎是我的模型预测需要比我的 CPU 更多的 RAM…
