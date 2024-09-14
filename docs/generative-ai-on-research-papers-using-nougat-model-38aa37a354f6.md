# 使用 Nougat 模型进行研究论文生成 AI

> 原文：[`towardsdatascience.com/generative-ai-on-research-papers-using-nougat-model-38aa37a354f6`](https://towardsdatascience.com/generative-ai-on-research-papers-using-nougat-model-38aa37a354f6)

## 用数据做有趣的事情！

[](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)![Priya Dwivedi](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------) [Priya Dwivedi](https://priya-dwivedi.medium.com/?source=post_page-----38aa37a354f6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38aa37a354f6--------------------------------) ·阅读时间 9 分钟·2023 年 9 月 20 日

--

![](img/286ebca3250788f30b4f28ba9521ee43.png)

图片由 [Dan Dimmock](https://unsplash.com/@dandimmock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/3mt71MKGjQ0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

最近，大型语言模型（LLMs）如 GPT-4 的进展展示了生成连贯文本的令人印象深刻的能力。然而，准确解析和理解研究论文仍然是 AI 面临的极具挑战的任务。研究论文包含复杂的格式、数学方程式、表格、图形以及特定领域的语言。信息的密度非常高，重要的语义通过格式编码。

在本文中，我将演示如何使用 Meta 的新模型 [Nougat](https://facebookresearch.github.io/nougat/) 准确解析研究论文。然后，我们将其与一个 LLM 流水线相结合，提取并总结论文中的所有表格。

这里的潜力是巨大的。许多数据/信息被锁定在未被正确解析的研究论文和书籍中。准确的解析使这些数据能够在包括 LLM 重新训练在内的许多不同应用中使用。

我制作了一段 YouTube 视频，更详细地解释了代码和我的实验。请查看 [这里](https://youtu.be/rYxaijVlc-A)。

# Nougat 模型

Nougat 是 Meta AI 研究人员开发的一种视觉变换器模型，可以将文档页面的图像转换为结构化文本 [1]。它将文档页面的光栅图像作为输入，并输出轻量级标记语言的文本。

Nougat 的主要优势在于它仅依赖于文档图像，而不需要任何 OCR 文本。这使得它能够正确恢复诸如数学方程之类的语义结构。它在来自 arXiv 和 PubMed 的数百万篇学术论文上进行训练，以学习研究论文格式和语言的模式。

[1] 中的下图展示了 PDF 中编写的数学方程如何在 Latex 中再现并正确渲染。

![](img/b798654ff35b41252906bef0aa8850f6.png)

来源：Nougat 论文中的图 5 — [`arxiv.org/pdf/2308.13418.pdf`](https://arxiv.org/pdf/2308.13418.pdf)

Nougat 使用视觉变换器编码器-解码器架构。编码器使用 Swin Transformer 将文档图像编码为潜在嵌入。Swin Transformer 通过使用移位窗口以分层方式处理图像。然后，解码器利用自注意力机制根据编码器输出自回归生成输出文本标记。

Nougat 使用随机梯度下降对页面图像和文本对进行端到端训练。数据增强技术如侵蚀、膨胀和弹性变换被用于提高鲁棒性。在训练过程中，还使用了特殊的反重复正则化来减少文本重复。

作者报告了如编辑距离、BLEU、METEOR 和 F1 分数等指标，基于学术论文的测试集。Nougat 在纯文本上表现出高准确度。错误主要集中在数学表达式上，因为 LaTeX 命令存在歧义。如下面所示，Nougat 在性能上显著超过了 GROBID 和 PDF OCR 等早期方法。

![](img/c352652b5748afe1deb43e6d6d0fab22.png)

来源：表 1 Nougat 论文 — [`arxiv.org/pdf/2308.13418.pdf`](https://arxiv.org/pdf/2308.13418.pdf)

# 运行和评估 Nougat 模型的输出

安装说明和运行 Nougat 模型的步骤在 Colab Notebook [这里](https://colab.research.google.com/drive/1oC7jK8UMEYRDAEPevn5VQweadjN4WyeW?usp=sharing)中共享。

Nougat 小型模型在免费的 Colab T4 GPU 上运行顺畅，结果非常令人印象深刻。

运行模型的主要步骤是安装包 `nougat-ocr`。然后我们使用子进程运行命令行 CLI，将下载的 PDF 传递给 Nougat 并获取其输出。

```py
!pip install -qq nougat-ocr

def nougat_ocr(file_name):

  # Command to run
  cli_command = [
      'nougat',
      '--out', 'output',
      'pdf', file_name,
      '--checkpoint', CHECKPOINT,
      '--markdown'
  ]

  # Run the command
  subprocess.run(cli_command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

  return
```

在我的 Colab Notebook 中，我在[**DOLA 论文**](https://arxiv.org/pdf/2309.03883.pdf)上运行了 Nougat。Nougat 令人印象深刻地捕捉了 Latex 中的数学方程。

```py
Recent language models are consists of an embedding layer, \(N\) stacked transformer layers, and an affine layer \(\phi(\cdot)\) for predicting the next-word distributution. Given a sequence of tokens \(\{x_{1},x_{2},\ldots,x_{t-1}\}\), the embedding layer first embeds the tokens into a sequence of vectors \(H_{0}=\{h_{1}^{(0)},\ldots,h_{t-1}^{(0)}\}\). Then \(H_{0}\) would be processed by each of the transformer layers successively. We denote the output of the \(j\)-th layer as \(H_{j}\). Then, the vocabulary head \(\phi(\cdot)\) predicts the probability of the next token \(x_{t}\)

\[p(x_{t}\mid x_{<t})=\mathrm{softmax}\big{(}\phi(h_{t}^{N})\big{)}_{x_{t}}, \quad x_{t}\in\mathcal{X},\]

where \(\mathcal{X}\) is the vocabulary set.

Instead of applying \(\phi\) just on the final layer, our approach contrasts the higher-layer and lower-layer information to obtain the probability of next token. More specifically, for the lower layers, we also compute the probability of the next tokens using \(\phi(\cdot)\),

\[q_{j}(x_{t}\mid x_{<t})=\mathrm{softmax}\big{(}\phi(h_{t}^{j})\big{)}_{x_{t}}, \quad j=1,\ldots,N.\]

The idea of applying language heads directly to the hidden states of the middle layers, known as _early exit_(Teerapittayanon et al., 2016; Elbayad et al., 2020; Schuster et al., 2022), has proven to be an effective inference method even without special training process (Kao et al., 2020), as the residual connections (He et al., 2016) in transformer layers make the hidden representations gradually evolve without abrupt changes. Using \(q_{j}(x_{t})\) to represent \(q_{j}(x_{t}\mid x_{<t})\) for notational brevity, we then compute the probability of the next token by,

\[\hat{p}(x_{t}\mid x_{<t}) =\mathrm{softmax}\big{(}\mathcal{F}\big{(}q_{N}(x_{t}),q_{M}(x_{t })\big{)}\big{)}_{x_{t}}, \tag{1}\] \[\text{where}\quad M =\operatorname*{arg\,max}_{j\in\mathcal{J}}\;d\big{(}q_{N}(\cdot ),q_{j}(\cdot)\big{)}.\]

Here, layer \(M\) is referred to as the _premature layer_, while the final layer is referred to as the _mature layer_. The operator \(\mathcal{F}(\cdot,\cdot)\), to be elaborated further in Section 2.3, is used to contrast between the output distributions from the premature layer and the mature layer by computing the difference between two distributions in the log domain. The premature layer is dynamically selected in each decoding step using a distributional distance measure \(d(\cdot,\cdot)\) (we use the Jensen-Shannon Divergence) between the mature layer and all the candidate layers in \(\mathcal{J}\). We discuss \(d(\cdot,\cdot)\) in more detail in Section 2.1 and Section 2.2\. The motivation for selecting the layer with the highest distance \(d(\cdot,\cdot)\) as the premature layer is to maximize the difference between the mature/premature layers.
```

我还发现表格在 Latex 中捕捉得很好。表 1 被准确地转换为 Latex，如下所示！

```py
\begin{table}
\begin{tabular}{l c c c c c} \hline \hline \multirow{2}{*}{**Model**} & \multicolumn{4}{c}{**TruthfulQA**} & \multicolumn{2}{c}{**FACTOR**} \\ \cline{2-6}  & **MC1** & **MC2** & **MC3** & **News** & **Wiki** \\ \hline LLaMa-7B & 25.6 & 40.6 & 19.2 & 58.3 & 58.6 \\ + ITI (Li et al., 2023) & 25.9 & - & - & - & - \\ + DoLa & **32.2** & **63.8** & **32.1** & **62.0** & **62.2** \\ \hline LLaMa-13B & 28.3 & 43.3 & 20.8 & 61.1 & 62.6 \\ + CD (Li et al., 2022) & 24.4 & 41.0 & 19.0 & 62.3 & 64.4 \\ + DoLa & **28.9** & **64.9** & **34.8** & **62.5** & **66.2** \\ \hline LLaMa-33B & 31.7 & 49.5 & 24.2 & 63.8 & 69.5 \\ + CD (Li et al., 2022) & **33.0** & 51.8 & 25.7 & 63.3 & **71.3** \\ + DoLa & 30.5 & **62.3** & **34.0** & **65.4** & 70.3 \\ \hline LLaMa-65B & 30.8 & 46.9 & 22.7 & 63.6 & 72.2 \\ + CD (Li et al., 2022) & 29.3 & 47.0 & 21.5 & 64.6 & 71.3 \\ + DoLa & **31.1** & **64.6** & **34.3** & **66.2** & **72.4** \\ \hline \hline \end{tabular}
\end{table}
Table 1: Multiple choices results on the TruthfulQA and FACTOR.
```

# 生成 LLM 总结 Latex 表格

有许多示例展示了 LLM 总结文本，包括研究论文。但几乎总是，表格和数学方程中的数据要么解析不正确，要么未包含在总结中。我现在测试一下像 GPT3.5 这样的模型在 Latex 表格上的表现如何。

我在这里使用 Langchain 编写了一个链式流水线，执行以下操作：

1.  识别研究论文中的所有表格

1.  读取 Latex 并理解表格

1.  创建一个输出字典，总结每个表格

完整的源代码包含在 Colab Notebook 中[这里](https://colab.research.google.com/drive/1oC7jK8UMEYRDAEPevn5VQweadjN4WyeW?usp=sharing)。如下面的代码片段所示，我们对 LLM 的提示是识别所有表格，总结它们，并以结构化格式分享结果。

```py
from langchain.prompts import (
    ChatPromptTemplate,
    PromptTemplate,
    SystemMessagePromptTemplate,
    AIMessagePromptTemplate,
    HumanMessagePromptTemplate,
)

from langchain.chat_models import ChatOpenAI
from langchain.chains import LLMChain

context_template="You are a helpful AI Researcher that specializes in analysing research paper outputs presented to you in Latex"

system_message_prompt = SystemMessagePromptTemplate.from_template(context_template)

human_template= """
Please extract all tables referenced in this paper. The tables are in Latex format. Summarize the tables one by one. 
Each summary should be 4-5 sentences long. Include numbers in summary where you can. Make a dictionary with table number, table name and summary of that table. 

 PAPER: {paper_content}

 """

human_message_prompt = HumanMessagePromptTemplate(
            prompt=PromptTemplate(
            template=human_template,
            input_variables=["paper_content"],))

chat_prompt_template = ChatPromptTemplate.from_messages([system_message_prompt,
                                                         human_message_prompt])

chat = ChatOpenAI(model_name="gpt-3.5-turbo-16k",
                  temperature=0.2)

summary_chain = LLMChain(llm=chat, prompt=chat_prompt_template)

output = summary_chain.run(noref_content)

pprint.pprint(output)
```

我之前对 LLM 的表现不太确定，所以不得不说，我对回复的质量感到震惊。论文中有 6 个表格，所有表格都被正确识别了。然后每个表格都被很好地总结了。

对上述提示的 LLM 回应如下。

```py
('Dictionary of Tables:\n'
 '\n'
 'Table 1:\n'
 '- Table Number: 1\n'
 '- Table Name: Multiple choices results on the TruthfulQA and FACTOR '
 'datasets\n'
 '- Summary: This table presents the results of multiple choice tasks on the '
 'TruthfulQA and FACTOR datasets. It compares the performance of different '
 'models, including the baseline, Inference Time Intervention (ITI), and DoLa. '
 'The table shows that DoLa consistently outperforms the other methods, '
 'improving the truthfulness and informativeness scores.\n'
 '\n'
 'Table 2:\n'
 '- Table Number: 2\n'
 '- Table Name: Open-ended generation results on TruthfulQA, StrategyQA, and '
 'GSM8K\n'
 '- Summary: This table summarizes the results of open-ended generation tasks '
 'on the TruthfulQA, StrategyQA, and GSM8K datasets. It compares the '
 'performance of different models, including the baseline, Contrastive '
 'Decoding (CD), and DoLa. The table shows that DoLa consistently enhances the '
 'truthfulness and informativeness scores, outperforming the other methods.\n'
 '\n'
 'Table 3:\n'
 '- Table Number: 3\n'
 '- Table Name: Multiple choices results on the FACTOR dataset\n'
 '- Summary: This table presents the results of multiple choice tasks on the '
 'FACTOR dataset. It compares the performance of different models, including '
 'the baseline, DoLa with dynamic premature layer selection, and DoLa with '
 'random layer selection. The table shows that DoLa with dynamic premature '
 'layer selection performs better than the other methods, improving the '
 'truthfulness and informativeness scores.\n'
 '\n'
 'Table 4:\n'
 '- Table Number: 4\n'
 '- Table Name: Comparison of MPT-7B and modifications on TruthfulQA, FACTOR, '
 'and CoT datasets\n'
 '- Summary: This table compares the performance of the MPT-7B model and its '
 'modifications on the TruthfulQA, FACTOR, and CoT datasets. It shows that '
 'DoLa improves the truthfulness and truthfulness+informativeness scores on '
 'most datasets, indicating the potential of DoLa to generalize across '
 'different transformer models.\n'
 '\n'
 'Table 5:\n'
 '- Table Number: 5\n'
 '- Table Name: Qualitative study for LLaMA-33B on TruthfulQA\n'
 '- Summary: This table presents qualitative examples from the TruthfulQA '
 'dataset, comparing the answers generated by the baseline and DoLa using the '
 'LLaMA-33B model. It shows that DoLa produces more truthful and informative '
 'answers compared to the baseline.\n'
 '\n'
 'Table 6:\n'
 '- Table Number: 6\n'
 '- Table Name: Averaged decoding latency per token in milliseconds\n'
 '- Summary: This table shows the average decoding latency per token in '
 'milliseconds for the baseline and DoLa. It indicates that DoLa adds a small '
 'additional latency to the decoding process, making it a practical and '
 'efficient decoding strategy.')
```

阅读这些示例时，我的观察是：

+   每个表格都被识别出来了。模型准确理解了表格的行和列

+   总结捕捉了表格中的结论

+   总结表明，模型使用了论文中的信息来理解表格。对于表格 3，模型准确理解了该表格比较了 DOLA 与随机层选择与过早层选择，即使表格本身并没有提到这一点

+   我本来希望在表格的总结中看到更多的数字和统计数据，但模型没有输出它们。附带说明，我还测试了 Anthropic.AI 的 Claude2，它能够总结表格，并提及关键的统计结论，呈现了更优的回复

我的代码现在还涵盖了上述流程的一个示例，搜索所有数学方程并解释它们。这里也有令人印象深刻的初步结果。

# 结论

在这篇文章中，我展示了 Nougat 模型如何直接从图像中解析复杂的研究论文。Nougat 克服了一个关键挑战——理解论文格式中编码的语义。

我展示了一个端到端的流程，通过 Nougat 解析论文，提取表格，并使用 LLMs 总结它们。Nougat 以高准确率提取了文本、数学、表格和格式。下游的 LLM 成功总结了提取的表格，专注于关键比较。

Nougat 帮助克服了 PDF 文本和当前最佳解决方案如 pyPDF 和 GROBID 的局限性。我们现在可以更好地理解论文中的信息。总体而言，这展示了生成性 AI 模型如何在解锁所有困于非结构化格式中的知识方面取得进展。

我经营自己的机器学习咨询，帮助客户进行生成性 AI 应用。如果你有兴趣合作，请通过 priа@deeplearninganalytics.org 给我发邮件。

# 参考文献

[1] Blecher, Lukas, et al. “Nougat: Neural Optical Understanding for Academic Documents.” arXiv preprint arXiv:2308.13418 (2023).

[2] Langchain Source Documentation — [`python.langchain.com/docs/get_started/introduction`](https://python.langchain.com/docs/get_started/introduction)
