# 微调更好的聊天模型，采用蒸馏身份偏好优化（IPO）

> 原文：[`towardsdatascience.com/fine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48`](https://towardsdatascience.com/fine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48)

## Mistral 7B 与 IPO 对齐

[](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)![本杰明·玛丽](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------)![数据科学的方向](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------) [本杰明·玛丽](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------) ·阅读时间 6 分钟·2023 年 12 月 13 日

--

![](img/e762c4828a29a687b5f127b736a29db8.png)

图片由 [Rishabh Dharmani](https://unsplash.com/@rishabhdharmani?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

要成为聊天模型，预训练的大型语言模型（LLMs）会在配对的指令/问题和预期答案的大型数据集上进行微调。虽然这种简单的微调可以产生令人信服的聊天模型，但它们的回答可能仍然不连贯、有偏见、不道德和不安全。这就是为什么我们通常会进行额外的训练步骤，以更好地使 LLM 与人类对齐。

这种对齐可以通过带有人类反馈的强化学习（RLHF）来完成。正如 OpenAI 和 ChatGPT 的成功所示，RLHF 可以产生最先进的聊天模型。然而，RLHF 的运行成本很高。它需要大量由人类标注的数据集和训练多个辅助模型（参考和奖励模型）。

作为 RLHF 的更简单和更便宜的替代方案，[直接偏好优化（DPO）](https://kaitchup.substack.com/p/fine-tune-your-own-instruct-version)最近已成功应用于对齐 LLMs，如 Hugging Face 的[Zephyr](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)和 Intel 的[神经聊天](https://huggingface.co/Intel/neural-chat-7b-v3)。

在本文中，基于 Google DeepMind 的研究，我们将看到尽管 RLHF 和 DPO 在对齐 LLMs 方面表现良好，但由于训练中使用的数据集，它们远未达到最终效果。DeepMind 还展示了为什么 DPO 容易过拟合。我将用通俗易懂的语言解释 DeepMind 提出的替代方案，即身份政策优化（IPO）目标，如何比 RLHF 和 DPO 更简单且更好地从训练数据中学习。

在接下来的部分中，我将展示如何使用 IPO，按照与 Hugging Face 训练 Zephyr 模型类似的训练配方。

我还实现了一个演示 Mistral 7B IPO 训练的笔记本。你可以在这里找到：

[获取笔记本（#31）](https://kaitchup.substack.com/p/notebooks)

描述 IPO 的 DeepMind 论文在 arXiv 上：

[一个理解来自人类偏好的学习的普遍理论范式](https://arxiv.org/abs/2310.12036)

# ΨPO：偏好优化的泛化

RLHF 和 DPO 在类似的数据集上进行训练：这些数据集包括至少两个由人类（或 LLMs）评分的可能答案。答案被配对，以便在一个配对中，一个答案的评分优于另一个。对于其对齐，我们希望 LLM 学习到哪个答案更受人类欢迎。这就是“偏好优化”。RLHF 通过强化学习来学习这一点，而 DPO 通过简单的分类器来学习。

此外，为了避免过拟合，RLHF 和 DPO 训练必须进行正则化。这种正则化是通过 KL 正则化实现的，控制 LLM 在每一步训练中变得更好，同时不偏离原始（也称为参考）未对齐模型太远。

我在这篇文章中更详细地解释了 KL 正则化在 RLHF 中的工作原理：

[](https://kaitchup.substack.com/p/train-instruct-llms-on-your-gpu-with-6a5?source=post_page-----99cddc819a48--------------------------------) [## 在您的 GPU 上使用 DeepSpeed Chat 训练 Instruct LLMs - 第 3 步：与人类的强化学习…

### DeepSpeed Chat 实际应用的效率

kaitchup.substack.com](https://kaitchup.substack.com/p/train-instruct-llms-on-your-gpu-with-6a5?source=post_page-----99cddc819a48--------------------------------)

如果没有这种正则化，LLM 会找到一种方法来生成回答，从而最小化训练损失，但这些回答可能没有意义，即它会过拟合训练数据。

DeepMind 证明了 RLHF 和 DPO 是更一般学习目标的特殊情况：Ψ偏好优化（ΨPO）。*注意：我不知道 DeepMind 如何发音“ΨPO”，但我猜这可能是 psi-P-O。*

DeepMind 指出的另一个弱点，但 LLM 从业者也非常熟悉的是，即使有 KL 正则化，DPO 也容易过拟合。例如，Hugging Face 在[他们关于训练 Zephyr 的技术报告](https://arxiv.org/abs/2310.16944)中发现，DPO 在仅一个训练周期后就能以完美的准确度过拟合训练数据。

DeepMind 论文的大部分内容致力于证明为什么会发生这种过拟合。我认为这是论文中最复杂的部分。让我们试着用简单的英语来总结一下。

RLHF 和 DPO 在带有偏好注释的答案对上进行训练。通常，在为这种训练创建的数据集中，相同的答案或一个接近的答案总是比另一个更好。在这种情况下，我们可以说它是确定性的：相同的输入总会产生相同的输出。这似乎很直观，但实际上，人们的意见不一致：给定一对答案 A 和 B，一个人可能认为 A 比 B 好，而另一个人可能认为 B 比 A 好。

如果我们想比较两个表现相近的 LLM，即生成的答案仅略有不同，这种情况会经常发生。RLHF 和 DPO 设计用来从这种非确定性数据中学习。

RLHF 和 DPO 为评分的答案分配 Elo 分数，换句话说，它们将成对偏好转换为逐点估计。它们不会直接预测哪个答案更好，而是预测每个答案的（复杂的）分数，以[logit](https://en.wikipedia.org/wiki/Logit)偏好的形式。

我们可以说这使得学习目标不必要地复杂化。实际上，当训练数据是确定性的，并且每个提示只使用非常少量的回答时，我们不需要逐点估计。

这就是为什么 DeepMind 提出了另一个更简单的学习目标：身份偏好优化。

# 身份偏好优化（IPO）

我们看到 DPO 容易出现过拟合。然而，DPO 的优势在于它是一个简单的学习目标，因为它不需要奖励模型，也不进行强化学习。

通过身份偏好优化（IPO）学习目标，DeepMind 提出了一种基于 DPO 的替代方案，它不需要奖励模型，同时更好地利用 KL-正则化。

像 DPO 一样，IPO 是ΨPO 目标的一个特例。IPO 和 DPO 的主要区别在于 IPO 直接学习成对的偏好，而不是 logit-偏好，即 IPO 不使用 Elo 分数。

论文中提供了 IPO 的完整数学证明和公式。他们还比较了 DPO 和 IPO 在 KL-正则化下不同系数的表现（见[论文的图 1](https://arxiv.org/pdf/2310.12036.pdf)）。

他们观察到 DPO 对 KL-正则化系数的增加几乎不敏感。另一方面，IPO 受系数增加的影响要大得多。有趣的是，在图 1 的 IPO 曲线中，我们可以看到 IPO 仍能区分输出 y1、y2 和 y3。它将它们排名为 y1 > y2 > y3，而 DPO 认为 y1 是完美的（接近 1.0），而 y2 和 y3 则被认为是同样糟糕的。DPO 出现过拟合，IPO 则没有。

# 为 Mistral 7B 蒸馏 IPO

IPO 已经在 Hugging Face 的 TRL 中提供，用于对齐 LLM。由于 IPO 是基于 DPO 的，他们直接将其添加到了[DPOTrainer](https://huggingface.co/docs/trl/main/en/dpo_trainer)中。

我用（几乎）相同的方法对 Mistral 7B 进行了对齐，这些方法受到 Zephyr 模型的启发，详细内容可以参见这篇文章：

[](https://kaitchup.substack.com/p/a-cheap-zephyr-7b-beta-distilled?source=post_page-----99cddc819a48--------------------------------) [## 便宜的 Zephyr 7B Beta：在消费级硬件上提炼的 DPO

### 在不使用 A100 GPU 的情况下训练类似 Zephyr 的模型的配方

[kaitchup.substack.com](https://kaitchup.substack.com/p/a-cheap-zephyr-7b-beta-distilled?source=post_page-----99cddc819a48--------------------------------)

主要的区别在于使用 IPO 而非 DPO。训练数据由其他 LLM 生成并评估，保持不变。这是一个提炼过的 IPO。

我还对 LoRA 超参数做了小的修改，使用了[Hugging Face 对齐手册](https://github.com/huggingface/alignment-handbook/blob/main/recipes/zephyr-7b-beta/sft/config_lora.yaml)中提出的参数：

```py
peft_config = LoraConfig(
        lora_alpha=64,
        lora_dropout=0.1,
        r=16,
        bias="none",
        task_type="CAUSAL_LM",
        target_modules= ['k_proj', 'q_proj', 'v_proj', 'o_proj']
)
```

然后，DPOTrainer 可以简单地配置为使用 IPO，如下所示：

```py
trainer = DPOTrainer(
    model,
    model_ref,
    args=training_arguments,
    beta=0.5,
    peft_config=peft_config,
    train_dataset=dataset_train_dpo,
    eval_dataset=dataset_test_dpo,
    tokenizer=tokenizer,
    loss_type='ipo'
)
```

“loss_type”被简单地设置为‘ipo’。 “beta”超参数是正则化的系数（tau）。

还值得注意的是，使用 DPO 时，Hugging Face 固定了一个非常低的学习率 5e-7。DPO 很容易过拟合或者在较高学习率下无法收敛。另一方面，使用 IPO 时，我观察到较高的学习率是可行的，但它应该与 beta（正则化的系数）一起调整，以防止过拟合。

# 结论

正如 DeepMind 所展示的那样，IPO 在理论上比 RLHF 和 DPO 更好，特别是对于用于对齐 LLM 的训练数据集。IPO 直接学习成对的偏好，并更好地利用 KL 正则化以避免过拟合。

由于 IPO 保留了 DPO 的主要优点，同时修正了其缺陷，它应该成为 LLM 对齐的新标准学习目标。
