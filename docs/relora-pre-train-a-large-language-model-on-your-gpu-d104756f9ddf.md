# ReLoRa: 在您的 GPU 上预训练大型语言模型

> 原文：[`towardsdatascience.com/relora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf`](https://towardsdatascience.com/relora-pre-train-a-large-language-model-on-your-gpu-d104756f9ddf)

## LoRa，但多次重置

[](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----d104756f9ddf--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d104756f9ddf--------------------------------) ·8 分钟阅读·2023 年 7 月 20 日

--

![](img/a84220fa206495cacbda94863cf4dfe4.png)

ReLoRa 框架 — 作者提供的图像

在 2021 年，[Hu et al.](https://arxiv.org/abs/2106.09685) 提出了低秩适配器（LoRa）用于 LLMs。此方法通过仅训练几个新增的参数（低秩网络），同时保持 LLM 的原始参数（高秩网络）冻结，从而显著降低了微调大型语言模型（LLMs）的成本。

使用 LoRa，我们仍然需要一个现有的预训练模型来进行微调，即它不能从零开始预训练一个好的 LLM，因为低秩限制。它使得大多数个人和组织无法承担预训练的费用。

为了减少这种成本，[Lialin et al. (2023)](https://arxiv.org/pdf/2307.05695.pdf) 提出了 ReLoRa。这是对 LoRa 的一种修改，允许从零开始预训练 LLMs。

在这篇文章中，我首先解释了 ReLoRa 如何工作。然后，我分析并评论了描述 ReLoRa 的科学论文中呈现的结果。在最后一节中，我展示了如何在您的计算机上设置和运行 ReLoRa。

*关于许可证的说明：* [*在 arXiv 上发表的科学论文*](https://arxiv.org/abs/2307.05695) *描述了 ReLoRa，并根据 CC BY 4.0 许可证进行分发。* [*ReLoRa 的源代码已发布在 GitHub 上*](https://github.com/guitaricet/peft_pretraining) *并根据允许商业使用的 Apache 2.0 许可证进行分发。*

# ReLoRa: 从低秩到高秩网络

要理解 ReLoRa 如何工作，我们必须先更深入地了解 LoRa。

LoRA 通过添加两个不同的新可训练参数集，*A* 和 *B*，在训练后将其合并回原始冻结的高秩网络。

这看起来可能显而易见，但理解 A 和 B 的和的秩高于它们各自秩的和是重要的。这可以形式化如下：

![](img/c1a26cfaf96c26dd1430b0388951656f.png)

方程 (1) 由 [Lialin et al. (2023)](https://arxiv.org/pdf/2307.05695.pdf) 提供

LoRa 仅训练了这两组参数。然而，如果我们能够重置、训练并将它们多次合并回原始高秩网络，我们将能够随着时间的推移增加网络的总体秩。换句话说，我们将获得一个更大的模型。

*为什么 LoRa 不执行这些重置？*

因为有几个重大障碍需要克服，以使这些重置变得有用。标准的 LLM 训练使用 Adam 优化器，Adam 优化器有其自身的状态。在不改变 Adam 状态的情况下重置 LoRa 的可训练参数，会使新的 LoRa 参数与上一个迭代的 LoRa 参数相似。模型将不会学到任何东西。

ReLoRa 提出的一个主要想法是还部分重置 Adam 优化器的状态，并结合“一个不规则的调度器来稳定训练和热启动”。这个不规则调度器将学习率重置为 0，并在几个训练步骤中进行新的热启动。以下是学习率的变化效果：

![](img/3d2bf3b9cf69192706a9e1e60f88e05d.png)

插图由 [Lialin et al. (2023)](https://arxiv.org/pdf/2307.05695.pdf) 提供

更正式地说，运行 ReLoRa 的完整算法可以写成如下形式：

![](img/3d6e9cc39f8d3c95b14ecffaa09c32b9.png)

算法由 [Lialin et al. (2023)](https://arxiv.org/pdf/2307.05695.pdf) 提供

# 结果：ReLoRa 的困惑度低于 LoRa

实际上，ReLoRa 似乎在成本低得多的情况下，得到的结果与标准预训练相似，但仅在参数数量达到最低值以上时有效。

![](img/e7b154048bf49bad3f39d1aada246bbc.png)

表格由 [Lialin et al. (2023)](https://arxiv.org/pdf/2307.05695.pdf) 提供

正如我们在上表中看到的，ReLoRa 在参数数量达到 2.5 亿个或更多时，表现大致与“完整训练”（没有 LoRa 并且没有冻结任何参数）相当。

他们还包括了使用标准 LoRa 进行预训练的结果。它表现不佳，并很好地说明了 ReLoRa 中重置的重要性。

在这些实验中，他们使用了类似于 Meta 的 LLaMa 模型的神经网络架构。这些模型的大部分训练仅用了 1 天时间，使用了 8 个 RTX 4090 GPU（即消费者级 GPU）。

他们没有尝试超过 3.5 亿个参数（大约是 BERT large 的大小），因为计算成本较高。

在一次消融研究中，作者还展示了 LoRa 的重启和调度器新热启动对于实现更低困惑度的重要性。他们还表明，移除不规则调度器可能会导致训练发散。

# ReLoRa 的计算效率

ReLoRa 迭代地训练并向模型中添加新参数，同时保持先前迭代的参数冻结。

因此，ReLoRa 在内存使用方面与 LoRa 一样高效。此外，冻结的参数可以量化到低精度，以进一步减少内存使用。例如，我们可以使用 QLoRa 进行此操作，如我在这里描述的：

## QLoRa: 在你的 GPU 上微调大型语言模型

### 现在可以在消费级硬件上对拥有数十亿参数的模型进行微调。

towardsdatascience.com

# 在你的计算机上运行 ReLoRa

Lialin 等人（2023）在 GitHub 上发布了他们的 [ReLoRa 实现](https://github.com/Guitaricet/peft_pretraining/tree/main)。

由于 ReLoRa 可以与 2.5 亿参数或更多的模型配合使用，我们可以在消费级硬件上运行它，例如，使用拥有超过 6 GB VRAM 的 GPU，或在免费的 Google Colab 实例上运行。

你可以很容易地重现他们的实验，并按以下步骤预训练你自己的 2.5 亿参数的 LLM。

*注意：如果你想直接测试而不编写代码，我在 The Kaitchup（我的 substack 新闻简报）上创建了一个 Google Colab 笔记本。* [*搜索笔记本 #3*](https://kaitchup.substack.com/p/notebooks)*。*

首先，克隆这个仓库：

```py
git clone https://github.com/Guitaricet/peft_pretraining.git
```

然后安装所有需求：

```py
cd peft_pretraining
pip install -r requirements.txt
```

由于训练需要很长时间，我建议首先尝试使用能够提前停止训练并缩短验证的超参数（在消费级硬件上可能需要几个小时）。

该框架没有缩短验证的选项，所以我们需要手动完成。打开文件 `torchrun_main.py`，并替换这一行：

```py
if evaluated_on_tokens > target_eval_tokens:
```

*注意：在撰写本文时，这一行是 129。*

通过：

```py
if evaluated_on_tokens > target_eval_tokens or total_batches > 10:
```

为了更好的性能，框架的作者建议将其分为两步运行。

第一步仅仅是初始化和训练网络几个步骤。

这是我用来测试一切是否正常工作的超参数：

```py
torchrun --nproc-per-node 1 torchrun_main.py \
    --model_config configs/llama_250m.json \
    --batch_size 4 \
    --total_batch_size 8 \
    --lr 5e-4 \
    --max_length 512 \
    --tags warm_start_250M \
    --save_every 10 \
    --num_training_steps 2 \
    --workers 1 \
    --eval_every 1
```

*注意：我将“— nproc-per-node”设置为 1，因为我只有 1 个 GPU。你应该根据你的 GPU 数量进行更改。*

在 `configs` 目录下，你会看到几个 llama 文件。它们包含了不同模型的配置，类似于 LLaMa 模型的架构，但大小不同。在这里，我选择了 250M 的大小。

输出非常详细，因为框架使用 wandb 记录所有内容。这也要求你在这里做出选择：

```py
wandb: (1) Create a W&B account
wandb: (2) Use an existing W&B account
wandb: (3) Don't visualize my results
```

我输入了“3”，因为我没有账户。

然后，在第二步中，我们使用 ReLoRa 的超参数和 PEFT 重新运行框架：

```py
torchrun --nproc-per-node 1 torchrun_main.py \
    --model_config configs/llama_250m.json \
    --batch_size 4 \
    --total_batch_size 8 \
    --lr 1e-3 \
    --max_length 512 \
    --use_peft \
    --relora 5 \
    --cycle_length 5 \
    --restart_warmup_steps 10 \
    --scheduler cosine_restarts \
    --warmup_steps 2 \
    --reset_optimizer_on_relora True \
    --num_training_steps 50 \
    --save_every 10 \
    --eval_every 10 \
    --continue_from checkpoints/llama_250m-2023-07-19-09-39-08/model_3 \
    --tags relora_250M
```

使用“ — continue_from”选项，我们提供了在第一步保存的模型。你的模型将有一个不同的名字，所以你需要更改它。你可以在“checkpoints”目录中找到它。

一旦确认一切正常运行，即没有错误且困惑度逐渐降低，你可以重新启动所有内容，但需要合理设置超参数，以便模型能够得到更好的训练。

*注意：不要忘记移除我们在 torchrun_main.py 中所做的更改，以缩短验证时间。你应该至少将批量总数增加到 100，以获得有意义的验证困惑度。*

```py
torchrun --nproc-per-node 1 torchrun_main.py \
    --model_config configs/llama_250m.json \
    --batch_size 4 \
    --total_batch_size 8\
    --lr 5e-4 \
    --max_length 512 \
    --tags warm_start_250M \
    --save_every 1000 \
    --num_training_steps 10000

torchrun --nproc-per-node 1 torchrun_main.py \
    --model_config configs/llama_250m.json \
    --batch_size 4 \
    --total_batch_size 8 \
    --lr 1e-3 \
    --max_length 512 \
    --use_peft \
    --relora 5000 \
    --cycle_length 5000 \
    --restart_warmup_steps 100 \
    --scheduler cosine_restarts \
    --warmup_steps 500 \
    --reset_optimizer_on_relora True \
    --num_training_steps 10000 \
    --save_every 5000 \
    --eval_every 5000 \
    --continue_from <your checkpoint from step one>\
    --tags relora_250M 
```

*注意：如果你有大量 VRAM 的 GPU，请增加批量大小。*

默认情况下（硬编码），框架使用了 C4 数据集（仅限英语）进行预训练。该数据集还被用于 Google 的 T5 模型预训练。它涵盖了许多任务、领域和体裁。

一旦预训练完成，你仍然需要对结果模型进行微调，以适应下游任务或你选择的领域。你可以通过 QLoRa 高效地完成这项工作。

# 结论

总结来说，ReLoRa 是一种利用低秩网络的新预训练方法。它就像是连续多次执行 LoRa，但具有以下特点：

+   优化器状态的部分重置

+   锯齿状学习率调度

+   每次重置 LoRa 后进行短暂的预热

多亏了 ReLoRa，我们现在可以在消费级硬件上预训练 LLM。

仍需探索这种方法是否对非常大的语言模型（超过 10 亿参数）也具有竞争力。

根据 ReLoRa 的作者，ReLoRa 随着模型的增大效果会更好。然而，这应由未来的工作来实证验证。

*如果你喜欢这篇文章并希望阅读后续文章，支持我工作的最佳方式是通过这个链接成为 Medium 会员：*

[](https://medium.com/@bnjmn_marie/membership?source=post_page-----d104756f9ddf--------------------------------) [## 使用我的推荐链接加入 Medium - Benjamin Marie]

### 作为 Medium 会员，你的会员费的一部分将用于支持你阅读的作者，你将获得所有故事的完全访问权限…

[medium.com](https://medium.com/@bnjmn_marie/membership?source=post_page-----d104756f9ddf--------------------------------)

*如果你已经是会员并希望支持这项工作，* [*只需关注我在 Medium 上的文章*](https://medium.com/@bnjmn_marie)*。*
