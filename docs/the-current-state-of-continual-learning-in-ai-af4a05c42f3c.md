# 《人工智能中的持续学习现状》

> 原文：[https://towardsdatascience.com/the-current-state-of-continual-learning-in-ai-af4a05c42f3c?source=collection_archive---------1-----------------------#2023-10-18](https://towardsdatascience.com/the-current-state-of-continual-learning-in-ai-af4a05c42f3c?source=collection_archive---------1-----------------------#2023-10-18)

## 为什么 ChatGPT 只训练到 2021 年？

[](https://medium.com/@jon.flynn2?source=post_page-----af4a05c42f3c--------------------------------)[![Jon Flynn](../Images/492cef280f4ea0b002e5d00ad2e083a5.png)](https://medium.com/@jon.flynn2?source=post_page-----af4a05c42f3c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af4a05c42f3c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----af4a05c42f3c--------------------------------) [Jon Flynn](https://medium.com/@jon.flynn2?source=post_page-----af4a05c42f3c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa3ee742fae3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-current-state-of-continual-learning-in-ai-af4a05c42f3c&user=Jon+Flynn&userId=a3ee742fae3&source=post_page-a3ee742fae3----af4a05c42f3c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af4a05c42f3c--------------------------------) ·23分钟阅读·2023年10月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf4a05c42f3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-current-state-of-continual-learning-in-ai-af4a05c42f3c&user=Jon+Flynn&userId=a3ee742fae3&source=-----af4a05c42f3c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf4a05c42f3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-current-state-of-continual-learning-in-ai-af4a05c42f3c&source=-----af4a05c42f3c---------------------bookmark_footer-----------)![](../Images/295e18f233f38577ab964460240114a5.png)

图片由作者使用 DALL-E 3 生成

## **知识前提：**

几年前，我通过StatQuest视频、Lena Voita的NLP博客以及《深度学习实践者》和《谈谈网络》等书籍学习了深度学习的基础。我现在希望了解深度学习中的持续学习的现状。我发现很少有信息能以简单的术语总结这个主题，并且需要筛选专家研究论文。因此，这篇文章是为那些对该主题有基本了解但发现研究难以阅读且可能不是专家的读者准备的。它集中在聊天机器人上，因此了解chatGPT的训练阶段也很有帮助。

# **简介**

![](../Images/c1b8d3a88f40aa06733f43b29cfe4f48.png)

ChatGPT告诉用户它的训练只到2021年9月（作者截图）

如果像ChatGPT这样的语言模型能够不断用新数据更新，它们将加速从软件开发到法律程序到学习的一系列任务。这也会使像这样的一些文章变得过时。

持续学习是暂停模型训练过程，保存模型当前状态，然后稍后在新数据上重新开始训练的能力。模型应该能够很好地泛化到新数据，同时仍保持对旧数据的泛化能力。有关更正式的定义，请参阅[这篇论文](https://arxiv.org/pdf/2302.00487.pdf)。

目前，行业内增强聊天机器人的数据趋势是使用RAG，将查询的向量与提示工程相结合来回答问题，而不是继续用新数据训练LLM。ChatGPT的零-shot学习能力，使其能够回答关于新、未见数据的问题，这种方法非常有吸引力。例如，你可以教它一种新的编程语言，然后用少量提示询问有关该语言的问题，尽管性能会随着输入的tokens数量增加而略微下降。基于新主题不断训练模型回答问题需要大量计算资源，更重要的是，需要广泛的相关主题数据。此外，如果一个主题在训练集中出现频率很低，它的泛化能力会很差。例如：拿一个不受欢迎的公共仓库，尽管它在训练过程中曾见过，但可能对它了解甚少，还可能出现幻觉。上下文窗口（模型能接收的tokens数量）正在迅速变大，使RAG更加吸引人。然而，理想情况下，我们是否不希望有一个智能的全知模型，而不需要任何外部数据库？

持续学习是通向AGI的重要一步，有人怀疑我们是否能够在没有深度学习网络架构重大变革的情况下实现这一目标。杰夫·霍金斯在他的书《[千脑](https://www.numenta.com/resources/books/a-thousand-brains-by-jeff-hawkins/)》中提到，他认为当前的ANN不具备有效的持续学习能力，并且认为未来的模型可能需要更类似于人脑的架构，利用他关于新皮质皮层柱中的参考框架的理论。

# 语言模型的预训练阶段与微调阶段的持续学习

今年早些时候，发布了一篇名为《[LIMA：少即是多的对齐](https://arxiv.org/abs/2305.11206)》的研究论文。该论文介绍了一个没有使用来自人类反馈的强化学习（RLHF）进行训练的聊天机器人，而是仅在1,000个精心标注的问答样本上进行了微调。令人惊讶的是，研究人员表示，在43%的情况下，“该聊天机器人的回应与GPT-4相当”。我没有深入了解这些回应是如何评估的，但尽管如此，普遍认为模型的大部分知识和能力是在预训练阶段获得的，这项研究进一步证明了这一点。

像ChatGPT和Llama-chat这样的模型已经经历了广泛的微调，以生成更对齐和有效的响应。OpenAI目前提供一个API来进一步[微调模型](https://platform.openai.com/docs/guides/fine-tuning)，该API接受问答数据作为输入用于进一步训练。然而，这不应被用来教模型新数据，而是用来[定制语气和可调性](https://openai.com/blog/gpt-3-5-turbo-fine-tuning-and-api-updates)。尝试通过微调模型来教它新数据可能会导致*灾难性遗忘*，即模型会忘记已学到的内容。本文将介绍一些旨在减轻这一问题的技术。

这也引出了关于持续学习的可行性和策略的一些关键问题：

+   在哪个阶段引入持续学习最为有利且最为容易？

+   鉴于微调和RLHF都会改变整个模型的参数，是否还有可能回到预训练阶段进行进一步修改？

*注：我为以下讨论的部分论文提供了一些类似PyTorch的伪代码。这些伪代码未经过测试，可能无法正常工作，用于逐步解析技术并翻译任何混淆的数学符号，以帮助读者理解。*

# 持续学习技术的5个子类别

[持续学习的综合概述论文](https://arxiv.org/pdf/2302.00487.pdf)指出，持续学习的训练策略可以分为5个子类别：

1.  基于正则化的方法：这种方法在训练过程中添加约束或惩罚。

1.  基于优化的方法：这种技术侧重于修改优化算法。

1.  基于表示的方法：这旨在学习跨不同任务的共享特征表示，帮助模型更好地推广到新的但相关的任务。

1.  基于回放的方法：这涉及存储先前任务的一些数据或学习特征，并在训练新任务时重播它们，以维持对早期学习任务的性能。换句话说，在训练新任务时混合旧数据集和新数据集。

1.  基于架构的方法：在这种方法中，网络架构是动态调整的，通常通过增长或分区来委派网络的不同部分给不同的任务。

# 1\. 基于正则化的方法

## 参数的软掩蔽

下面的软掩蔽技术在训练过程中对每个参数的梯度进行掩蔽和调整。接下来的*基于优化的方法*也会操纵梯度以实现持续学习。记住，梯度不只是在训练期间出现和消失的临时数字，它们是指导权重演变的信号。

## SPG

本[论文](https://arxiv.org/pdf/2306.14775.pdf) 提出了一种名为 SPG（Soft-masking of Parameter-level Gradient flow）的技术，旨在：

1.  在每个任务上训练模型直至收敛。

1.  训练后，计算每个任务的每个参数的“重要性”。

1.  根据其累积重要性软掩蔽参数，使重要参数在学习新任务时更不可能改变。

让我们逐步分解这个方法：

## 1\. 训练第一个任务

如常在第一个任务的数据集上训练模型。

## 2\. 计算第一个任务的参数重要性

在第一个任务的训练完成后，我们计算每个模型参数的重要性。这里的直觉很简单，我们使用每个参数的梯度来计算其重要性。较大的梯度意味着该参数的微小变化将导致损失更大的变化，这意味着模型的性能可能会更显著地变化，因此该参数是重要的。

梯度也被归一化，因为第一层的梯度可能很小，而最后一层的梯度可能很大。如果你基于这些原始梯度值计算重要性，最后一层的参数会显得更重要，因为其梯度的规模，并非因为它们在任务中真正更关键。

![图片](../Images/315a4134afb5961eac6c44a9e83f2760.png)

计算 SPG（详见[论文](https://arxiv.org/pdf/2306.14775.pdf) 第3.1节）中模型参数重要性的方程式

让我们将这个计算翻译成类似于 PyTorch 的伪代码：

```py
import torch

def compute_final_importance(model, loss_function, data_loader):
    # Get a single batch from the data loader
    inputs, labels = next(iter(data_loader)) 

    # Forward and backward pass to calculate the gradients for all parameters
    outputs = model(inputs)
    loss = loss_function(outputs, labels)
    loss.backward()

    importances = []

    # Calculate importance based on the gradients
    for param in model.parameters():
        if param.grad is not None:  # Gradients may be None for some unused parameters
            normalized_grad = (param.grad - torch.mean(param.grad)) / torch.std(param.grad)
            importance = torch.tanh(normalized_grad)
            importances.append(importance)

    return torch.stack(importances).mean(dim=0)
```

## 3\. 跨任务积累重要性

每个参数在任务之间的累计重要性是通过在任何阶段取最大值来简单计算的。

## 4\. 训练后续任务、组合损失和软掩码机制：

在新任务上进行训练时，研究人员使用一个由两个部分组成的组合损失函数。其中一个是标准损失函数，用于新任务和数据，另一个是额外的损失函数，它涉及将*新*数据通过*旧*模型（即上一个任务之后的收敛模型检查点），并汇总生成的logits。在分类网络中，logits通常是模型在通过类似softmax函数之前生成的原始非标准化预测。这些logits的总和作为一种损失形式。其理由是，如果在模型参数发生变化时，总和logits受到显著影响，那么这些参数对之前学习任务的性能至关重要。

这些额外损失生成的梯度在反向传播过程中作为指导，推动共享参数朝着一个不容易损害第一个任务性能的方向改变。因此，它充当了一种惩罚项，以强制确保对模型所做的任何更新不会导致与先前任务相关的重要信息的显著丧失。

在下一个任务上训练模型。使用标准训练循环，但在反向传播过程中，根据梯度的累计重要性来修改梯度。这就是软掩码机制：

```py
import torch

accumulated_importance = # calculated at the end of each task

for epoch in range(num_epochs):
  for x, y in train_loader:

    # Forward Pass: Calculate the loss for the current task using the proper loss function
    logits = new_model(x)
    loss_current_task = nn.CrossEntropyLoss()(logits, y)

    # Forward Pass: Calculate the additional losses for previous tasks (CHI mechanism)
    loss_previous_tasks = 0
    for prev_task_id in range(task_id):
        logits_prev = old_model(x, prev_task_id)
        loss_previous_tasks += logits_prev.sum()

    # Combine the losses
    combined_loss = loss_current_task + loss_previous_tasks

    # Backward Pass
    optimizer.zero_grad()
    combined_loss.backward()

    # Update the accumulated importance
    for param, acc_imp in zip(model.parameters(), accumulated_importance):
        grad = param.grad
        acc_imp = torch.max(acc_imp, torch.abs(grad)) 

    # Soft-masking the gradients before taking an optimization step
    for param, imp in zip(model.parameters(), accumulated_importance):
        param.grad *= (1 - importance)

    optimizer.step()
```

## 5\. 软掩码特殊情况

+   特征提取器：共享特征提取器中参数的梯度根据其特定的累计重要性进行修改。

+   分类头：对于分类头，梯度根据特征提取器的平均重要性进行修改。

## 将其应用于大型语言模型（LLMs）

请注意，这篇论文没有用语言模型进行实验，但我假设在语言模型中，你可以将变换器层视为类似于“特征提取器”，将最终分类层（预测序列中的下一个词或标记）视为“分类头”。

# 应用于语言模型持续预训练的软掩码

接下来我们将讨论一篇将类似软掩码应用于语言建模中的预训练阶段的论文。

[这篇论文](https://arxiv.org/pdf/2302.03241.pdf)介绍了一种称为DAS（带软掩码的持续DA预训练）的方法，用于大型语言模型预训练阶段的持续学习。它应用了一种类似于刚才讨论的软掩码技术，并结合了其他几种技术，试图在不遇到灾难性遗忘的情况下继续对LLM进行预训练。

让我们一步步分解：

# 初始预训练阶段

像正常一样预训练LLM。

# 在新领域的进一步预训练

## 准备新领域数据：

准备一个来自不同领域的新数据集。

## 计算每个神经元的重要性

SPG 使用梯度来确定每个参数的重要性，然后将计算得到的重要性值应用于训练过程中参数梯度调整的掩码。本文试图确定每个单元/神经元的重要性，而不是参数，然后在训练过程中以相同的方式使用这种重要性，通过掩盖梯度来进行训练。

本文使用两种不同的方法来计算神经元的重要性，具体取决于任务。其一是基于梯度的重要性检测方法（最初在[这篇论文](https://arxiv.org/pdf/1905.10650.pdf)中概述），其二是定制的“代理损失函数”。

首先介绍的是*不是*用于*第一个*新领域的持续学习。为什么？它需要来自训练数据集的数据才能工作，而作者表示用户“无法访问庞大的原始预训练数据集”，这是一个合理的假设。因此，在持续学习的第一个阶段使用的是代理损失函数，然后在随后的每个阶段使用其他方法。

**代理损失函数（“代理 KL 散度损失”）：**

我最初觉得这个术语令人困惑，但它之所以这样称呼，是因为原始的基于梯度的重要性检测方法本身被定义为一种损失函数，你可以用它来运行网络的输出以获得每个神经元的梯度，然后可以用这些梯度来推导重要性，就像 SPG 技术一样。计算方法如下：

+   取出我们希望训练的新领域的一个子集，并将其通过模型输入两次，以获得两个不同的表示。这些表示会有所不同，因为 Transformer 架构中存在的 dropout 掩码。

+   计算这两个表示之间的 KL 散度。

## 使用代理和综合损失的修改版反向传播流程

1.  **前向传播：** 数据通过神经网络进行前向传播。

1.  **反向传播：**

**应用代理损失以调整梯度：** 代理损失函数的单元级重要性用于软掩盖原始梯度。这可以表示为：

```py
adjusted_grad *= (1 − unit_level_importance)
```

**计算综合损失（MLM + 对比损失）：** 使用 MLM 和对比损失来计算综合损失。

# 在更多领域进行进一步预训练

1.  **直接重要性计算：** 对于每个新领域，现在可以直接使用来自新领域的数据通过方程 3 中概述的基于梯度的方法来计算每个单元的重要性，从而消除了在初始预训练后仅使用一次的代理损失函数的需要。

1.  **神经元的重要性在每个新任务学习时逐步更新。** 这种更新使用逐元素最大值。“逐元素最大（EMax）操作”指的是逐个元素比较两个向量，并取每个对应元素的最大值来创建一个新向量。例如：如果你有两个相同长度的向量A和B，逐元素最大值将产生一个新向量C，其中每个元素 *C*[*i*] 是 *A*[*i*] 和 *B*[*i*] 之间的最大值。

# 2\. 基于优化的方法

我们将参考[综合调查论文](https://arxiv.org/pdf/2302.00487.pdf)第3.1节中概述的两种技术。

## 梯度方向保持

论文讨论了操控基于梯度的优化过程，以使新训练样本的梯度*方向*接近旧训练样本的梯度。公式

> ⟨ ∇θ Lₖ(θ; Dₖ), ∇θ Lₖ(θ; Mₜ) ⟩ ≥ 0

强制要求学习新任务时不会增加旧任务的损失。本质上，鼓励新任务和旧任务的梯度对齐。

分解公式时，我们取新任务的损失梯度（∇θ Lₖ(θ; Dₖ)）与旧任务的损失梯度（∇θ Lₖ(θ; Mₜ)）的点积，应该是非负的。在这种情况下，正的点积意味着旧任务和新任务的梯度通常指向相同的方向，这两个向量之间的角度小于或等于90度。

## 前向/反向传播：

## 前向传播：

你需要将新任务的输入数据 *Dₖ* 和旧任务的 *Mₜ*​ 通过同一个模型来计算每个任务的损失。

## 反向传播：

1.  计算旧任务和新任务的网络参数的损失梯度。

1.  对齐检查：计算两个梯度的点积。然后使用这些信息以使新任务的梯度的点积为非负。

1.  更新权重：使用这些“对齐”的梯度更新模型参数。

```py
 import torch

# Forward pass for the new task
output_k = model(D_k)
loss_k = criterion(output_k, y_k)

# Forward pass for the old task
output_t = model(M_t)
loss_t = criterion(output_t, y_t)

# Compute gradients for both tasks
loss_k.backward(retain_graph=True)  # Compute gradients for new task but keep computation graph
grad_k = torch.cat([p.grad.view(-1) for p in model.parameters()])  

optimizer.zero_grad() 

loss_t.backward()  # Compute gradients for old task
grad_t = torch.cat([p.grad.view(-1) for p in model.parameters()]) 

# Compute dot product and modify gradients if they don't align
dot_product = torch.dot(grad_k, grad_t)
if dot_product < 0:
    # I'm not sure how you modify the gradients here if they don't align, I'm not sure the paper specifies it

# Use the modified gradient to update model parameters
index = 0
for p in model.parameters():
    num_params = p.numel()
    # Update using modified gradients
    p.grad = grad_k[index: index + num_params].view(p.shape)
    index += num_params

optimizer.step()
```

## 无需旧训练样本的梯度方向保持

文本还强调，即使不存储旧样本，也可以进行梯度投影。NCL（自然连续学习，[论文链接](https://proceedings.neurips.cc/paper/2021/hash/ec5aa0b7846082a2415f0902f0da88f2-Abstract.html)）是这里总结的技术。请注意，这可以归类为正则化和优化两种方法。

## 训练过程逐步：

## **前向传播：**

你需要将新数据通过网络并计算损失。

## **反向传播：**

**目标：** 目的是在遵守距离约束 *d*(*θ*,*θ*+*δ*)≤*r.* 的同时，最小化任务特定的损失 *ℓk(θ)*。

**算法逐步**：

1.  如常，计算模型参数∇*θ*​ℓ*k*​(*θ*)的损失梯度。

1.  *δ* 是使用更新规则计算的。这为你提供了基于新任务要求的模型参数 *θ* 的“建议”更改。

1.  然后，将这个 *δ* 插入到距离约束公式中：*d(θ,θ+δ)=squareroot(δ⊤Λ_k-1​δ)*​。约束在当前参数 *θ* 周围起到边界作用，由距离度量 *d*(*θ*,*θ*+*δ*) 和半径 *r* 定义。我很难理解为什么叫它“半径”，而不是“约束值”或其他什么。我认为这是因为研究人员在高维空间中可视化梯度和训练过程。当你基于距离度量应用约束时，本质上是在高维空间中定义了一个围绕当前参数值的“球体”。这个球体的“半径” *r* 设置了在学习新任务时参数可以移动的限制。

1.  如果提议的 *δ* 根据这个距离度量将 *θ* 移动得过远，即超出了这个边界，你就将其缩小，以使其保持在由半径 *r* 定义的允许区域内。

让我们更深入地查看每一部分：

**更新规则：** 更新规则提供了 *θ* 应该移动的方向。

![](../Images/54949a33247d881c077379249243497d.png)

来自 [持续学习论文的综合概述](https://arxiv.org/pdf/2302.00487.pdf) 第3.1节的 NCL 更新规则

分解如下：

+   *∇θ ℓk(θ)* 表示由损失函数计算的所有参数 (*θ*) 的梯度。

+   参数重要性计算 (*Λ^(k-1)_(-1)*): 这个术语表示一个 *精度矩阵*，它是计算网络中参数重要性的另一种方式。*更多细节见下*

+   正则化项 (*θ — μ_(k-1)*): 这个项将更新后的参数拉近到来自先前任务的最佳参数 *μ_(k-1)*。像之前的方法一样，它作为正则化器以避免偏离已经学到的内容。

+   学习率 (*λ*)

**距离约束：** 在应用这个更新之前，你通常会检查这个变化 *δ* 是否会违反距离约束 *d*(*θ*,*θ*+*δ*)≤*r*。如果违反了，你通常会缩小 *δ*，以使其满足约束。

**精度矩阵解释：** 在之前的软掩蔽方法中，我们通过所有神经元的输出或它们的梯度来计算重要性。在这个方法中使用了一个精度矩阵。这有点复杂，所以我会尽力解释：

我们首先计算网络参数的 *协方差矩阵*。在神经网络的上下文中，梯度矩阵 *G* 的列对应于模型的参数（权重和偏差）。*G* 的每一行代表一个训练样本的梯度向量，相对于所有这些参数。

如果你有一个具有 *P* 个参数的神经网络（这包括所有层的所有权重和偏置），那么每个梯度向量将有 *P* 个元素，每个元素对应一个参数。因此，*G* 将是一个形状为 *N* × *P* 的矩阵，其中 *N* 代表每个批次，因此每行代表在给定批次中的所有训练样本的平均梯度向量。

当你从 *G* 计算协方差矩阵 Σ 时，得到的矩阵将具有 *P* × *P* 的维度。对角线条目 Σ*ii*​ 将指示与 *ith* 参数相关的梯度的方差，而非对角线条目 Σ*ij*​ 将指示与 *ith* 和 *jth* 参数相关的梯度之间的协方差。这让你了解这些参数在训练过程中如何相互作用或协变。该矩阵的逆矩阵是 *精度矩阵*，我们用它来确定重要性。

为什么选择 *精度矩阵* 而不是 *协方差矩阵*？虽然协方差矩阵 Σ 确实捕捉了参数在训练过程中如何相互作用，但它没有特别指示每个参数在考虑所有其他参数时对当前任务的关键程度。相比之下，精度矩阵允许我们评估 *条件独立性*（这是概率论中的一个概念，查阅一下）。精度矩阵中的大值表示，在给定所有其他参数的情况下，知道一个参数对另一个参数的了解具有很高的信息量。我不会进入如何运作的示例，所以请让ChatGPT生成一些示例，使用一个非常小的神经网络来看看这些值如何被解释。

我们之前看到的计算重要性的方法关注于单个神经元或参数，忽略了它们之间的关系。另一方面，精度矩阵可以捕捉这些关系。像深度学习中的一切一样，这是否是一种更好的计算网络重要性的方法，还需要通过经验来验证，并且可能会根据任务和网络规模的不同而有所不同。

**PyTorch中的算法步骤：**

```py
import torch

# Constraint radius
radius = 0.1

for epoch in range(num_epochs):  
    for batch_idx, (data, target) in enumerate(data_loader):
        optimizer.zero_grad()

        # Forward pass
        output = model(data)
        loss = loss_function(output, target)

        # Backward pass to get gradients for params
        loss.backward()
        model_grad = torch.cat([p.grad.data.view(-1) for p in model.parameters()])

        # Compute δ using the NCL method
        # δ = Λ^(-1) * grad - (θ - µ)
        delta = torch.matmul(torch.inverse(covarianceMatrix), model_grad) - (torch.cat([p.data.view(-1) for p in model.parameters()]) - parametersForPrevTask)

        # Check constraint
        if torch.norm(delta) > radius:
            delta = radius * delta / torch.norm(delta)

        # Update model parameters (θ) using δ
        idx = 0
        for p in model.parameters():
            length = p.data.numel()
            p.data += delta[idx: idx + length].view(p.data.shape)
            idx += length

        # Update Λ and µ for the next task, probably going to be task-specific and non-trivial
```

# 3\. 基于表示的方法

首先，需要注意的是，对LLM进行预训练以便进一步在下游任务中进行微调，是这个子类别中的持续学习的一个例子。我认为ChatGPT在处理从未见过的数据时的推理能力也是这种方法的一个例子。虽然我们从技术上称之为零样本学习，而“持续学习”这个术语要求更新模型参数，但它超越了我们以前见过的任何东西。正如介绍中所讨论的，提示工程可能是持续学习的未来，而不是不断更新参数。

接下来我们将探讨使用知识蒸馏进行持续学习。我不太确定这属于哪个子类别，但我猜这可能是表示、架构和重放方法的混合。尽管我们正在审查的一些技术可能看起来随意且在大规模上未经验证，但这一领域的突破往往是不可预测的。因此，保持广阔的视角非常重要。

## 知识蒸馏用于持续学习

你可以将一个网络的知识（或“蒸馏”）转移到另一个网络中，第二个网络能够合理地近似原始网络所学习的功能。

蒸馏模型（*学生*）被训练来模仿较大网络（*教师*）的输出，而不是直接用原始数据进行训练。例如，假设你想训练一个较小的学生模型来模仿一个大型预训练语言模型（教师）。将原始预训练数据集通过教师模型生成“软目标”。这些是潜在输出的概率分布，例如：下一词预测任务中，教师可能会提供像 90% 为“cat”、5% 为“kitten”、3% 为“feline”等概率。

这通常是为了将知识转移到更小的模型上，并且尽管模型较小，但效果非常好。

让我们看看一些研究人员是如何[成功应用](https://ojs.aaai.org/index.php/AAAI/article/view/17600)到一个NER（命名实体识别）模型上的。训练过程非常简单：

# 训练过程一步步进行

论文中概述了两种主要方法：AddNER 和 ExtendNER。

## AddNER 模型

请注意，NER 模型通过接收一系列标记（通常是一个句子）作为输入，然后为每个标记输出一个概率分布（针对不同类型的实体）。IOB 标记法通常用于 NER 模型，每个标记可以被标记为‘O’，或作为实体类型 *X* 的开始（‘B-’）或内部（‘I-’）。‘O’表示‘Outside’，这意味着当前标记不属于任何实体。因此，对于 *n* 个实体类型，你将在分类层中得到 2*n* 个输出神经元：*n* 个用于‘B-’ 标签（每种实体类型一个），*n* 个用于‘I-’ 标签（同样，每种实体类型一个）。加上‘O’ 标签，表示一个标记不属于任何实体，你将得到 2*n* + 1 个可能的标签。最终的维度可以写成 *h* × (2*n* + 1)，其中 *h* 是隐藏层输出的大小。请注意，这仅适用于标记只能属于一个实体的模型。例如：“Apple” 可以被标记为“FOOD”和“COMPANY”。

## 架构和师生设置

在这种情况下，学生模型是教师模型的一个副本，额外增加了一个输出分类层，以学习模型需要识别的每种新实体类型。在训练过程中，新的输出层从新的标注数据中学习，而旧的层则通过教师模型的输出进行指导，以减少遗忘。

训练后，旧的输出层不会被丢弃。它接着使用冲突解决器部分*(第3.3节末尾)*中描述的算法和启发式方法，将这些输出组合成每个序列中的单一最终预测。

![](../Images/31710e332c9941fc063c2fed5783abe9.png)

[AddNER模型的示意图](https://ojs.aaai.org/index.php/AAAI/article/view/17600)来自第3.2节

## 前向传播

1.  旧实体类型：将输入句子通过教师模型，以获得旧实体类型的概率分布（在此背景下为“软目标”）。

1.  新实体类型：相同的句子也会通过新学生模型，该模型具有专门针对新实体类型的额外输出层。

## 后向传播

**组合损失函数：**

1.  KD损失：通过比较新模型（学生）对旧实体类型的输出概率与旧模型（教师）的匹配程度来计算。它使用KL散度来计算。它可能是逐个标记计算的，然后在句子或批次中的所有标记上求和或取平均，但我认为论文没有详细说明这一点。

1.  交叉熵损失：这是常用的损失函数，用于比较模型对新实体类型的预测与来自新数据集的实际标签。

1.  两者结合：这两种损失通过加权求和的方式结合成一个组合损失。结合这些损失的权重由超参数alpha和beta设定，像调整其他超参数一样，根据实验结果优化性能。

```py
# Hyperparameters alpha and beta for weighting the two loss functions
alpha = 0.5
beta = 0.5

for epoch in range(num_epochs):
    for sentence, labels in D_new:
        # Forward pass in teacher model for old entity types
        teacher_probs_Ei = teacher_model(sentence)

        # Forward pass in student model for old and new entity types
        # Note: the new entity types must go through the new output layer (not shown in this pseudocode)
        student_probs_Ei, student_probs_Enew = student_model(sentence)

        # Compute KD loss
        kd_loss = KL_divergence(teacher_probs_Ei, student_probs_Ei)

        # Compute CE loss for new entity types
        ce_loss = cross_entropy(labels, student_probs_Enew)

        # Combined loss
        total_loss = alpha * kd_loss + beta * ce_loss

        # Backward pass
        total_loss.backward()

        # Update student model parameters
        optimizer.step()
```

## ExtendNER模型

## 架构和师生设置

ExtendNER模型扩展了输出层的维度，以适应新的实体类型，而不是添加新的输出层。论文相当简单地解释了这些维度应如何设置：

“假设*Mi*能够识别*n*个实体类型，它的最终层可以被视为一个*h×(2n+1)*的矩阵。然后，*Mi+1*的输出层将扩展为一个*h × (2n + 2m + 1)*的矩阵，以适应新的实体类型。”

![](../Images/e04816628dc4e50a569db1c30a59c8d1.png)

[ExtendNER模型的示意图](https://ojs.aaai.org/index.php/AAAI/article/view/17600)来自第3.4节

## 前向传播

与AddNER相同，但维度扩展。

## 后向传播

损失计算使用*KL散度损失*或交叉熵损失，具体取决于以下因素：

+   当NER类别标签*y*为“O”（来自IOB标注模式）时，使用KL散度损失。

+   当类别标签*y*不是“O”时，使用交叉熵损失。

## 最终预测

Viterbi算法用于解码最终的实体类型。

AddNER和ExtendNER模型在持续学习中表现良好，结果之间差异不大

# 4\. 基于回放的方法

## “微调的语言模型是持续学习者”

[论文链接](https://arxiv.org/pdf/2205.12393.pdf)

论文中的模型不是像GPT这样仅用于对话回应的通用单任务模型。相反，它是为一系列专业任务进行微调的，从文本简化到俳句生成。这些任务各自有独特的要求、评估标准和专业训练数据集。

研究人员将旧数据集的部分与新数据集混合，通过在微调新任务时只混入1%的旧任务数据集，取得了很好的效果。这是在多个任务上顺序进行的（8）。该模型在零-shot学习设置中也表现良好，意味着它可以很好地推广到未经过训练的任务。例如，当给定一个未见过的主题时，它可以生成具有正确音节数的俳句，显示出其推广能力。研究人员还提到，他们的方法是任务顺序不变的，意味着学习任务的顺序不会影响模型的表现。实验发现，与新数据集混合的旧数据集的数量不会显著影响主要任务的表现。然而，它会影响零-shot学习。在0%复习时，模型倾向于忘记零-shot任务，而在1%复习时，模型在这些任务中的表现保持得很好。

这些都显得积极，我们可以只添加1%的旧数据集并解决持续学习的问题，但当然，将其应用于像chatGPT这样的聊天机器人，将是经验性的，并且可能完全不同。即使假设chatGPT可以在微调和RLHF阶段持续训练，也需要大量标记的对话数据。

# 5\. 基于架构的方法

我不会在这里详细讨论具体的论文或实现，但我会简要概述这种方法和几种不同的技术。我建议阅读[这篇综合调查论文](https://arxiv.org/pdf/2302.00487.pdf)的第4.5节。它比其他章节更容易阅读。

1.  参数分配：在这里，网络参数的一个子集被分配给每个任务。这可以通过屏蔽无关的神经元或明确识别当前任务的重要神经元来实现。

1.  模块化网络：这涉及为每个任务使用独立的子网络或模块。

子网络可以以各种方式连接，以形成一个集成体或更复杂的架构。以下是几种常见的子网络连接方法：

## 输出的连接：

在这种方法中，多个子网络的输出被串联成一个张量，然后可以通过额外的层处理以生成最终输出。

## 投票机制：

在某些模型中，每个子网络对可能的结果进行“投票”，最终决定通过多数投票或加权投票来做出。这有生物学启发，因为它类似于新皮层中不同皮层柱进行投票的方式。

## 跳跃连接：

一些架构允许子网络具有跳跃连接到模型的其他部分，从而允许信息在模块之间流动。

## 顺序：

在这种情况下，一个子网络的输出作为下一个子网络的输入。

回到聊天机器人，如果能创建一个由两个子网络组成的架构，我觉得特别有趣。第一个是预训练模型，它包含通用的“知识”。第二个包含对齐模型所需的知识。一旦模型对齐，它将不再需要标记的对话数据。相反，它可以通过以无监督的方式训练预训练子网络来不断更新。

# **结论**

总之，深度学习中持续学习的子领域具有挑战性，并且大部分仍未被了解。这是因为我们尚未完全理解大规模语言模型中的神经元如何工作，正如引言中所述，当前的网络架构或深度学习本身可能不适合此任务。

我注意到上个月 ChatGPT（仅 GPT-4）已经[更新](https://stackdiary.com/chatgpts-cutoff-date-upgraded-to-january-2022/)，现在它显示“自我的训练截止日期是 2022 年 1 月”，所以我想知道 OpenAI 的团队是如何实现这一点的。

![](../Images/1746b4e381fc5d968f5a8d60d20a46a6.png)

ChatGPT（GPT-4 变体）告知用户其训练数据截至到 2022 年 1 月（作者截图）
