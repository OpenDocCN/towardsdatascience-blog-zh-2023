# 在CPU上使用Hugging Face Pipelines运行Falcon推断

> 原文：[https://towardsdatascience.com/running-falcon-on-a-cpu-with-hugging-face-pipelines-b60b3b8a32a3?source=collection_archive---------2-----------------------#2023-06-06](https://towardsdatascience.com/running-falcon-on-a-cpu-with-hugging-face-pipelines-b60b3b8a32a3?source=collection_archive---------2-----------------------#2023-06-06)

![](../Images/344b0c93c3824c6cec55f3db8be1c23a.png)

[图片来源](https://www.freepik.com/free-photo/wild-eagle-standing-majestic-scene-generative-ai_40949970.htm#query=falcon&position=0&from_view=search&track=sph)

## 了解如何在第4代Xeon CPU上使用Hugging Face Pipelines运行7亿和40亿Falcon模型

[](https://eduand-alvarez.medium.com/?source=post_page-----b60b3b8a32a3--------------------------------)[![爱德华多·阿尔瓦雷斯](../Images/afa0ad855c8ec2e977ebbe60dc3e77a4.png)](https://eduand-alvarez.medium.com/?source=post_page-----b60b3b8a32a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b60b3b8a32a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b60b3b8a32a3--------------------------------) [爱德华多·阿尔瓦雷斯](https://eduand-alvarez.medium.com/?source=post_page-----b60b3b8a32a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe49cc416a8ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-falcon-on-a-cpu-with-hugging-face-pipelines-b60b3b8a32a3&user=Eduardo+Alvarez&userId=e49cc416a8ef&source=post_page-e49cc416a8ef----b60b3b8a32a3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b60b3b8a32a3--------------------------------) ·5分钟阅读·2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb60b3b8a32a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-falcon-on-a-cpu-with-hugging-face-pipelines-b60b3b8a32a3&user=Eduardo+Alvarez&userId=e49cc416a8ef&source=-----b60b3b8a32a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb60b3b8a32a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-falcon-on-a-cpu-with-hugging-face-pipelines-b60b3b8a32a3&source=-----b60b3b8a32a3---------------------bookmark_footer-----------)

很容易认为我们只能通过 GPU 执行由数十亿参数构成的 LLM 推理。虽然 GPU 在深度学习中相较于 CPU 提供了显著的加速，但硬件的选择应始终基于具体的使用案例。例如，假设你的终端用户只需要每 30 秒得到一次响应。如果你在经济和后勤上都很困难去预留能在 < 30 秒内给出答案的加速器，那么你可能会遇到收益递减的问题。

![](../Images/11d61d846e9add1d0566bd508e52052a.png)

图 1\. 从终端用户到硬件和软件堆栈的反向思考——像“计算意识的 AI 开发者”一样思考——图像由作者提供

这一切都回到一个基本原则，即成为一个“计算意识的 AI 开发者”——从应用程序的目标反向推导出使用的正确软件和硬件。想象一下开始一个家庭项目，比如挂一个新架子，却直接使用大锤，而没有考虑到一个更小、更精确的锤子可能更适合这个项目。

在本文中，我们将使用 Hugging Face [Pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines) 在 [4th Generation Xeon CPU](https://www.intel.com/content/www/us/en/newsroom/news/4th-gen-xeon-scalable-processors-max-series-cpus-gpus.html#gs.zlhfe8) 上对 [Falcon-7b](https://huggingface.co/tiiuae/falcon-7b) 和 [Falcon-40b](https://huggingface.co/tiiuae/falcon-40b) 进行推理。Falcon-40b 是由阿布扎比技术创新研究院 (TII) 开发的一个 40 亿参数的解码器模型。它在多个模型如 LLaMA、StableLM、RedPajama 和 MPT 上表现优越，利用 FlashAttention 方法实现更快和优化的推理，显著提高了在不同任务中的速度。

# 环境设置

一旦访问到你的 Xeon 计算实例，你必须确保有足够的存储空间来下载 Falcon 的检查点和模型碎片。如果你想同时测试 7 亿和 40 亿的 Falcon 版本，我们建议至少确保 150 GB 的存储空间。你还必须提供足够的 RAM 以将模型加载到内存中，并提供足够的核心以高效运行工作负载。我们成功地在 [Intel Developer Cloud](https://bit.ly/3Fewcto) 的 32 核心 64GB RAM 虚拟机（第 4 代 Xeon）上运行了 7 亿和 40 亿 Falcon 版本。然而，这只是众多有效计算规格中的一种，进一步的测试可能会提高性能。

1.  安装 miniconda。你可以在他们的网站找到最新版本：[https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

1.  创建一个 conda 环境 `conda create -n falcon python==3.8.10`

1.  安装依赖项 `pip install -r requirements.txt`。你可以在下面找到 requirements.txt 文件的内容。

```py
transformers==4.29.2
torch==2.0.1
accelerate==0.19.0
einops==0.6.1

# requirements.txt
```

4\. 激活你的 conda 环境 `conda activate falcon`

# 使用 Hugging Face Pipelines 运行 Falcon

[Hugging Face](https://www.intel.com/content/www/us/en/developer/partner/hugging-face.html) 管道提供了一个简单而高级的接口，用于将预训练模型应用于各种自然语言处理（NLP）任务，如文本分类、命名实体识别、文本生成等。这些管道抽象了模型加载、分词和推理的复杂性，使用户能够仅用几行代码快速利用最先进的NLP模型。

以下是一个方便的脚本，你可以在 cmd/terminal 中运行它来试验原始的预训练 Falcon 模型。

```py
from transformers import AutoTokenizer, AutoModelForCausalLM
import transformers
import torch
import argparse
import time

def main(FLAGS):

    model = f"tiiuae/falcon-{FLAGS.falcon_version}"

    tokenizer = AutoTokenizer.from_pretrained(model, trust_remote_code=True)

    generator = transformers.pipeline(
        "text-generation",
        model=model,
        tokenizer=tokenizer,
        torch_dtype=torch.bfloat16,
        trust_remote_code=True,
        device_map="auto",
    )

    user_input = "start"

    while user_input != "stop":

        user_input = input(f"Provide Input to {model} parameter Falcon (not tuned): ")

        start = time.time()

        if user_input != "stop":
            sequences = generator( 
            f""" {user_input}""",
            max_length=FLAGS.max_length,
            do_sample=False,
            top_k=FLAGS.top_k,
            num_return_sequences=1,
            eos_token_id=tokenizer.eos_token_id,)

        inference_time = time.time() - start

        for seq in sequences:
         print(f"Result: {seq['generated_text']}")

        print(f'Total Inference Time: {inference_time} seconds')

if __name__ == "__main__":
    parser = argparse.ArgumentParser()

    parser.add_argument('-fv',
                        '--falcon_version',
                        type=str,
                        default="7b",
                        help="select 7b or 40b version of falcon")
    parser.add_argument('-ml',
                        '--max_length',
                        type=int,
                        default="25",
                        help="used to control the maximum length of the generated text in text generation tasks")
    parser.add_argument('-tk',
                        '--top_k',
                        type=int,
                        default="5",
                        help="specifies the number of highest probability tokens to consider at each step")

    FLAGS = parser.parse_args()
    main(FLAGS)

# falcon-demo.py
```

要运行脚本（falcon-demo.py），你必须提供脚本和各种参数：

`python falcon-demo.py --falcon_version "7b" --max_length 25 --top_k 5`

该脚本有 3 个可选参数，帮助控制 Hugging Face 管道的执行：

+   **falcon_version:** 允许你从 Falcon 的 70 亿或 400 亿参数版本中进行选择。

+   **max_length:** 用于控制文本生成任务中生成文本的最大长度。

+   **top_k:** 指定在每一步中考虑的最高概率标记数量。

你可以修改脚本以添加/删除/编辑参数。重要的是，你现在可以访问到有史以来最强大的开源模型之一！

## 玩转原始 Falcon

原始 Falcon 并未针对任何特定目的进行调整，因此可能会输出无意义的内容（图 2）。不过，这并不妨碍我们提出一些问题来进行测试。当脚本完成模型下载和创建管道后，你将被提示向模型提供输入。当你准备停止时，键入“stop”。

![](../Images/8825f4b73d5939d9eca8153326a8ffd3.png)

图 2\. 在 Intel 第四代 Xeon 上使用默认脚本参数对 70 亿参数 Falcon 模型进行命令行接口推理测试 — 图片由作者提供

脚本会打印推理时间，让你了解模型在当前参数设置和你为该工作负载提供的计算资源下响应的时间。

*提示：通过调整 max_length 参数，你可以显著改变推理时间。*

本教程旨在分享如何在 CPU 上通过 Hugging Face Transformers 运行 Falcon，但不探索 Intel CPUs 上进一步优化的选项。像 [Intel Extension for Transformers](https://github.com/intel/intel-extension-for-transformers) 这样的库提供了通过量化、蒸馏和剪枝等技术加速基于 Transformer 的模型的能力。量化是一种广泛使用的模型压缩技术，可以减少模型大小并提高推理延迟 — 这将是探索提升此工作流性能的宝贵下一步。

# 总结与讨论

基础的 LLMs 为开发者创造了构建令人兴奋的 AI 应用的机会。然而，通常一半的挑战是找到一个允许商业衍生的正确许可的模型。Falcon 提供了一个难得的机会，因为它兼具性能和许可证灵活性。

尽管 Falcon 从开源的角度来看相当民主化，但其规模给工程师/爱好者带来了新的挑战。本教程通过结合 Falcon 的“真正开放”许可证、Hugging Face Pipelines 和 CPU 的可用性/可及性，帮助解决了这些问题，使开发者可以更多地访问这个强大的模型。

**尝试一些令人兴奋的事情包括：**

+   通过利用 [Intel Extension for PyTorch](https://www.intel.com/content/www/us/en/developer/tools/oneapi/optimization-for-pytorch.html#gs.0ugs0o) 将 Falcon 微调到特定任务。

+   使用 [Intel Neural Compressor (INC)](https://www.intel.com/content/www/us/en/developer/tools/oneapi/neural-compressor.html#gs.zm5zqx) 和 [Intel Extension for Transformers](https://github.com/intel/intel-extension-for-transformers) 中提供的模型压缩工具。

+   调整 Hugging Face 管道的参数，以优化性能以适应你的特定用例。

***别忘了关注*** [***我的个人资料以获取更多类似的文章***](https://eduand-alvarez.medium.com/) ***！***
