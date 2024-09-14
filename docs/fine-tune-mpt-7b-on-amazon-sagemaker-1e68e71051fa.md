# 在 Amazon SageMaker 上微调 MPT-7B

> 原文：[`towardsdatascience.com/fine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa`](https://towardsdatascience.com/fine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa)

![](img/2f5691f2830e17d1af6c907a144f4567.png)

照片由 [Jeffery Ho](https://unsplash.com/@jefferyho?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

## 学习如何准备数据集并创建一个训练任务，以便在 Amazon SageMaker 上微调 MPT-7B。

[](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)![João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------) [João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------) ·阅读时间 9 分钟·2023 年 6 月 20 日

--

每周都会有新的大型语言模型（LLMs）被宣布，每个模型都试图超越其前任并占据评估排行榜的首位。其中最新的模型之一是 [MPT-7B](https://www.mosaicml.com/blog/mpt-7b)，由 MosaicML 发布。与同类其他模型不同，这款 70 亿参数的模型是开源的，并且获得了商业使用的许可证 ([Apache 2.0 许可证](https://github.com/mosaicml/llm-foundry/blob/main/LICENSE)) 🚀。

像 MPT-7B 这样的基础模型是在具有万亿个标记（100 个标记约等于 75 个单词）的数据集上进行预训练的，并且当提示得当时，它们能够生成令人印象深刻的输出。然而，要真正释放大型语言模型在实际应用中的价值，仅仅智能的提示工程可能不足以使其适用于你的用例，因此，需要在领域特定的数据集上对基础模型进行微调。

大型语言模型（LLMs）具有数十亿个参数，因此微调如此庞大的模型是具有挑战性的。好消息是，相比于预训练基础模型，微调的成本更低、速度更快，因为 1) 领域特定的数据集是“较小”的，2) 微调只需对训练数据进行少量遍历。

***在本文中我们将学习：***

+   如何创建和构建用于微调大型语言模型的数据集。

+   什么是完全分片数据并行的分布式训练作业以及如何配置它。

+   如何定义一个 😊 HuggingFace 估算器。

+   如何在 Amazon SageMaker 中启动一个微调 MPT-7B 的训练作业。

# 1\. 安装依赖项并设置 S3 路径

让我们首先安装 [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk) 和其他一些包。这个 SDK 使得在 AWS 上训练和部署机器学习模型变得可能，只需几行 Python 代码。下面的代码可以在 Github 的 `[sagemaker_finetuning.ipynb](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b/blob/main/sagemaker_finetuning.ipynb)` notebook 中找到。在 SageMaker Studio、SageMaker notebook 实例或在笔记本电脑上运行 notebook，前提是 [认证到 AWS 账户](https://docs.aws.amazon.com/signin/latest/userguide/command-line-sign-in.html)。

```py
!pip install "sagemaker==2.162.0" s3path boto3 --quiet

from sagemaker.huggingface import HuggingFace
from sagemaker.inputs import TrainingInput
from sagemaker import s3_utils
import sagemaker
import boto3
import json
```

下一步是定义数据将保存到 S3 的路径，并创建一个 SageMaker 会话。

```py
# Define S3 paths
bucket             = "<YOUR-S3-BUCKET>"
training_data_path = f"s3://{bucket}/toy_data/train/data.jsonl"
test_data_path     = f"s3://{bucket}/toy_data/test/data.jsonl"
output_path        = f"s3://{bucket}/outputs"
code_location      = f"s3://{bucket}/code"

# Create SageMaker session
sagemaker_session  = sagemaker.Session()
region             = sagemaker_session.boto_region_name
role               = sagemaker.get_execution_role()
```

# 2\. 构建微调数据集

我们将创建一个虚拟数据集来演示如何微调 MPT-7B。由于在完整数据集上训练此大小的模型需要较长时间且成本较高，因此首先在小数据集上测试和调试训练任务是一个好主意，其次将训练规模扩大到完整数据集。

+   **将数据集格式化为字典列表** — 数据集应格式化为字典列表，每个示例具有键值结构，*例如*，

```py
{
    "prompt": "What is a Pastel de Nata?",
    "response": "A Pastel de Nata is a Portuguese egg custard tart pastry, optionally dusted with cinnamon."
}
```

`prompt` 是给模型的输入（*例如*，一个问题）。`response` 是模型训练预测的输出（*例如*，`prompt` 中问题的答案）。原始提示通常会经过预处理，以适应提示模板，这有助于模型生成更好的输出。请注意，模型是为因果语言建模训练的，因此可以把它看作是一个“文档完成器”。设计提示模板时，最好让模型觉得它正在完成一个文档。Andrej Karpathy 在他的演讲 [*State of GPT*](https://youtu.be/bZQun8Y4L2A) 中很好地解释了这一机制。

```py
prompt_template = """Write a response that appropriately answers the question below.
### Question:
{question}

### Response:
"""

dataset = [
    {"prompt": "What is a Pastel de Nata?",
     "response": "A Pastel de Nata is a Portuguese egg custard tart pastry, optionally dusted with cinnamon."},
    {"prompt": "Which museums are famous in Amsterdam?",
     "response": "Amsterdam is home to various world-famous museums, and no trip to the city is complete without stopping by the Rijksmuseum, Van Gogh Museum, or Stedelijk Museum."},
    {"prompt": "Where is the European Parliament?",
     "response": "Strasbourg is the official seat of the European Parliament."},
    {"prompt": "How is the weather in The Netherlands?",
     "response": "The Netherlands is a country that boasts a typical maritime climate with mild summers and cold winters."},
    {"prompt": "What are Poffertjes?",
     "response": "Poffertjes are a traditional Dutch batter treat. Resembling small, fluffy pancakes, they are made with yeast and buckwheat flour."},
]

# Format prompt based on template
for example in dataset:
    example["prompt"] = prompt_template.format(question=example["prompt"])

training_data, test_data = dataset[0:4], dataset[4:]

print(f"Size of training data: {len(training_data)}\nSize of test data: {len(test_data)}")
```

+   **将训练和测试数据上传到 S3** — 一旦训练和测试集准备好并格式化为字典列表，我们将使用下面的工具函数将它们作为 JSON 行上传到 S3：

```py
def write_jsonlines_to_s3(data, s3_path):
    """Writes list of dictionaries as a JSON lines file to S3"""

    json_string = ""
    for d in data:
        json_string += json.dumps(d) + "\n"

    s3_client   = boto3.client("s3")

    bucket, key = s3_utils.parse_s3_url(s3_path)
    s3_client.put_object(
         Body   = json_string,
         Bucket = bucket,
         Key    = key,
    )

write_jsonlines_to_s3(training_data, training_data_path)
write_jsonlines_to_s3(test_data, test_data_path)
```

# 3\. SageMaker 训练任务

使用 S3 中可用的数据集，我们现在将在 Amazon SageMaker 中创建一个训练任务。为此，我们需要创建一个入口点脚本，修改配置文件以指定训练设置，并定义一个 HuggingFace 估算器。我们将（重新）使用来自 [LLM Foundry](https://github.com/mosaicml/llm-foundry) 和 [Composer](https://github.com/mosaicml/composer) 库的 CLI 启动器，这些启动器设置了分布式训练环境。这两个包均由 [MosaicML](https://www.mosaicml.com/) 维护，该公司是 MPT-7B 的背后公司。工作文件夹应按如下结构组织：

```py
└── **fine-tune-mpt-7b-sagemaker**/
    ├── training_script_launcher.sh
    ├── fine_tuning_config.yaml
    ├── sagemaker_finetuning.ipynb
```

我们现在将深入了解这些文件中的每一个。

+   **创建一个配置文件** `[**finetuning_config.yaml**](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b/blob/main/finetuning_config.yaml)`— [LLM Foundry](https://github.com/mosaicml/llm-foundry) 仓库中提供的模板是一个很好的起点，特别是 `[mpt-7b-dolly-sft.yaml](https://github.com/mosaicml/llm-foundry/blob/main/scripts/train/yamls/finetune/mpt-7b_dolly_sft.yaml)` 文件。然而，根据数据集大小和训练实例，您可能需要调整一些配置，例如批量大小。我已经修改了该文件以在 SageMaker 中微调模型（请查看 `[finetuning_config.yaml](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b/blob/main/finetuning_config.yaml)`）。您需要关注的参数如下：

```py
max_seq_len: 512
global_seed: 17

...
# Dataloaders
train_loader:
  name: finetuning
  dataset:
    hf_name: json
    hf_kwargs:
        data_dir: /opt/ml/input/data/train/
...

eval_loader:
  name: finetuning
  dataset:
    hf_name: json
    hf_kwargs:
        data_dir: /opt/ml/input/data/test/

...
max_duration: 3ep
eval_interval: 1ep
...
global_train_batch_size: 128

...
# FSDP
fsdp_config:
  sharding_strategy: FULL_SHARD
  mixed_precision: PURE
  activation_checkpointing: true
  activation_checkpointing_reentrant: false
  activation_cpu_offload: false
  limit_all_gathers: true
  verbose: false

# Checkpoint to local filesystem or remote object store
save_folder: /tmp/checkpoints
dist_timeout: 2000
```

`max_seq_length` 指示输入的最大令牌数（记住 100 个令牌约等于 75 个单词）。训练和测试数据将使用 😊 [Datasets](https://huggingface.co/docs/datasets/index) 库从容器内与训练任务关联的 `/opt/ml/input/data/{train, test}` 目录加载。查看 [SageMaker Training Storage Folders](https://docs.aws.amazon.com/sagemaker/latest/dg/model-train-storage.html) 文档，以了解容器目录的结构。`max_duration` 指定微调的轮数。两到三轮通常是一个不错的选择。`eval_interval` 指示模型在测试集上的评估频率。

分布式训练策略是 Fully Sharded Data Parallel (FSDP)，这使得像 MPT-7B 这样的巨大模型的训练变得高效。与传统的数据并行策略不同，后者在每个 GPU 中保留模型副本，FSDP 将模型参数、优化器状态和梯度在数据并行工作者之间进行分片。如果您想深入了解 FSDP，可以查看这篇有见地的 [PyTorch 介绍文章](https://pytorch.org/blog/introducing-pytorch-fully-sharded-data-parallel-api/)。FSDP 已集成在 [Composer](https://github.com/mosaicml/composer) 中，这是 [LLM Foundry](https://github.com/mosaicml/llm-foundry) 使用的分布式训练库。

`save_folder` 确定模型检查点（`.pt` 文件）保存的位置。我们将其设置为临时文件夹 `/tmp/checkpoints`。

+   **创建入口脚本** `[**launcher.sh**](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b/blob/main/launcher.sh)`— 一个 bash 脚本作为入口点。这个 bash 脚本克隆了 LLM Foundry 仓库，安装了所需的依赖，并且更重要的是，使用 Composer 库的分布式启动器运行训练脚本。请注意，通常在 SageMaker 中，训练作业是通过像 `python train.py` 这样的命令运行训练脚本。然而，在我们的场景中，可以将 bash 脚本作为入口点，这提供了更多的灵活性。最后，我们将保存到 `/tmp/checkpoints` 的模型检查点转换为 HuggingFace 模型格式，并将最终的工件保存到 `/opt/ml/model/`。SageMaker 会压缩此目录中的所有文件，创建一个 tarball `model.tar.gz`，并将其上传到 S3。这个 tarball 对于推理非常有用。

```py
# Clone llm-foundry package from MosaicML
# This is where the training script is hosted
git clone https://github.com/mosaicml/llm-foundry.git
cd llm-foundry

# Install required packages
pip install -e ".[gpu]"
pip install git+https://github.com/mosaicml/composer.git@dev

# Run training script with fine-tuning configuration
composer scripts/train/train.py /opt/ml/code/finetuning_config.yaml

# Convert Composer checkpoint to HuggingFace model format
python scripts/inference/convert_composer_to_hf.py \
    --composer_path /tmp/checkpoints/latest-rank0.pt \
    --hf_output_path /opt/ml/model/hf_fine_tuned_model \
    --output_precision bf16

# Print content of the model artifact directory
ls /opt/ml/model/
```

+   **定义 😊 HuggingFace 估算器** — 估算器设置了用于运行训练作业的 Docker 容器。我们将使用一个带有 PyTorch 2.0.0 和 Python 3.10 的镜像。bash 脚本和配置文件会自动上传到 S3，并在容器内可用（由 SageMaker Python SDK 处理）。我们将训练实例设置为`[g5.48xlarge](https://aws.amazon.com/ec2/instance-types/g5/)`，它配备了 8x NVIDIA A10G GPU。`[p4d.24xlarge](https://aws.amazon.com/ec2/instance-types/p4/)` 也是一个不错的选择。虽然它更贵，但配备了 8x NVIDIA A100 GPU。我们还指明了要在训练集和测试集中跟踪的指标（交叉熵和困惑度）。这些指标的值通过正则表达式捕获并发送到 Amazon CloudWatch。

```py
# Define container image for the training job
training_image_uri = f"763104351884.dkr.ecr.{region}.amazonaws.com/huggingface-pytorch-training:2.0.0-transformers4.28.1-gpu-py310-cu118-ubuntu20.04-v1.1"

# Define metrics to send to CloudWatch
metrics = [
    # On training set
    {"Name": "train:LanguageCrossEntropy",
     "Regex": "Train metrics\/train\/LanguageCrossEntropy: ([+-]?((\d+\.?\d*)|(\.\d+)))"},
    {"Name": "train:LanguagePerplexity",
     "Regex": "Train metrics\/train\/LanguagePerplexity: ([+-]?((\d+\.?\d*)|(\.\d+)))"},
    # On test set
    {"Name": "test:LanguageCrossEntropy",
     "Regex": "Eval metrics\/eval\/LanguageCrossEntropy: ([+-]?((\d+\.?\d*)|(\.\d+)))"},
    {"Name": "test:LanguagePerplexity",
     "Regex": "Eval metrics\/eval\/LanguagePerplexity: ([+-]?((\d+\.?\d*)|(\.\d+)))"},
]

estimator_args = {
    "image_uri": training_image_uri,     # Training container image
    "entry_point": "launcher.sh",        # Launcher bash script
    "source_dir": ".",                   # Directory with launcher script and configuration file
    "instance_type": "ml.g5.48xlarge",   # Instance type
    "instance_count": 1,                 # Number of training instances
    "base_job_name": "fine-tune-mpt-7b", # Prefix of the training job name
    "role": role,                        # IAM role
    "volume_size": 300,                  # Size of the EBS volume attached to the instance (GB)
    "py_version": "py310",               # Python version
    "metric_definitions": metrics,       # Metrics to track
    "output_path": output_path,          # S3 location where the model artifact will be uploaded
    "code_location": code_location,      # S3 location where the source code will be saved
    "disable_profiler": True,            # Do not create profiler instance
    "keep_alive_period_in_seconds": 240, # Enable Warm Pools while experimenting
}

huggingface_estimator = HuggingFace(**estimator_args)
```

> ⚠️ 确保请求 SageMaker 训练的相应配额，并在使用此酷炫功能时请求 [热池](https://docs.aws.amazon.com/sagemaker/latest/dg/train-warm-pools.html) 的配额。如果你计划在 SageMaker 中运行多个作业，可以查看 [SageMaker 节省计划](https://aws.amazon.com/savingsplans/ml-pricing/)。

+   **启动训练作业 🚀** — 我们已准备好在 Amazon SageMaker 上开始训练作业：

```py
huggingface_estimator.fit({
    "train": TrainingInput(
        s3_data=training_data_path,
        content_type="application/jsonlines"),
    "test": TrainingInput(
        s3_data=test_data_path,
        content_type="application/jsonlines"),
}, wait=True)
```

训练时间将取决于数据集的大小。使用我们的虚拟数据集，训练大约需要 **20min** 完成。一旦模型训练完成并转换为 😊 HuggingFace 格式，SageMaker 将把模型 tarball (`model.tar.gz`) 上传到 S3 `output_path`。我发现实际上上传步骤花费的时间较长（>1h），这可能是由于模型工件压缩的大小（~25GB）。

# 4\. 总结

在本文中，我展示了如何准备数据集并在 SageMaker 中创建训练任务，以微调 MPT-7B 以适应你的使用案例。该实现利用了来自 [LLM Foundry](https://github.com/mosaicml/llm-foundry) 的训练脚本，并使用了 [Composer](https://github.com/mosaicml/composer) 库的分布式训练启动器。一旦你微调了你的模型并想要部署它，我建议查看 [Philipp Schmid 的博客文章](https://www.philschmid.de/)；那里有很多关于如何在 SageMaker 中部署 LLM 的示例。祝你玩得愉快，享受你的微调 MPT-7B 模型！🎉

本文中使用的所有代码都可以在 Github 上找到：

[## GitHub - jpcpereira/sagemaker-fine-tune-mpt-7b](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b?source=post_page-----1e68e71051fa--------------------------------)

[github.com](https://github.com/jpcpereira/sagemaker-fine-tune-mpt-7b?source=post_page-----1e68e71051fa--------------------------------)

— 乔昂·佩雷拉

*感谢阅读。希望本文能帮助你入门在 Amazon SageMaker 中微调大型语言模型如 MPT-7B。如果你想阅读我未来的文章，请* [*关注我*](https://medium.com/@joao.pereira.abt/subscribe)*。非常感谢反馈！如果你有任何问题，请在下面留言，或直接通过* ***电子邮件*** *或在* [***LinkedIn***](https://www.linkedin.com/in/jpcpereira/)*联系我。*
