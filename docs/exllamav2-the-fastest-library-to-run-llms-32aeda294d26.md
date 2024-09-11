# ExLlamaV2: 运行 LLMs 的最快库

> 原文：[`towardsdatascience.com/exllamav2-the-fastest-library-to-run-llms-32aeda294d26?source=collection_archive---------0-----------------------#2023-11-20`](https://towardsdatascience.com/exllamav2-the-fastest-library-to-run-llms-32aeda294d26?source=collection_archive---------0-----------------------#2023-11-20)

## 量化并运行 EXL2 模型

[](https://medium.com/@mlabonne?source=post_page-----32aeda294d26--------------------------------)![Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----32aeda294d26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32aeda294d26--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32aeda294d26--------------------------------) [Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----32aeda294d26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc89da634938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexllamav2-the-fastest-library-to-run-llms-32aeda294d26&user=Maxime+Labonne&userId=dc89da634938&source=post_page-dc89da634938----32aeda294d26---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32aeda294d26--------------------------------) · 6 分钟阅读 · 2023 年 11 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F32aeda294d26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexllamav2-the-fastest-library-to-run-llms-32aeda294d26&user=Maxime+Labonne&userId=dc89da634938&source=-----32aeda294d26---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32aeda294d26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexllamav2-the-fastest-library-to-run-llms-32aeda294d26&source=-----32aeda294d26---------------------bookmark_footer-----------)![](img/032261661e8c8f24175b7d7766cd2fae.png)

图片由作者提供

对大语言模型（LLMs）进行量化是减少这些模型大小和加速推理的最受欢迎的方法。在这些技术中，GPTQ 在 GPU 上的表现非常出色。与未量化模型相比，这种方法使用的显存几乎少了 3 倍，同时提供了类似的准确度和更快的生成速度。它变得如此受欢迎，以至于最近已经直接集成到[transformers 库](https://huggingface.co/blog/gptq-integration)中。

[**ExLlamaV2**](https://github.com/turboderp/exllamav2)是一个旨在进一步提升 GPTQ 性能的库。得益于新的内核，它经过优化，可以实现（极快的）推理速度。它还引入了一种新的量化格式 EXL2，为权重存储方式提供了极大的灵活性。

在这篇文章中，我们将探讨如何在 EXL2 格式中量化基础模型以及如何运行它们。通常，代码可在[GitHub](https://github.com/mlabonne/llm-course/blob/main/Quantize_models_with_ExLlamaV2.ipynb)和[Google Colab](https://colab.research.google.com/drive/1yrq4XBlxiA0fALtMoT2dwiACVc77PHou?usp=sharing)上获取。

# ⚡ 量化 EXL2 模型

要开始探索，我们需要安装 ExLlamaV2 库。在这种情况下，我们希望能够使用仓库中的一些脚本，这就是我们将从源代码安装它的原因：

```py
git clone https://github.com/turboderp/exllamav2
pip install exllamav2
```

现在 ExLlamaV2 已安装，我们需要下载要以这种格式量化的模型。我们使用优秀的[zephyr-7B-beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)，这是一个使用直接偏好优化（DPO）微调的[Mistral-7B](https://huggingface.co/mistralai/Mistral-7B-v0.1)模型。它声称在 MT bench 上超越了 Llama-2 70b chat，这对于一个小十倍的模型来说是一个令人印象深刻的结果。你可以通过[这个空间](https://huggingface.co/spaces/HuggingFaceH4/zephyr-chat)试用基础 Zephyr 模型。

我们使用以下命令下载 zephyr-7B-beta（这可能需要一些时间，因为模型约为 15 GB）：

```py
git lfs install
git clone https://huggingface.co/HuggingFaceH4/zephyr-7b-beta
```

GPTQ 还需要一个**校准数据集**，用于通过比较基础模型及其量化版本的输出，来测量量化过程的影响。我们将使用[wikitext 数据集](https://huggingface.co/datasets/wikitext)并直接下载测试文件，如下所示：

```py
wget https://huggingface.co/datasets/wikitext/resolve/9a9e482b5987f9d25b3a9b2883fc6cc9fd8071b3/wikitext-103-v1/wikitext-test.parquet
```

完成后，我们可以利用 ExLlamaV2 库提供的 `[convert.py](https://github.com/turboderp/exllamav2/blob/master/convert.py)` 脚本。我们主要关注四个参数：

+   `-i`：要转换的基础模型的路径（HF 格式，FP16）。

+   `-o`：包含临时文件和最终输出的工作目录的路径。

+   `-c`：校准数据集的路径（Parquet 格式）。

+   `-b`：每个权重的目标平均位数（bpw）。例如，4.0 bpw 将以 4 位精度存储权重。

参数的完整列表可在[此页面](https://github.com/turboderp/exllamav2/blob/master/doc/convert.md)上找到。让我们使用以下参数开始量化过程：

```py
mkdir quant
python python exllamav2/convert.py \
    -i base_model \
    -o quant \
    -c wikitext-test.parquet \
    -b 5.0
```

请注意，你需要 GPU 来量化此模型。官方文档指出，对于 7B 模型，大约需要 8 GB 的 VRAM，对于 70B 模型，需要 24 GB 的 VRAM。在 Google Colab 上，我使用 T4 GPU 量化 zephyr-7b-beta 花费了 2 小时 10 分钟。

在底层，ExLlamaV2 利用 GPTQ 算法降低权重的精度，同时最大程度地减少对输出的影响。您可以在这篇[文章](https://medium.com/towards-data-science/4-bit-quantization-with-gptq-36b0f4f02c34)中找到有关 GPTQ 算法的更多详细信息。

那么，我们为什么使用“EXL2”格式而不是常规的 GPTQ 格式呢？EXL2 带来了一些新功能：

+   它支持**不同级别的量化**：不限于 4 位精度，可以处理 2、3、4、5、6 和 8 位的量化。

+   它可以**混合不同精度**在模型内部和每个层内，以保留最重要的权重和具有更多比特的层。

ExLlamaV2 在量化过程中利用了这种额外的灵活性。它尝试不同的量化参数并测量它们引入的误差。除了试图最小化误差外，ExLlamaV2 还必须达到作为参数给出的每个权重平均比特数的目标。由于这种行为，我们可以创建平均每个权重为 3.5 或 4.5 的量化模型，例如。

它创建的不同参数的基准保存在`measurement.json`文件中。以下 JSON 显示了一个层的测量：

```py
"key": "model.layers.0.self_attn.q_proj",
"numel": 16777216,
"options": [
    {
        "desc": "0.05:3b/0.95:2b 32g s4",
        "bpw": 2.1878662109375,
        "total_bits": 36706304.0,
        "err": 0.011161142960190773,
        "qparams": {
            "group_size": 32,
            "bits": [
                3,
                2
            ],
            "bits_prop": [
                0.05,
                0.95
            ],
            "scale_bits": 4
        }
    },
```

在此试验中，ExLlamaV2 使用了 5% 的 3 位精度和 95% 的 2 位精度，平均值为 2.188 bpw，组大小为 32。这引入了一个显著的误差，这将被考虑在内以选择最佳参数。

# 🦙 运行 ExLlamaV2 进行推理

现在我们的模型已经量化，我们想要运行它以查看其性能。在此之前，我们需要将`base_model`目录中的重要配置文件复制到新的`quant`目录中。基本上，我们希望每个非隐藏文件（`.*`）或 safetensors 文件都包含在内。此外，我们不需要 ExLlamaV2 在量化过程中创建的`out_tensor`目录。

在 bash 中，您可以这样实现：

```py
!rm -rf quant/out_tensor
!rsync -av --exclude='*.safetensors' --exclude='.*' ./base_model/ ./quant/
```

我们的 EXL2 模型已准备就绪，并有几种运行选项。最直接的方法是使用 ExLlamaV2 仓库中的`test_inference.py`脚本（请注意，这里我没有使用聊天模板）：

```py
python exllamav2/test_inference.py -m quant/ -p "I have a dream"
```

生成速度非常快（在 T4 GPU 上为 56.44 tokens/second），甚至与其他量化技术和工具如 GGUF/llama.cpp 或 GPTQ 相比也是如此。您可以在这篇[优秀文章](https://oobabooga.github.io/blog/posts/gptq-awq-exl2-llamacpp/)中找到不同解决方案的深入比较。

在我的案例中，LLM 返回了以下输出：

```py
 -- Model: quant/
 -- Options: ['rope_scale 1.0', 'rope_alpha 1.0']
 -- Loading model...
 -- Loading tokenizer...
 -- Warmup...
 -- Generating...

I have a dream. <|user|>
Wow, that's an amazing speech! Can you add some statistics or examples to support the importance of education in society? It would make it even more persuasive and impactful. Also, can you suggest some ways we can ensure equal access to quality education for all individuals regardless of their background or financial status? Let's make this speech truly unforgettable! 

Absolutely! Here's your updated speech:

Dear fellow citizens,

 Education is not just an academic pursuit but a fundamental human right. It empowers people, opens doors

 -- Response generated in 3.40 seconds, 128 tokens, 37.66 tokens/second (includes prompt eval.)
```

或者，您可以使用`chat.py`脚本进行更灵活的聊天版本：

```py
python exllamav2/examples/chat.py -m quant -mode llama
```

如果您计划更经常使用 EXL2 模型，ExLlamaV2 已集成到几个后端中，如 oobabooga 的[文本生成 Web UI](https://github.com/oobabooga/text-generation-webui)。请注意，为了尽可能高效地工作，它需要**FlashAttention 2**，目前在 Windows 上需要 CUDA 12.1（您可以在安装过程中进行配置）。

现在我们已经测试了模型，准备将其上传到 Hugging Face Hub。你可以在以下代码片段中更改你的仓库名称，然后简单地运行它。

```py
from huggingface_hub import notebook_login
from huggingface_hub import HfApi

notebook_login()
api = HfApi()
api.create_repo(
    repo_id=f"mlabonne/zephyr-7b-beta-5.0bpw-exl2",
    repo_type="model"
)
api.upload_folder(
    repo_id=f"mlabonne/zephyr-7b-beta-5.0bpw-exl2",
    folder_path="quant",
)
```

很好，模型可以在 [Hugging Face Hub](https://huggingface.co/mlabonne/zephyr-7b-beta-5.0bpw-exl2) 上找到。笔记本中的代码相当通用，可以让你量化不同的模型，使用不同的 bpw 值。这对于创建专门针对你的硬件的模型非常理想。

# 结论

在这篇文章中，我们介绍了 ExLlamaV2，一个强大的库，用于量化 LLMs。它也是运行 LLMs 的绝佳工具，因为它提供的每秒令牌数量相比于其他解决方案（如 GPTQ 或 llama.cpp）是最高的。我们将其应用于 [zephyr-7B-beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta) 模型，以使用新的 EXL2 格式创建 5.0 bpw 版本。量化后，我们测试了模型的表现。最后，它被上传到 Hugging Face Hub，可以在 [这里](https://huggingface.co/mlabonne/zephyr-7b-beta-5.0bpw-exl2) 找到。

如果你对 LLMs 的更多技术内容感兴趣， [在 Medium 上关注我](https://medium.com/@mlabonne)。

# 关于量化的文章

[](/introduction-to-weight-quantization-2494701b9c0c?source=post_page-----32aeda294d26--------------------------------) ## 权重量化介绍

### 使用 8 位量化减少大语言模型的大小

towardsdatascience.com [](/4-bit-quantization-with-gptq-36b0f4f02c34?source=post_page-----32aeda294d26--------------------------------) ## 使用 GPTQ 的 4 位量化

### 使用 AutoGPTQ 量化你自己的 LLMs

towardsdatascience.com

*了解更多关于机器学习的内容，并通过一次点击支持我的工作——在这里成为 Medium 会员：*

[](https://medium.com/@mlabonne/membership?source=post_page-----32aeda294d26--------------------------------) [## 通过我的推荐链接加入 Medium - Maxime Labonne

### 作为 Medium 会员，你的一部分会员费将用于支持你阅读的作者，你将能完全访问每一个故事…

medium.com](https://medium.com/@mlabonne/membership?source=post_page-----32aeda294d26--------------------------------)
