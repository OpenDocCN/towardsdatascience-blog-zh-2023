# TensorFlow 中的超参数搜索不再出现 OOM 异常

> 原文：[https://towardsdatascience.com/no-more-oom-exceptions-during-hyperparameter-searches-in-tensorflow-26e6e3069bc9?source=collection_archive---------3-----------------------#2023-04-01](https://towardsdatascience.com/no-more-oom-exceptions-during-hyperparameter-searches-in-tensorflow-26e6e3069bc9?source=collection_archive---------3-----------------------#2023-04-01)

## 使用包装函数来避免 OOM 异常

[](https://pascaljanetzky.medium.com/?source=post_page-----26e6e3069bc9--------------------------------)[![Pascal Janetzky](../Images/43d68509b63c5f9b3fc9cef3cbfc1a88.png)](https://pascaljanetzky.medium.com/?source=post_page-----26e6e3069bc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26e6e3069bc9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----26e6e3069bc9--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----26e6e3069bc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F672b95fdf976&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fno-more-oom-exceptions-during-hyperparameter-searches-in-tensorflow-26e6e3069bc9&user=Pascal+Janetzky&userId=672b95fdf976&source=post_page-672b95fdf976----26e6e3069bc9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26e6e3069bc9--------------------------------) ·8分钟阅读·2023年4月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F26e6e3069bc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fno-more-oom-exceptions-during-hyperparameter-searches-in-tensorflow-26e6e3069bc9&user=Pascal+Janetzky&userId=672b95fdf976&source=-----26e6e3069bc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F26e6e3069bc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fno-more-oom-exceptions-during-hyperparameter-searches-in-tensorflow-26e6e3069bc9&source=-----26e6e3069bc9---------------------bookmark_footer-----------)

现在是 2023 年。机器学习不再是炒作，而是日常产品的核心。更快的硬件使得训练更大规模的机器学习模型成为可能——而且时间也更短。每天有约 100 篇关于机器学习或相关领域的论文提交到 [arXiv](https://arxiv.org/list/cs.LG/pastweek?skip=0&show=635)，因此至少三分之一的论文有可能利用硬件的能力来进行超参数搜索，以优化他们使用的模型。这很简单，不是吗？只需选择一个框架——[Optuna](https://optuna.org)、[wandb](https://wandb.ai/site)，随便哪个——将其插入你正常的训练循环中，然后……

OOM 错误。

至少，这就是 TensorFlow 经常发生的情况。

![](../Images/b3e570b3e786fedc8cb8fa13c920d878.png)

照片由 [İsmail Enes Ayhan](https://unsplash.com/@ismailenesayhan?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 当前状态

缺乏适当释放 GPU 内存的功能引发了许多讨论和问题，出现在像 StackOverflow 或 GitHub ([1](https://stackoverflow.com/questions/39758094/clearing-tensorflow-gpu-memory-after-model-execution)、[2](https://github.com/tensorflow/tensorflow/issues/36465)、[3](https://stackoverflow.com/questions/69296496/clear-the-graph-and-free-the-gpu-memory-in-tensorflow-2)、[4](https://discuss.tensorflow.org/t/clear-the-graph-and-free-the-gpu-memory-in-tensorflow-2/4731)、[5](https://github.com/keras-team/keras/issues/12625)、[6](https://stackoverflow.com/questions/75644097/is-there-a-way-to-clear-gpu-memory-after-training-the-tf2-model)）等问答论坛中。每个问题都有一套类似的解决方法：

- 限制 GPU 内存增长

- 使用 numba 库来清除 GPU

- 使用原生 TF 函数，这些函数*应该*可以做到这一点

- 切换到 PyTorch
