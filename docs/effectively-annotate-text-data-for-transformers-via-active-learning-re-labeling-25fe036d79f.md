# 通过主动学习 + 重新标注有效标注Transformer的文本数据

> 原文：[https://towardsdatascience.com/effectively-annotate-text-data-for-transformers-via-active-learning-re-labeling-25fe036d79f?source=collection_archive---------2-----------------------#2023-04-27](https://towardsdatascience.com/effectively-annotate-text-data-for-transformers-via-active-learning-re-labeling-25fe036d79f?source=collection_archive---------2-----------------------#2023-04-27)

## 利用主动学习辅助数据标注来提升Transformer模型性能

[](https://medium.com/@chrismauck10?source=post_page-----25fe036d79f--------------------------------)[![Chris Mauck](../Images/d0aeb4d0458544afdfdd59915a962b18.png)](https://medium.com/@chrismauck10?source=post_page-----25fe036d79f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25fe036d79f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25fe036d79f--------------------------------) [Chris Mauck](https://medium.com/@chrismauck10?source=post_page-----25fe036d79f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F96a38f7ac238&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-annotate-text-data-for-transformers-via-active-learning-re-labeling-25fe036d79f&user=Chris+Mauck&userId=96a38f7ac238&source=post_page-96a38f7ac238----25fe036d79f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25fe036d79f--------------------------------) ·9分钟阅读·2023年4月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25fe036d79f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-annotate-text-data-for-transformers-via-active-learning-re-labeling-25fe036d79f&user=Chris+Mauck&userId=96a38f7ac238&source=-----25fe036d79f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25fe036d79f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-annotate-text-data-for-transformers-via-active-learning-re-labeling-25fe036d79f&source=-----25fe036d79f---------------------bookmark_footer-----------)![](../Images/368c78a18b228e319f02837c08250e17.png)

ActiveLab 选择你应该（重新）标注的数据，以训练更有效的模型。给定相同的标注预算，ActiveLab 优于其他选择方法。

在这篇文章中，我重点介绍了使用主动学习来改进一个针对文本分类的微调Transformer模型，同时保持从人工标注者那里收集的标签总数较低。当资源限制使你无法为全部数据获取标签时，主动学习旨在通过选择数据标注者应花费精力标注的示例来节省时间和金钱。

与标准数据标注相比，ActiveLab显著降低了标签成本和实现特定模型性能所需的时间。在本文展示的实验中，ActiveLab在仅使用标准训练的35%标签开支的情况下达到了90%的模型准确率。

# 什么是主动学习？

主动学习有助于优先标注哪些数据，以最大化在标注数据上训练的监督机器学习模型的性能。这个过程通常是迭代进行的——在每一轮，主动学习告诉我们应该收集哪些示例的额外注释，以在**有限的标注预算下**最大限度地提高我们当前模型的表现。

主动学习最适用于在你有大量未标注数据和有限标注预算的情况下高效地标注数据。在这种情况下，你需要决定标注哪些示例以训练一个准确的模型。这里提出的方法（以及大多数机器学习）一个重要假设是，个别示例是独立且同分布的。

# 什么是ActiveLab？

[ActiveLab](https://arxiv.org/abs/2301.11856)是一种主动学习算法，特别适用于标注者有噪声的情况，因为它有助于决定我们应该为一个之前已标注但其标签似乎有问题的示例（标签看起来可疑）再收集一个标注，还是为一个尚未标注的示例收集标注。在收集这些新标注以增加我们的训练数据集后，我们重新训练模型并评估其测试准确率。

[CROWDLAB](https://cleanlab.ai/blog/multiannotator/)是一种估计我们在多标注数据中的共识标签信心的方法，它通过加权集成*任何*训练模型的概率预测*p_M*以及每个标注者分配的单个标签*j*来生成准确的估计。ActiveLab形成了类似的加权集成估计，将每个标注者的选择视为由另一个预测器输出的概率决策*p_j*：

![](../Images/e514002fa30e38d71af8523c27eab17f.png)

当之前标注的数据点的当前共识标签的概率低于我们对未标注数据点预测标签的（纯模型基础上的）信心时，ActiveLab决定重新标注数据。

ActiveLab最适合数据标注应用，其中数据标注者并不完美，并且你能够训练出一个合理的分类器模型（能够产生比随机预测更好的结果）。该方法适用于任何数据模态和分类器模型。

# 动机

我最近加入了 [Cleanlab](https://cleanlab.ai/) 担任数据科学家，并很高兴分享如何使用 ActiveLab（我们 [开源库](https://github.com/cleanlab/cleanlab?utm_medium=tds) 的一部分，AGPL-v3 许可证下免费提供）在各种工作流程中改进数据集。

在这里，我考虑的是一个二分类文本任务：预测特定短语是礼貌还是不礼貌。与随机选择要收集额外标注的示例相比，使用 ActiveLab 的主动学习在每一轮中始终能产生更好的 Transformer 模型（大约 **50%** 的错误率），无论总标注预算是多少！

本文的其余部分将介绍你可以使用的开源代码，以实现这些结果。你可以运行所有代码以重现我的主动学习实验，点击这里查看：[Colab Notebook](https://colab.research.google.com/github/cmauck10/active-learning/blob/master/active-learning.ipynb)

# 文本礼貌性的分类

我考虑的数据集是 [斯坦福礼貌语料库](https://convokit.cornell.edu/documentation/wiki_politeness.html) 的一个变体。它被结构化为一个二分类文本任务，用于分类每个短语是否礼貌。人工注释员会收到选定的文本短语，并提供关于其礼貌性的（不完美的）注释：0 代表不礼貌，1 代表礼貌。

在标注数据上训练一个 Transformer 分类器，我们在一组保留的测试示例上测量模型准确性，我对这些示例的真实标签很有信心，因为它们是基于 5 位注释员对这些示例的一致性标记得出的。

关于训练数据，我们有：

+   `X_labeled_full`：我们初始的训练集，仅有 100 个文本示例，每个示例有 2 个标注。

+   `X_unlabeled`：一个包含 1900 个未标记文本示例的大集合，我们可以考虑让注释员进行标记。

这里是一些来自 `X_labeled_full` 的示例：

1.  **嗨，文章写得很好。“投机性的联排别墅”是什么意思？是为潜在租户而建造的，而不是为已确认的买家而建造的？**

+   注释（由注释员 #61 提供）：礼貌

+   注释（由注释员 #99 提供）：礼貌

2\. **恭喜，还是说好运？**

+   注释（由注释员 #16 提供）：礼貌

+   注释（由注释员 #22 提供）：不礼貌

3\. **谷歌上有 450 万条结果显示非哥伦比亚大学的校园。你在说什么？**

+   注释（由注释员 #22 提供）：不礼貌

+   注释（由注释员 #61 提供）：不礼貌

# 方法论

对于每一轮 **主动学习** 我们：

1.  计算 ActiveLab 共识标签，用于从迄今为止收集到的所有标注中获得每个训练示例的标签。

1.  使用这些共识标签在当前训练集上训练我们的 Transformer 分类模型。

1.  在测试集上评估测试准确性（测试集具有高质量的真实标签）。

1.  运行交叉验证以从我们的模型中获得整个训练集和未标记集的样本外预测类别概率。

1.  获取训练集和未标注集中的每个示例的ActiveLab主动学习得分。这些得分估计了为每个示例收集另一个注释的有用性。

1.  选择具有最低主动学习得分的示例子集(*n = batch_size*)。

1.  为每个* n *个选定示例收集一个额外的注释。

1.  将新的注释（以及如果被选中的新的之前未标注的示例）添加到我们下一个迭代的训练集中。

我随后比较了通过主动学习标注的数据与通过**随机选择**标注的数据训练的模型。对于每轮随机选择，我使用多数投票共识，而不是主动学习共识（在步骤1中），然后随机选择**n**个示例以收集额外的标签，而不是使用ActiveLab得分（在步骤6中）。

# 模型训练与评估

这是我们用于模型训练和评估的代码，使用了提供许多最先进Transformer网络的[Hugging Face库](https://huggingface.co/docs/transformers/model_doc/distilbert)。

```py
# Helper method to get accuracy and pred_probs from Trainer.
def compute_metrics(p):   
    logits, labels = p
    pred = np.argmax(logits, axis=1)
    pred_probs = softmax(logits, axis=1)
    accuracy = accuracy_score(y_true=labels, y_pred=pred)
    return {"logits":logits, "pred_probs":pred_probs, "accuracy": accuracy}

# Helper method to initiate a new Trainer with given train and test sets.
def get_trainer(train_set, test_set):

    # Model params.
    model_name = "distilbert-base-uncased"
    model_folder = "model_training"
    max_training_steps = 300
    num_classes = 2

    # Set training args.
    # We time-seed to ensure randomness between different benchmarking runs.
    training_args = TrainingArguments(
        max_steps=max_training_steps, 
        output_dir=model_folder,
        seed = int(datetime.now().timestamp())
    )

    # Tokenize train/test set.
    dataset_train = tokenize_data(train_set)
    dataset_test = tokenize_data(test_set)

    # Initiate a pre-trained model.
    model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=num_classes)
    trainer = Trainer(
        model=model,
        args=training_args,
        compute_metrics = compute_metrics,
        train_dataset = train_tokenized_dataset,
        eval_dataset = test_tokenized_dataset,
    )
    return trainer
```

我首先对测试集和训练集进行分词，然后初始化一个预训练的DistilBert Transformer模型。对DistilBert进行300次训练步骤的微调在我的数据中取得了准确性和训练时间的良好平衡。该分类器输出预测的类别概率，我将其转换为类别预测，然后评估其准确性。

# 使用主动学习得分来决定下一步标注什么

这是我们用来评分每个示例的代码，基于主动学习对获取该示例一个更多标签的有用性的估计。

```py
from cleanlab.multiannotator import get_active_learning_scores

pred_probs, pred_probs_unlabeled = get_pred_probs(train_set, X_unlabeled)

# Compute active learning scores.
active_learning_scores, active_learning_scores_unlabeled = get_active_learning_scores(
    multiannotator_labels, pred_probs, pred_probs_unlabeled
)
# Get the indices of examples to collect more labels for.
chosen_examples_labeled, chosen_examples_unlabeled = get_idx_to_label(
    X_labeled_full,
    X_unlabeled,
    extra_annotations,
    batch_size_to_label,
    active_learning_scores,
    active_learning_scores_unlabeled,
)
```

在每轮主动学习过程中，我们通过对当前训练集进行3折交叉验证来训练我们的Transformer模型。这使我们能够获得训练集中每个示例的样本外预测类别概率，我们还可以使用训练好的Transformer模型获取未标注池中每个示例的样本外预测类别概率。所有这些操作都在`get_pred_probs`辅助方法中实现。使用样本外预测有助于我们避免由于潜在的过拟合而产生的偏差。

一旦我获得这些概率预测，我将它们传递到开源的[cleanlab](https://github.com/cleanlab/cleanlab)包中的`get_active_learning_scores`方法，该方法实现了[ActiveLab算法](https://arxiv.org/abs/2301.11856)。该方法为我们提供了所有标注和未标注数据的得分。较低的得分表示收集一个额外标签对当前模型的有用性最高的数据点（标注数据和未标注数据之间的得分可以直接比较）。

我将一批得分最低的示例作为收集注释的例子（通过`get_idx_to_label`方法）。在每一轮中，我始终收集相同数量的注释（无论是在主动学习还是随机选择方法下）。在此应用中，我将每个示例的最大注释数量限制为5个（不想反复花费精力对相同示例进行标注）。

# 添加新注释

这是用于将新的注释添加到当前训练数据集的代码。

```py
# Combine ids of labeled and unlabeled chosen examples.
chosen_example_ids = np.concatenate([X_labeled_full.iloc[chosen_examples_labeled].index.values, X_unlabeled.iloc[chosen_examples_unlabeled].index.values])

# Collect annotations for the selected examples.
for example_id in chosen_example_ids:
  # Collect new annotation and who it's coming from.
    new_annotation = get_annotation(example_id, chosen_annotator)

  # New annotator has been selected.
    if chosen_annotator not in X_labeled_full.columns.values:
        empty_col = np.full((len(X_labeled_full),), np.nan)
        X_labeled_full[chosen_annotator] = empty_col

    # Add selected annotation to the training set.
    X_labeled_full.at[example_id, chosen_annotator] = new_annotation
```

`combined_example_ids`是我们希望收集注释的文本示例的ID。对于每个示例，我们使用`get_annotation`辅助方法从标注者那里收集新的注释。在这里，我们优先选择已经标注过其他示例的标注者。如果给定示例的标注者在训练集中不存在，我们将随机选择一个。在这种情况下，我们在训练集中添加一个新列，表示新的标注者。最后，我们将新收集的注释添加到训练集中。如果相应的示例之前未被标注，我们还将其添加到训练集中，并从未标记集合中移除。

我们现在已经完成了一轮新的注释收集，并在更新后的训练集上重新训练了Transformer模型。我们在多个回合中重复这个过程，以不断扩展训练数据集并提升模型性能。

# 结果

我进行了25轮主动学习（标注数据批次并重新训练Transformer模型），每轮收集25个注释。我重复了这一过程，下一次使用随机选择来确定每轮标注的示例——作为基线比较。在额外数据被注释之前，这两种方法都以相同的初始训练集100个示例开始（因此在第一轮中实现了大致相同的Transformer准确率）。由于训练Transformers固有的随机性，我对每种数据标注策略进行了五次完整的过程，并报告了五次重复运行中的测试准确率的标准差（阴影区域）和均值（实线）。

**总结：与标准数据注释相比，ActiveLab大大减少了时间和标注成本以实现相同的模型性能。例如，ActiveLab以仅35%的标注费用达到90%的模型准确率。**

![](../Images/211af98037fbd0c02e3592929148ee88.png)

相较于随机选择，ActiveLab在5次运行中的表现明显优越。标准差以阴影表示，实线为均值。

我们发现，选择接下来标注的数据对模型性能有着深远的影响。使用ActiveLab进行的主动学习在每一轮中都明显优于随机选择。例如，在第4轮中，训练集总共有275个标注，通过主动学习获得**91%的准确率**，而没有聪明选择标注数据策略时，仅获得76%的准确率。总体而言，经过主动学习构建的数据集上训练得到的Transformer模型，无论总标注预算是多少，其错误率大约只有**50%**。

尽管主动学习有其优点，但它可能并不总是最有利的方法。例如，当数据标注过程成本低廉，或在未标记的数据集与模型在部署过程中遇到的数据之间存在选择偏差或分布变化时。此外，主动学习反馈循环依赖于分类器模型生成比随机更具信息性的预测。当情况并非如此时，主动学习可能无法提供关于数据信息量的显著信号。

**在进行文本分类数据标注时，您应考虑使用具有重新标注选项的主动学习，以更好地应对标注者的不完美。**

*除非另有说明，否则所有图片均由作者提供。*
